# Installation Autoware.Auto using Docker (W/O ADE)

#### Install docker
> sudo apt update && sudo apt install docker.io 

#### Create Docker image
> sudo apt update  

> git clone https://github.com/tjddus0403/LAB/tree/main/Autoware  

> cd Autoware  

> docker build -t autoware.auto:tjddus .  

#### Check the Docker image
> docker images

#### Run container
> ./run.sh autoware.auto:tjddus

#### Localization demo using rosbag
> cd ~  

> curl https://autoware-auto.s3.us-east-2.amazonaws.com/rosbag2/rosbag2-astuff-1-lidar-only.tar.gz | tar xz  

> ros2 launch autoware_demos localization.launch.py  

#### AVP (Autonomous Valet Parking) demo
###### 1. install & build ros2-lgsvl-bridge
> sudo apt install libboost-all-dev

> source /opt/ros/foxy/setup.bash

> sudo apt update

> sudo apt install ros-foxy-lgsvl-bridgehttps://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/avpdemo.html

> git clone https://github.com/lgsvl/ros2-lgsvl-bridge.git

> source /opt/ros/foxy/setup.bash

> cd ros2-lgsvl-bridge

> git checkout foxy-devel

> colcon build --cmake-args '-DCMAKE_BUILD_TYPE=Release'

###### 2. running ros2-lgsvl-bridge (In container, terminal 1)
> cd ~

> source ros2-lgsvl-bridge/install/setup.bash

> lgsvl_bridge
###### 3. running svl simulator (host terminal)
https://www.svlsimulator.com/ -> Download for Linux

Download 폴더에서 zip파일 Extract here

> cd ~/Download/svlsimulator-linux64-2021.3

> ./simulator

계정과 cluster 생성 후, 아래의 조건으로 simulation setup  
- Runtime Template: Random Traffic  
- Maps: AutonomouStuff  
- Vehicle Asset: Lexus2016RXHybrid  
- Sensor configuration: Autoware.Auto  
- Bridge: ROS2  
- Autopilot: Autoware.Auto (Apex.AI)  
- Connection: localhost:9090  
simulation 생성 후, Run simulation  
*****아직 simulation start button 누르지 말고 기다릴 것*****

###### 4. running rviz (In container, terminal 2)
> source /opt/AutowareAuto/setup.bash

> ros2 launch autoware_auto_launch autoware_auto_visualization.launch.py

###### 5. running Autoware.Auto stack (In container, terminal 3)
> source /opt/AutowareAuto/setup.bash

> ros2 launch autoware_demos avp_sim.launch.py

###### 6. Initilaizing NDT localizer (In container, terminal 4)
> source /opt/AutowareAuto/setup.bash

> ros2 topic pub --once /localization/initialpose geometry_msgs/msg/PoseWithCovarianceStamped "
{header : {
    stamp : {
        sec: 0,
        nanosec: 0
    },
    frame_id : "map"
},
pose : {
    pose : {
        position : {
            x: -57.463,
            y: -41.644,
            z: -2.01,
        },
        orientation : {
            x: 0.0,
            y: 0.0,
            z: -0.99917,
            w: 0.04059,
        },
    },
    covariance : [
        0.25, 0.0,  0.0, 0.0, 0.0, 0.0,
        0.0,  0.25, 0.0, 0.0, 0.0, 0.0,
        0.0,  0.0,  0.0, 0.0, 0.0, 0.0,
        0.0,  0.0,  0.0, 0.0, 0.0, 0.0,
        0.0,  0.0,  0.0, 0.0, 0.0, 0.0,
        0.0,  0.0,  0.0, 0.0, 0.0, 0.068,
    ],
}}
"

*****simulation start button 누르고 방향키 조절하며 demo 실행*****


#### 참고
##### https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html
##### https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/installation-no-ade.html
##### svlsimulator.com/docs/system-under-test/ros2-bridge/
##### https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/avpdemo.html
##### https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/ndt-initialization.html
