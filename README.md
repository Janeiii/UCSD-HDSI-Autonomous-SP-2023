
# Automated Reasoning Applied to Physical Systems Improving the Robustness of Autonomous Vehicles - LiDAR Application, UCSD HDSI - DSC 190 SP 2023 Capstone Project


This guide will provide you with the necessary steps and instructions to set up and configure your LIVOX HAP, and walk you through the process of setting up and configuring the LIVOX HAP system. The LIVOX HAP system combines LiDAR pointcloud visualization and cone detection capabilities, providing valuable information for path planning in ROS2-based autonomous vehicles. For more information, please visit [evgokart-slam](https://github.com/aashishbhole/evgokart-slam).

![Photo Credit from https://www.livoxtech.com/hap](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*JRQrihM7Ki3ZOsKsVppGxQ.png)
## Features

- LiDAR Pointcloud Visualization
- LiDAR Clustering Towards Cones (Cone Detection)
- Sends Topic Information To Path Planning of Autonomous Viehcle In ROS2


## Usage/Examples (Jetson Container)

1. SSH into Jetson from terminal
```bash
ssh -X jetson@ucsd-agx-03
Password: jetsonucsd
```
2. Start container 
If the docker container have not yet been launched:
```bash
sudo usermod -aG docker jetson
newgrp docker
docker pull ghcr.io/ucsd-ecemae-148/donkeycontainer:agx
xhost +
docker run \
    --name donkey_framework\
    -it\
    --rm \
    --privileged \
    --net=host \
    -e DISPLAY=$DISPLAY \
    -v /dev/bus/usb:/dev/bus/usb \
    --device-cgroup-rule='c 189:* rmw' \
    --device /dev/video0 \
    --volume='/home/jetson/.Xauthority:/root/.Xauthority:rw' \
    --volume='/tmp/.X11-unix/:/tmp/.X11-unix' \
    --volume='/home/jetson/projects/mycar:/projects/mycar' \
    ghcr.io/ucsd-ecemae-148/donkeycontainer:agx
```
If the docker container has been launched:
```bash
exec -it donkeycarframework bash
```



## LIVOX-SDK2 Installation

Install CMake using apt

```bash
sudo apt install cmake
```
Compile and install the Livox-SDK2
```bash
git clone https://github.com/Livox-SDK/Livox-SDK2.git
cd ./Livox-SDK2/
mkdir build
cd build
cmake .. && make -j
sudo make install
```
## Sample run for SDK

To run examples provided by Livox, run the following command

```bash
cd samples/livox_lidar_quick_start && ./livox_lidar_quick_start ../../../samples/livox_lidar_quick_start/hap_config.json
```


## LIVOX ROS Driver Installation
Clone Driver from the github repo

```bash
git clone https://github.com/Livox-SDK/livox_ros_driver2.git ws_livox/src/livox_ros_driver2
```
Build the driver with ROS2 Foxy,
cd to ws_livox/src/livox_ros_driver2 and 

```bash
source /opt/ros/foxy/setup.sh
./build.sh ROS2

```
## Source the driver and launch 

To run examples provided by Livox, run the following command

```bash
source ../../install/setup.sh
ros2 launch livox_ros_driver2 [launch file]
Eg. ros2 launch livox_ros_driver2 rviz_HAP_launch.py
```
Real-life example from RViz
![alt text](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*E3QiZYDYLCZ86vjzLbP4nw.png)
![alt text](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*hOp18l1q8JIe91LqGtlhng.png)
## Demo

Below shows a quick demo of how LIVOX HAP Lidar is applied in real-life senario, alongside the application of path planning on the left.

![Demo running the cone detection](https://github.com/aashishbhole/evgokart-slam/raw/main/media/demo.gif)


## FAQ

#### launch with "livox_lidar_rviz_HAP.launch" but no point cloud display on the grid?

Please check the "Global Options - Fixed Frame" field in the RViz "Display" pannel. Set the field value to "livox_frame" and check the "PointCloud2" option in the pannel.

#### launch with command "ros2 launch livox_lidar_rviz_HAP_launch.py" but cannot open shared object file "liblivox_sdk_shared.so" ?

Please add '/usr/local/lib' to the env LD_LIBRARY_PATH.
- If you want to add to current terminal:
```bash
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib
```
- If you want to add to current user:
```bash
vim ~/.bashrc
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib
source ~/.bashrc
```
## Documentation

[Final Report](https://docs.google.com/document/d/1kUisbx2-pAKSdtpWiEEyaNBT1TFwzrGiAgTsGAvwrDo/edit?usp=sharing)

[Project Reference](https://docs.google.com/document/d/1SFfYeL9RrRfCy0YRkwnT921exbTTX5tCZhqelGi_MPg/edit)

[Medium](https://medium.com/@janetlinw/an-introduction-guide-on-setting-up-livox-hap-lidar-54881600c26a)


## Acknowledgements

 - [Awesome Readme Templates](https://awesomeopensource.com/project/elangosundar/awesome-README-templates)
 - [Awesome README](https://github.com/matiassingers/awesome-readme)
 - [How to write a Good readme](https://bulldogjob.com/news/449-how-to-write-a-good-readme-for-your-github-project)
 - [Livox HAP Driver](https://github.com/Livox-SDK/livox_ros_driver2)
 - [Livox HAP SDK](https://github.com/Livox-SDK/Livox-SDK2)


## Support

For additional support, please contact or email mlopezme@ucsd.edu.

