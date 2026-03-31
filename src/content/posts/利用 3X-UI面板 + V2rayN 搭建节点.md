---
title: '利用 3X-UI面板 + V2rayN 搭建节点'
published: 2026-02-25
draft: false
description: '利用 3X-UI面板 + V2rayN 搭建节点'
tags: ['3X-UI', 'VPS', 'V2rayN']
---

---

## 免责声明

**仅供交流学习使用！**
使用者在使用时，必须遵守当地法律和规定。  
使用者有责任确保他们的行为符合其所在地区的法律、规章以及其他适用的规定。  
如果你不清楚相关法律法规，请勿使用本教程中的方法搭建节点，以免触犯法律。

## 项目概述

[3X-UI](https://github.com/MHSanaei/3x-ui)是一个基于网页的高级开源控制面板，专为管理 Xray-core 服务器而设计。它提供了用户友好的界面，用于配置和监控各种 VPN 和代理协议。作为原始 X-UI 项目的增强版本，3X-UI 提供了更好的稳定性、更广泛的协议支持和额外的功能。

# 一、准备工作

## 1.一台境外VPS，系统主要为 Debian/Ubuntu/CentOS

- 有的推荐[Racknerd](https://www.racknerd.com/的vps)、[HostDare](https://bill.hostdare.com/aff.php?aff=4226)、[搬瓦工](https://bandwagonhost.com/aff.php?aff=78545)
- 目前我使用的是[云悠 YUNYOO](https://yunyoo.cc/)，系统使用 Debian 12
## 2. 下载并安装SSH工具

- 相关工具很多，有的推荐[FinalShell](https://www.hostbuf.com/)
- 我使用的是[XShell 8](https://www.xshell.com/zh/free-for-home-school/)
## 3. 解析到 [Cloudflare](https://dash.cloudflare.com/) 域名一个（非必需）

* 国内域名注册商：腾讯云、阿里云、华为云等
	* 优缺点：相对便宜，但需要实名
* 国外域名注册商：name、GoDaddy、Spaceship、Porkbun等
	* 优缺点：注册简单、无需实名，支付方式可能麻烦，部分支持支付宝
* 可以去这个网站[比较所有顶级域名的价格](https://zh-hans.tld-list.com/)
- 目前我使用的是[Spaceship](https://www.spaceship.com/)
## 4. v2rayN 或者其他相关软件亦可

* [v2rayN](https://github.com/2dust/v2rayN)（windows端）
* [v2rayNG](https://github.com/2dust/v2rayNG)（安卓端）

# 二、搭建步骤

## 1. 安装更新运行环境

* 下面环境的安装方式，根据自己的系统选择命令进行安装

### 1.1. Debian/Ubuntu系统（二选一）

```bash
apt update -y && apt install curl wget -y
apt update -y && apt install -y curl && apt install -y socat
```

### 1.2. CentOS系统（二选一）

```bash
yum update -y && yum install curl wget -y
yum update -y && yum update -y && yum install -y socat
```

## 2. 安装`3X-UI`面板

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
```

## 3. 设置用户名和密码，还有端口号

* 3XUI会自动生成用户名、密码和登录面板路径，记得保存
* 登录面板路径一般为`http://IP:端口/路径`

## 4. 添加`入站节点`以及导入规则

### 4.1. `Vless + tcp + reality`

* 点击 `入站列表` ⇒ `添加入站`
* 备注：自己填，协议：`vless`，端口：自己填（或者默认）
* 点击`客户`一栏，`电子邮件`可留空（默认也可）
* `传输` 选择 `TCP(RAM)` （这个是默认的，不用改）
* `安全` 选择 `Reality`，点击最下面`Get New Cert`按钮获得公钥和私钥，其他选项可不改
* 往上翻 `客户` 一栏中出现 `flow` 选项，选择 `xtls-rprx-vision` 选项（或者`xtls-rprx-vision-udp443`）
* `Sniffing` 点击`开启`（非必选）
* 最后点击`创建`即可

### 4.2 导入规则

在`入站列表`中找到建立的站点，点击菜单（三点），选择`导出连接`，复制并打开`v2rayN`软件，右键粘贴即可，可以测试一下`真连接延迟`

## 5.如果需要域名

### 5.1 购买域名

- **后缀选择**：`.com` 最稳，`.top` / `.xyz` 最便宜
- **解析托管**：购买后立即将 **NS (Name Server)** 修改为 **Cloudflare** 的地址
- 具体如何托管可查看[如何把域名托管到Cloudflare](https://www.youtube.com/watch?v=x_-76QVuC3c)
### 5.2 购买域名并托管完成后

#### 5.2.1 在 Cloudflare 添加 A 记录

在 Cloudflare 的 **DNS 记录** 页面，点击 **“添加记录”**：
- **类型 (Type)：** `A`
- **名称 (Name)：** `@` 或 `panel`
- **IPv4 地址：** **VPS 的 IP**
- **代理状态 (Proxy status)：** **务必暂时关闭（显示为灰色云朵）**
    - _注：开启云朵会干扰证书申请，等证书申请成功后再决定是否开启。_
#### 5.2.2 申请证书（以 Debian 12 + XShell 为例）

登录 Xshell，执行下面这串组合命令。
它会自动安装工具、申请证书并设置自动续期。
请务必替换命令中的邮箱地址和域名为你自己的信息：
```bash
# 1. 安装必要的验证工具
apt update && apt install socat -y

# 2. 安装 acme.sh 脚本（请把 my@example.com 换成你的邮箱）
curl https://get.acme.sh | sh -s email=my@example.com
source ~/.bashrc

# 3. 申请证书（将 xxx.com 换成你在第一步设定的域名）
# 如果你第一步 Name 填的是 @，就用 xxx.com
# 如果你第一步 Name 填的是 panel，就用 panel.xxx.com
acme.sh --issue -d xxx.com --standalone
```

如果显示报错如下：
```bash
It is recommended to install crontab first. Try to install 'cron', 'crontab', 'crontabs' or 'vixie-cron'.

We need to set a cron job to renew the certs automatically.

Otherwise, your certs will not be able to be renewed automatically.

Please add '--force' and try install again to go without crontab.

./acme.sh --install --force

Pre-check failed, cannot install.
```

是因为因为 **Debian 12** 的精简版镜像通常不会预装 `cron`，我们需要手动补上：
```bash
apt update && apt install cron -y
```

安装完后，需要让它跑起来并保证服务器重启后它也能自动运行：
```bash
systemctl enable --now cron
```

继续执行安装 `acme.sh` 脚本
#### 5.2.3 存放证书

申请成功后，证书在隐藏目录下。我们需要把它拷贝出来，让 3X-UI 面板能读到它：

```bash
# 创建一个专门放证书的文件夹
mkdir -p /etc/cert

# 安装证书到该文件夹（记得替换域名）
acme.sh --install-cert -d xxx.com \
--key-file /etc/cert/key.pem \
--fullchain-file /etc/cert/cert.pem
```

#### 5.2.4 在 3X-UI 面板开启 HTTPS

- 打开 3X-UI 面板（现在还是 `http://IP:端口/路径`）
- 点击左侧的 **“面板设置” (Panel Settings)**
- 找到以下两个路径框，准确填入：
    - **面板证书公钥路径 (Public Key Path)：** `/etc/cert/cert.pem`
    - **面板证书密钥路径 (Private Key Path)：** `/etc/cert/key.pem`
- **保存配置**，然后点击 **“重启面板”**
- 重启后，原来的 HTTP 链接就失效了，现在需要使用：**`https://你的域名:端口/你的安全路径`**
- 证书申请成功后，可以去 Cloudflare 把那个橙色的**小云朵点亮**，开启后 VPS IP 会被隐藏，但对于**移动用户**，延迟可能会增加

## 6. 注意事项：

### 6.1 节点搭建

**“协议+传输+安全”** 主要有三种常用的组合：
- **VLESS + TCP + Reality**
- **VLESS + xhttp + Reality**
- **VMess + WebSocket(WS) + TLS**

### 核心配置对比表

| **特性**        | **VLESS + TCP + Reality** | **VLESS + xhttp + Reality** | **VMess + WebSocket(WS) + TLS** |
| ------------- | ------------------------- | --------------------------- | ------------------------------- |
| **传输效率**      | **极高 (直通车)**              | 中等 (模拟请求)                   | 较低 (多次封装)                       |
| **延迟 (Ping)** | **最低**                    | 较低                          | 较高                              |
| **抗封锁性**      | 极强 (借用真实域名)               | **最强 (行为模拟)**               | 中等 (依赖 CDN 续命)                  |
| **配置难度**      | 简单 (无需域名)                 | 简单 (无需域名)                   | 复杂 (需域名+证书)                     |
| **首选场景**      | **视频观影/日常使用**             | 极端严密封锁环境                    | 救急/IP 被封后套 CDN                  |

#### 6.1.1 VLESS + TCP + Reality

这是目前跨海链路（尤其是美西到中国）的性能标杆。

- **工作原理：** 它直接在 TCP 握手阶段通过 Reality 技术“借用”了微软等大厂的身份，让流量看起来就像你在正常访问海外大厂官网。
- **适用场景：** **重度 YouTube 4K/8K 视频用户。**
- **为什么选它：** 它的头部开销极小。

#### 6.1.2 VLESS + xhttp + Reality

这是 Xray 内核近期推出的“黑科技”，可以看作是 `h2`（HTTP/2）传输方式的进化版。

- **工作原理：** 它将数据伪装成更符合现代互联网标准的 **HTTP 请求/流** 行为。相比 TCP，它的行为模式更像是在真实的网页交互。
- **适用场景：** **网络环境极其恶劣或经常被精准识别。**
- **为什么选它：** 某些地区运营商会对长连接进行干扰。如果发现 `vless+tcp` 偶尔断流或速度不稳定，切换到 `xhttp` 能通过更精细的流量伪装避开审查。但对于 512MB 内存的vps，它的 CPU 消耗会略高于 TCP。

#### 6.1.3 VMess + WebSocket (WS) + TLS

这是几年前的流行方案，但在 2026 年的今天，除非有特殊需求，否则不再推荐作为首选。

- **工作原理：** 先把数据包封装成 WebSocket 网页协议，再套一层 TLS 证书加密。
- **适用场景：** **IP 地址被墙后的“起死回生”。**
- **为什么不推荐：**
    - **效率极低：** 数据包被套了太多层，就像穿着棉袄游泳，速度和延迟都会大打折扣，看 4K 视频会明显感觉到起步慢。
    - **CDN 依赖：** 这种方案唯一的价值是可以搭配 **Cloudflare CDN**。如果你的 VPS IP 被封了，你可以通过 CDN 转发流量来绕过，但这会显著增加延迟（从 150ms 变成 300ms+）。

### 6.2 VPS 购买

#### 6.2.1 线路决定连接质量

对于跨境连接，**物理距离**只是参考，**路由优先级**才是决定晚高峰是否卡顿的关键。
关注 **“三网优化”“回国优化”** 等词。

- **精品网三剑客：**
    - **CN2 GIA (电信精品)：** 电信用户的“救命稻草”，负载低且极其稳定。
    - **CMIN2 (移动精品)：** 移动用户的顶级选择，是抗干扰能力最强的回国线路之一。
    - **AS9929 (联通精品)：** 联通的王牌线路，带宽大且延迟低。
- **普通线路 (BGP/163)：** 标称 1Gbps 但价格极低（如 $20/年以下）的机器，通常在晚高峰会因为出口拥堵而出现严重丢包。
- **双程优化：** 务必确认是“去程和回程”都经过优化，有些机器仅回程优化，去程绕路会导致握手延迟极高。

#### 6.2.2 带宽与流量的“数字游戏”

- **带宽上限：** 观看 YouTube 4K 视频，建议选择 **30Mbps** 以上的实测带宽。
- **计费模式：**
    - **双向计费：** 意味着服务器下载 1G 数据再发给你 1G，总流量算 2G。这在购买小流量套餐（如 400G-600G）时需要特别留意。
    - **带宽限速：** 优先选择“大带宽+有限流量”的套餐，而非“小带宽（如 5M/10M）+无限流量”的套餐，因为后者无法支撑高清视频需求。

#### 6.2.3 硬件性能的“及格线”

虽然 VPN 主要是网络任务，但协议的加密解密依然消耗硬件资源：

- **CPU：** 至少 **1 vCore**。如果你使用复杂的协议（如 Reality + Vision），2 核 CPU 能提供更稳的延迟控制。
- **内存 (RAM)：**
    - **512MB：** 极简主义者的极限。需要开启虚拟内存（Swap）并精简系统服务。
    - **1GB 及以上：** 推荐配置。能顺畅运行 **Debian 12**、3X-UI 面板、Xray 内核及各种自动化脚本。
- **系统版本：** 强烈建议选择最新的稳定版（如 **Debian 12**），因为它内置的内核版本较高，支持直接开启 **BBR** 加速。

#### 6.2.4 协议兼容性与抗封锁能力

在配置入站规则时，务必考虑协议的隐蔽性：

- **首选协议：** **VLESS + Reality + TCP + Vision**。这能通过模拟真实网站流量（如微软或苹果官网）来规避 GFW 的主动探测。
- **流控 (Flow)：** 开启 `xtls-rprx-vision`。这是防止流量被精准识别的核心防护层。
- **流量探测 (Sniffing)：** 务必开启并勾选 `HTTP` 和 `TLS`，这对于后续精准分流（如解锁 Netflix）至关重要。

#### 6.2.5 IP 质量与附加价值

- **流媒体解锁：** 数据中心 IP 往往会被 Netflix 等平台列入黑名单。如果需要看全区视频，通常需要配合 **Cloudflare Warp** 进行 IP 欺骗。
- **退款政策：** 优先选择支持“24小时内不满意退款”或“按时计费（Pay-as-you-go）”的商家，防止买到被封 IP 后血本无归。
- **地理位置：** 
	* **香港/日本：** 适合追求极低延迟的网页交互。
    - **美西（洛杉矶/圣何塞）：** 适合追求大带宽和极致性价比的视频体验。

### 6.3 域名采购

- **续费陷阱**：务必查看“续费价格”而非仅看“首年优惠价”。
- **隐私保护**：确认商家提供免费的 **WHOIS Privacy**，防止个人电话和邮箱被爬虫抓取。
- **DNSSEC 冲突**：在迁移 DNS 路径时，若发现解析不通，检查并删除旧注册商处残留的 **DS 记录**。
### 6.4 证书申请

- **禁止带“云”申请**：开启 Cloudflare CDN 代理（橙色云朵）时申请证书，往往会因验证包被拦截而失败。
- **禁止忽略 Cron**：如果不装 `cron`，你的证书就是一个“一次性产品”，到期即废。
- **禁止泄露路径**：HTTPS 登录后的“安全路径”（Secret Path）要像密码一样保管，它是防止扫描器撞库的最后一道防线。
### 6.5 如果遇到在添加证书后访问3X-UI面板失效的问题

原因是 **Cloudflare 代理（小云朵）** 开启后，默认只转发 443 端口，不识别你的 **3X-UI 自定义高位端口**，可以在关闭 Cloudflare 代理（小云朵）后成功访问面板。
或者采取**配置 Origin Rules (回源规则)**：将外部 443 请求在后台自动重定向到 VPS 的真实端口。具体步骤如下：

- 打开 Cloudflare 账户主页，在左侧栏找到`规则`一项，选择`概述`，找到`Origin Rules`，预览模板会开启一个默认的模板
- 匹配字段改为“主机名”等于 `你的域名`，并**点亮 DNS 页面中的橙色小云朵**，部署规则即可
- 更改完之后，即可使用`https://你的域名/你的安全路径`直接访问面板，不在需要端口

# 三、参考资料

1. [3X-UI面板](https://www.luopojunzi.com/p/X3-UI/)
2. [如何自建翻墙节点？](https://www.youtube.com/watch?v=MuWTmEiNe1g)
3. [利用vps实现科学上网](https://northfourta.github.io/2022/03/14/%E5%88%A9%E7%94%A8vps%E5%AE%9E%E7%8E%B0%E7%A7%91%E5%AD%A6%E4%B8%8A%E7%BD%91/)
4. [安装 3X-UI 搭建自己的节点](https://bwgjmsjc.com/gemini-%e6%97%a0%e6%b3%95%e4%bd%bf%e7%94%a8/) 
5. [自建节点被墙怎么办？](https://www.youtube.com/watch?v=NnyG3DyZbN8)
6. [cloudflare 中的Origin Rules这个菜单怎么找不到了？](https://www.nodeloc.com/t/topic/39951/2)
7. 部分步骤在[Gemini](https://gemini.google.com/app)帮助下完成