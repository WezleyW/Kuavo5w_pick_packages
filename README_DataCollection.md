# Kuavo5W Data Collection tutorial

This is a tutorial for data collection of Kuavo5W for Embodied Intelligence and Robotic Systems (EMBODICA) Lab, School of Intelligence and Science Technology, Nanjing University (Suzhou Campus).

# 1. Basic information

External Network of Kuavo5W

```bash
# WiFi
SSID: LejuRobotics
Password: 1234567890

# 下位机 IP:
192.168.1.11 # usb
192.168.1.12 # wifi

# 上位机 IP:
192.168.1.13 # usb
192.168.1.14 # wifi

# VR IP:
192.168.1.15 # usb
192.168.1.16 # wifi
```

Internal Network of Kuavo5W

```bash
# 下位机 IP:
192.168.26.1

# 上位机 IP:
192.168.26.12

# 底盘 IP:
192.168.26.22
```

# 2. Python-based VR Control

## 2.1 Connect Kuavo5W and quest3 with LejuRobotics network

> [!WARNING]
> Wired connection and wireless connection are both feasible, but **pay attention to different IP address**  

## 2.2 Teleoperation

### Default teleoperation mode (dangerous!!!)

Teleoperation with all degree of freedom (arms+hands+head+waist+base)
```bash
cd kuavo-ros-opensource-1.3.3/
sudo su
source devel/setup.bash
roslaunch humanoid_controllers load_kuavo_real_wheel_vr.launch \
  ip_address:=192.168.1.15 \    # Double-check that the IP address matches the actual connection method (wired or wireless)!!!
  enable_videostream:=true \
  camera_publisher_name:=/cam_h/color

# press `o` to enable control
```

### Upper teleoperation mode (recommended)

Teleoperation with only degree of freedom of upper body (arms+hand)
```bash
cd kuavo-ros-opensource-1.3.3/
sudo su
source devel/setup.bash
roslaunch humanoid_controllers load_kuavo_real_wheel_vr.launch \
  ip_address:=192.168.1.15 \    # Double-check that the IP address matches the actual connection method (wired or wireless)!!!
  control_torso:=false \
  enable_head_control:=false \
  enable_base_control:=false \
  enable_videostream:=true \
  camera_publisher_name:=/cam_h/color

# press `o` to enable control
```

### Teleoperation command

#### (1) 手部控制（灵巧手）

手指张合：按前扳机控制手指张合。

拇指开合：触摸 X / A 键控制拇指开合。

#### (2) 腰部控制

开启方法：

轻触摸X和Y键+按下B 键(注意不是重按X和Y键)，此时下肢会同步人体动作。

关闭方法：

轻触摸X和Y键+按下B 键(注意不是重按X和Y键)，此时下肢会复位到初始状态。

注意：开启腰部控制后请缓慢移动身体，以避免发生机器人快速运动导致异常损坏。

#### (3) 手臂解锁/锁定与复位

解锁手臂：同时按X + A键，再按左右上扳机，再按X和A键解锁手臂。

复位并锁定手臂：再次同时按下 X + A，此时手臂恢复到初始状态并锁定。

后续如需再次解锁，重复按下 X + A 即可。

#### (4) 固定手臂位置

固定：同时按下 X + B 此时会固定手臂。

解锁：再次按 X + B 此时会解锁手臂。

#### (5) 移动控制

前进/后退：向前/后推左手柄拨杆。

左转/右转：向左/右推右手柄拨杆。

#### (6) 程序关闭

同时按下 X + Y 键，此时机器人启动程序关闭

