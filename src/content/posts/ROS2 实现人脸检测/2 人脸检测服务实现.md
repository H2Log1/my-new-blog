---
title: ROS2 实现人脸检测（三）：人脸检测服务实现
published: 2026-03-22
draft: false
description: 本文介绍了如何在ROS2中实现人脸检测的服务
tags: ['ROS2', '人脸检测']
---

---

在 `demo_python_service/demo_python_service` 下新建 `face_detect_node.py ` 文件：
```python
import rclpy
from rclpy.node import Node
from face_interfaces.srv import FaceDetector
import face_recognition
import cv2
from ament_index_python.packages import get_package_share_directory
import os
from cv_bridge import CvBridge
import time

class FaceDetectNode(Node):
    def __init__(self, node_name):
        super().__init__(node_name)
        self.service = self.create_service(
        	FaceDetector, 
        	"face_detect", 
        	self.detect_face_callback)
        self.bridge = CvBridge()
        self.number_of_times_to_upsample = 1
        self.model = "hog"
        self.defaut_image_path = os.path.join(
        	get_package_share_directory("demo_python_service"), 
        	"resource/default.jpg")
        self.get_logger().info(f"人脸检测服务启动！") 

    def detect_face_callback(self, request, response):
        if request.image.data:
            cv_image = self.bridge.imgmsg_to_cv2(request.image)
        else:
            cv_image = cv2.imread(self.defaut_image_path)
            self.get_logger().info(f"传入图像为空，使用默认图像！") 
        start_time = time.time()
        self.get_logger().info(f"加载图像完成，开始识别！")
        face_locations =face_recognition.face_locations(
        	cv_image,
        	number_of_times_to_upsample=self.number_of_times_to_upsample, 	
        	model=self.model)
        response.use_time = time.time() - start_time
        response.number = len(face_locations)
        for top,right,bottom,left in face_locations:
            response.top.append(top)
            response.right.append(right)
            response.bottom.append(bottom)
            response.left.append(left)
        return response

def main():
    rclpy.init()
    node = FaceDetectNode("face_detect")
    rclpy.spin(node)
    rclpy.shutdown()
```
在 `setup.py` 中的 `entry_points` 下添加：
```python
"face_detect_client=demo_python_service.face_detect_client_node:main",
```
在终端中运行并检验结果：
```bash
colcon build
source source install/setup.bash
ros2 run demo_python_service face_detect
```
然后新开一个终端，输入：
```bash
ros2 service call /face_detect face_interfaces/srv/FaceDetector
```
在终端中运行人脸检测服务：
```bash
colcon build
source source install/setup.bash
ros2 run demo_python_service face_detect
```
同目录下，新开一个终端，输入：
```bash
source source install/setup.bash
ros2 run demo_python_service face_detect_client
```
