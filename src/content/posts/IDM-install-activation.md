---
title: IDM 安装激活完整教程
published: 2026-04-15
draft: false
description: ""
tags:
  - idm
---
---

# # IDM 安装激活教程

## 下载 IDM 
从IDM官网下载最新的官方安装包：[Internet Download Manager](https://www.internetdownloadmanager.com/)

## 激活 IDM
通过脚本激活：[IDM-Activation-Script](https://github.com/Astro-Saurav/IDM-Activation-Script/blob/main/README.md)
1. 打开`PowerShell`，输入：
	```bash
		 irm "https://bit.ly/idm_Activate" | iex
	```
	或者：
	```bash
	 irm "https://raw.githubusercontent.com/Astro-Saurav/IDM-Activation-Script/main/IAS.ps1" | iex
	```
2. 后续按照脚本选项操作即可