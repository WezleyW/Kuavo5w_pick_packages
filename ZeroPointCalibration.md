# 1.进入零点标定
```bash
cd kuavo-ros-opensource  
sudo su  
source devel/setup.bash  
roslaunch humanoid_controllers load_kuavo_real_wheel.launch cali:=true cali_leg:=true cali_arm:=true ##标定腿部和手臂、头部
```

# 2.标定腰部
在终端输入 y 确认，腰部电机将进入“软”状态。
> [!WARNING]
> 此时机器人将瘫倒！！！务必有人在按 y 前扶着机器人

手动将腰部摆到正确位置，根据指示保存标定结果，并进入后续标定流程。

# 3.标定手臂及头部
在终端按 o 继续标定流程
o->1->2
按照指示完成手臂及头部即可

