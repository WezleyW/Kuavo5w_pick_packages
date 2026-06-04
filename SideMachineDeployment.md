# 1. 硬件准备
网线连接 4090 及 Kuavo5W
4090_lab 及 Kuavo5W 中的相关网络设置已完成，仅需下位机重新建立桥接即可，无需重新配置。
如意外修改配置，参考原始配置教程：https://bcn9fa1lvktb.feishu.cn/wiki/UYIWw4laoiSoQvkq5epcPqmNnje

# 2. 启动机器人
下位机执行：

```bash
cd kuavo-ros-opensource-1.3.3
sudo su
source devel/setup.bash
roslaunch humanoid_controllers load_kuavo_real_wheel.launch #可以加上cali:=true参数进行限位标定
#等待，按o启动
```

让机器人升高至预备姿态：

```bash
bash set_robot.sh
```

# 3. 配置网络环境
## 3.1 建立桥接网络
下位机执行

```bash
cd kuavo-ros-opensource-1.3.3
sudo bash setup_bridge.sh
```

检查桥接网络

```bash
ip -br addr | grep br0
```

如观察到 `br0 192.168.26.1/24`，则说明桥接建立成功。

## 3.2 检查网络连接
下位机执行

```bash
ping 192.168.26.10
```

4090 边侧机执行

```bash
ping 192.168.26.1
```

都能 ping 通，说明网络通畅

## 3.3 检查 ROS 通讯
边侧机执行：
```bash
rostopic echo /sensors_data_raw
rostopic echo /cam_h/color/image_raw/compressed
rostopic echo /cam_r/color/image_raw/compressed
rostopic echo /cam_l/color/image_raw/compressed
rostopic echo /dexhand/state   
```
rostopic 正常，即可开始边侧机推理

# 4. 边侧机部署推理
边侧机进入 docker，
```bash
docker start ubuntu_ros_container_wezley
docker exec -it ubuntu_ros_container_wezley /bin/bash
```

运行如下代码

```bash
cd workspace/Projects_wezley/Kuavo5W/kuavo_data_challenge_wezley
conda activate kdc_dev
python kuavo_deploy/eval_kuavo.py
#后面输入3，
#输入configs/deploy/kuavo_env.yaml
#输入2插值到起始位置并开始播放bag(bag位置根据kuavo_env.yaml中90行的go_bag_path指定)
#输入3开始模型推理
```
