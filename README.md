# FatigueDetectRKNN

基于 RK3568 NPU 的疲劳驾驶检测系统，控制界面包含**疲劳驾驶检测**、**二分类模型训练数据采集**、**视频采集**三个部分，另外可以在第三方（浏览器）显示实时检测结果，或者选择指定视频（回放）的实时检测结果。

硬件部分以 ROCK 3A 板卡为主体，另外包含树莓派 OV5647 摄像头模组，和一个有源蜂鸣器模块（疲劳时通知、采集数据时辅助提示），风扇是可选的（风扇安装在外壳上）。

另外包含一套外壳，可以使用 3D 打印机与硬件组装在一起，是否需要风扇可以自选。

## 一、软件

### 环境

疲劳驾驶检测系统的环境配置在 `tools\envs_tools`，具体的配置可以直接查看或者执行脚本 `auto_env.sh`。操作系统的话应该 Ubuntu22.04 即可，我选用的 ROCK 3A 打包好的镜像。

ONNX 模型转 RKNN 模型的环境在 `tools\model_tools`，包含一个模型转换服务端，和对应的 docker 环境配置。（注意这个环境创建了一个 docker，是另外的一个系统）

### 启动

在终端执行 `python app.py` 启动疲劳驾驶检测系统，在局域网的电脑的浏览器中打开 `http://your_board_ip:5000` 即可看到系统的主页。

### 模型

检测系统包含人脸检测、人脸关键点检测、眼睛状态检测和嘴巴状态检测几个部分，其中人脸检测部分和人脸关键点部分来自于 [Ultra-Light-Fast-Generic-Face-Detector-1MB](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB) 和 [PFLD_GhostOne](https://github.com/AnthonyF333/PFLD_GhostOne)，可以参考这两个仓库的说明对模型进行再训练。眼睛检测和嘴巴检测部分是一个简单的图片二分类模型，由猫狗二分类模型改进而来，在本系统中是可以继续训练的。眼睛和嘴巴检测的结果通过疲劳判别部分输出是否是疲劳状态（根据帧序列做简单的占比分析而已）。

模型全部通过 `tools\model_tools` 中的模型转换程序，由 ONNX 模型转为 RKNN 模型。

### API

API 是给第三方显示结果用的。

* `/video_feed/<video_name>` ： 选择录制的视频回放推理

  * 视频存放的位置在 `data/video/` 下
* `/camera_feed` ： 摄像头实时图像推理数据

```html
<img src="http://your_board_ip:5000/camera_feed" alt="" />
// 或者
<img src="http://your_board_ip:5000/video_feed/video_1.mp4" alt="" />
```

## 二、硬件

主体：

* ROCK 3A 板卡
* 树莓派 OV5647 摄像头（接到板卡的 CSI 接口上）
* 有源蜂鸣器（接到板卡的 GPIO 上）

额外：

* 风扇：与外壳配套，并不是安装在板卡上

  * 大小：50mmX50mmX10mm
  * 平行孔距：41mm
  * 对角孔距：57mm

## 三、外壳

网上找不到 rock 3a 合适的外壳，而且还需要安装摄像头和散热风扇，所以手搓了一套。

外壳文件在 shell 目录下，包含外壳部分和盖子部分。外壳和盖子之间的连接，摄像头和外壳之间的连接，均可以用 2mm 直径螺丝固定，外壳与盖子之间固定不需要螺母。