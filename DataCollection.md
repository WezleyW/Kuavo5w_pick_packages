# Kuavo5W Data Collection Tutorial

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

# 2. VR control

## 2.1 Connect Kuavo5W and quest3 with LejuRobotics network

> [!WARNING]
> Wired connection and wireless connection are both feasible, but **pay attention to different IP address**  

## 2.2 Roslunch Kuavo5W
### Kuavo-ros-opensource-1.3.3: incremental teleoperation
In the **first terminal**, run following roslaunch script:
```bash
cd kuavo-ros-opensource-1.3.3/
sudo su
source devel/setup.bash
roslaunch humanoid_controllers load_kuavo_real_wheel_vr.launch \
  ip_address:=192.168.1.15 \
  control_torso:=false \
  enable_head_control:=false \
  enable_base_control:=false \
  enable_videostream:=false \ # 如需图传，可改为true
  camera_publisher_name:=/cam_h/color \
  use_cpp_incremental_ik:=true \
  use_incremental_hand_orientation:=false
# press `o` to enable control
```

### Kuavo-ros-opensource-1.4.4: incremental teleoperation (暂不可用）
In the **first terminal**, run following roslaunch script:
```bash
cd kuavo-ros-opensource-1.4.4/
sudo su
source devel/setup.bash
roslaunch humanoid_controllers load_kuavo_real_wheel_vr.launch \
    ip_address:=192.168.1.15 \
    use_cpp_incremental_ik:=true \
    use_incremental_hand_orientation:=false \
    enable_head_control:=false \
    enable_chassis_control:=false \
    enable_lb_leg_control:=false \
    control_torso:=false

# press `o` to enable control
```

### Nodlet failure
If the roslaunch is failed, run 
```bash
pkill -f ros
````
and try again.
> [!WARNING]
> You may need to **reload some camera ros nodes** after pkill.

## 2.3 Set to initial position
In the **second terminal**, run setup script to set the robot to target preparing position.
```bash
bash set_robot.sh
```
> [!WARNING]
> Set to initial position before wearing quest3 and active Kuavo-Hand-Track-MR app.

## 2.4 Reset Videostream

Wear quest3 and activate Kuavo-Hand-Track-MR app. 

## 2.5 Absolute VR command
Absolute VR commands are summrized as below:

#### (1) 手部控制（灵巧手）
手指张合：按前扳机控制手指张合。
拇指开合：触摸 X / A 键控制拇指开合。

#### (2) 手臂解锁/锁定与复位
解锁手臂：同时按X + A键，再按左右上扳机，再按X和A键解锁手臂。
复位并锁定手臂：再次同时按下 X + A，此时手臂恢复到初始状态并锁定。
后续如需再次解锁，重复按下 X + A 即可。

#### (3) 固定手臂位置
固定：同时按下 X + B 此时会固定手臂。
解锁：再次按 X + B 此时会解锁手臂。

#### (4) 腰部控制
开启方法：
轻触摸X和Y键+按下B 键(注意不是重按X和Y键)，此时下肢会同步人体动作。

关闭方法：
轻触摸X和Y键+按下B 键(注意不是重按X和Y键)，此时下肢会复位到初始状态。
注意：开启腰部控制后请缓慢移动身体，以避免发生机器人快速运动导致异常损坏。

#### (5) 移动控制
前进/后退：向前/后推左手柄拨杆。
左转/右转：向左/右推右手柄拨杆。

#### (6) 程序关闭
同时按下 X + Y 键，此时机器人启动程序关闭

## 2.6 Incremental VR command
#### (1) Switch arm control mode to 2:
 press x + A twice to enable arm control mode switched to 2. (in terminal you will see the arm control mode changed from 0 -> 1 -> 2)

#### (2) Control the robot arms using hand joysticks: 
press the both side trigger, meanwhile you can use the joystick to enable the arm tracking. other operation is same as above.

# 3. Rosbag data collection
## 3.1 Upper machine
```bash
# 上位机执行： rs-enumerate-devices 查看左右手腕相机Device info/Serial Number
# 终端执行 sudo vim /etc/kuavo.conf 将查到的设备号改入 CAMERA_LEFT=,CAMERA_RIGHT=
sudo systemctl enable start_camera.service 
# 这一步之前已执行好，可以不执行
```

## 3.2 Lower machine
Run following scripts to start rosbag record.
> [!WARNING]
> Run followning scripts in the **third terminal**. Do not run these scripts in sudo mode!
```bash
# 下位机执行：
cd kuavo-ros-opensource-1.3.3
source devel/setup.bash

# before you start recording, configure the save path and rosbag name when necessary
python3 src/demo/examples_code/record_data/rosbag_tool.py

# In the terminal select: `recording the rosbag` to start the recording
# press `ctrl + c` to and save the recording
```

# 4.Reset Kuavo5W
> [!WARNING]
> Run followning scripts in the **second terminal** to set the robot to the initial position.
```bash
bash reset_robot.sh
```
