---
layout: article
title: Hykabaï - Transport Module
abstract: Hykabï MicroSDV design
categories: Robot
tags: Hykabaï
eyeCatcher: https://lh3.googleusercontent.com/HT8q9vMl5-NA6gO6i66Z7Ij_zU_gob5ZVe9l8X7FHe6MNZZowya9Xx2XMv1UVoE8TKAc1V1ZV3t2XW2qDMAJg7cpwyvkTD31FX3ldHtoeT5fkkNwzBCDYiHDB8Rs2ShbnXb9u6SdhA=w2400
mathjax: true
---

# Hykabaï

# Table of Contents
- [Hykabaï](#hykabaï)
- [Table of Contents](#table-of-contents)
  - [Design Goal](#design-goal)
    - [Name](#name)
  - [Mind Map ](#mind-map-)
  - [SDV](#sdv)
  - [Design Characteristics ](#design-characteristics-)
  - [Axiomatic Design ](#axiomatic-design-)
  - [Mechanical Design ](#mechanical-design-)
    - [Differential Configuration ](#differential-configuration-)
    - [Materials](#materials)
  - [Electronic Design ](#electronic-design-)
    - [Boards](#boards)
    - [Encoders](#encoders)
  - [Electrical Design ](#electrical-design-)
    - [Battery](#battery)
    - [Motors](#motors)
    - [Connections](#connections)
  - [Assembly](#assembly)
  - [Software Design ](#software-design-)
    - [Install Steps](#install-steps)
    - [Block diagram ](#block-diagram-)
    - [Control](#control)
      - [On/Off Control ](#onoff-control-)
  - [ROS](#ros)
  - [Testing](#testing)
  - [Troubleshooting](#troubleshooting)

 
## Design Goal

The transport module was designed with the following characteristics in mind:
- Manufacturing
  - Fast manufacturing
  - Replicable
  - Low cost
- Basic unit on a Fleet Management System (FMS)
- Optimized size for desktop manufacturing equipment

This 3D model shows the design of the robot, you can move it around to check some of it's components.

<script src="https://embed.github.com/view/3d/aojedao/microfactory/dev/assets/stl/Hykabaiv4Web.stl"></script>

*Interactive 3D Model of Hykabaï, click and move it*

This is the built robot, available at the Experimental Fabrication Laboratory (LabFabEx) at Universidad Nacional de Colombia.

![HykabaiPic](https://lh3.googleusercontent.com/pw/ADCreHcBrCqs4BqAPo4G-aViOwapNKJfKQIxqkXK2FBclAOABAeJzk5-ZGsTyfXHVj4cgEcz_k_0XlTFGHUSQqJRpmzJOe8c68KsfuX11CdB3acaCAULMlg=w2400)

*Picture of Hykabaï*

### Name

Hykabaï means horse in Chibcha, one of the Colombian native languages, and given the similarities as transport, the name was adopted.

## Mind Map <a name="MindMap"></a>
With these considerations, the following mind map was designed, aiming to provide a tree of possible options to different design arquitectures.

![Mind Map](https://lh3.googleusercontent.com/pw/AJFCJaWGYCNRNBE5FYuvXPsg3QFc-eF-jMe2TvyWld7_uHiC3sMo9i_mCS8AmDO3ic1GaMdp5wOrEzrIrrN9ZGOADHD4huRVXy7cz4OA34TcYo7t2HxWgUs=w2400)

*Mind Map of the Robot design*

## SDV
Likewise, the Self Driving Vehicle model was elected for navigation, instead of an Autonomous Guided Vehicle one, as the routing flexibility (capability of reordering the production steps) of the Microfactory would be the reduced with the second option, as well as the necessity of placing reference beacons or guidelines which would increase the implementation cost.

## Design Characteristics <a name="Design-characteristics"></a>

Hykabaï has the following Key Performance Indicators (KPI):

![Robot KPIs](https://lh3.googleusercontent.com/pw/AIL4fc--RAqqyvKbAOGQ4tbamXyovSsYcGzlak5Ck1VoMXK9QXds8FabHVYlppiBShBfgqEp7Wp9mWeSmjqzpiIR447XBga63XyZ0X-tmnoU28bWJRB8pMg=w2400)

*Hykabaï Key Performance Indicators*

## Axiomatic Design <a name="Axiomatic-design"></a>
Given that this robot will reduce custom built components to replace with Commercial Off The Shelf (COTS) components, the axiomatic design on tihs robot was only applied up to matrix B, because manufacturing processes parameters will be reduced as much as possible.

Matrix A

| Requirement | ROS | Differential Configuration | LIDAR | PRIA Integrated | Containerized |
|  :----:  |  :----:  |  :----:  |  :----:  |  :----:  |  :----:  |
| Autonomous Driving         | x | 0 | 0  | 0 | 0  |
| 360 degree movement        | 0 | x | 0  | 0 | 0  |
| 360 degree perception      | 0 | 0 | x  | 0 |0   |
| Lab Integrated             | 0 | 0 | 0  | x | 0  |
| Replicable                 | 0 | 0 | 0  | 0 | x  |

Matrix B

| Requirement    | Raspberry Pi | Gearmotor   | RPLIDAR A1 | TurtlePRIA | Docker |
| -------- | ------- | -------- | ------- | -------- | ------- |
| ROS                        | x | 0 | 0  | 0 | 0  |
| Differential Configuration | 0 | x | 0  | 0 | 0  |
| LIDAR                      | 0 | 0 | x  | 0 |0   |
| PRIA Integrated            | 0 | 0 | 0  | x | 0  |
| Containerized              | 0 | 0 | 0  | 0 | x  |

## Mechanical Design <a name="mechanical design"></a>
### Differential Configuration <a name="differential-conf"></a>

Afterwards, the traction system was designed. The differential model allows turning over a point, in other words, the robot's central axis perpendicular to the ground. This is important as it reduces space required for maneuvering in small areas, unlike the Ackermann model.

![Kinematic Model Diagram](https://www.researchgate.net/profile/Davide-Spinello/publication/282918322/figure/fig1/AS:337397943422976@1457453346302/Kinematic-model-of-a-differential-drive-mobile-robot.png)

*Kinematic Model of a Differential Robot*

In this diagram we observe a line that crosses the wheels and the orientation line, that is the point mentioned beforehand. The kinematic model is described as follows:

$$ \begin{bmatrix}
  x_k\\
  y_k\\
  \theta_k
  \end{bmatrix} = \begin{bmatrix}
  x_{k-1}\\
  y_{k-1}\\
  \theta_{k-1}
  \end{bmatrix} + \begin{bmatrix}
  \cos{\theta} & 0 \\
  \sin{\theta} & 0 \\
  0 & 1
  \end{bmatrix} \begin{bmatrix}
  \Delta s\\
  \Delta \theta 
\end{bmatrix}$$

And the robot velocities are:

  $$\dot x=v \cos{\theta}$$ 

  $$\dot y = v \sin{\theta}$$ 

  $$\dot \theta = \omega$$ 

### Materials

  The robot counts with two types of componentes, COTS and manufacturable components. Most of the manufactured parts are the structure chassis, and can be done trough multiple technologies and materials. Proposed methods are displayed in the following picture, where blue figures represent a manufacturing method and orange ones represent materials.

![Materials](https://lh3.googleusercontent.com/pw/ADCreHcuM2Qd_XVo5OGRBaGNGJHFeIkeA_U4_8SftHZbULm5z_IUpvsIBvjwCCfPHg6MfC5seOL8v4YDgFfOiAiU3ffxIk21QOo0H_a_txjlAnEVlOffXzQ=w2400)

*Possible manufacturing methods and materials*

## Electronic Design <a name="Electronic-design"></a>

### Boards

Due to the necessity of a high complexity task item and an integrated process two boards to execute each task group was selected. The first board is a [Raspberry Pi 3B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b/), that works as the ROS Master node, as well as the LIDAR and task sequence execution. The second board is a [BeagleBone Blue](https://beagleboard.org/blue), another Single Board Computer (SBC) with a robotics cap integrated in it (DC Motor drivers, servo motors, IMU, etc.). This was done taking into account the [OpenRoACH](https://ieeexplore.ieee.org/document/8794042) project. 

![Raspberry Pi 4](https://secure.sayal.com/images_ext/Raspberry_PI4_overview.jpg)

*Raspberry Pi 4 Components*

The Raspberry Pi 4 is a popular SBC used to implement Linux-based systems in robotics applications. There are multiple RAM size options, for Hykabaï, the 8GB RAM model was used.

![BeagleBone Blue](https://lh3.googleusercontent.com/pw/AIL4fc_X3cJ-DspzTihO8PR0O44jXXM83AhP6ivHcS1Y_CsLhDsZnvLqAs2JIDSa-WFilN9ym-kxTmomk2iNhWAUh4qpE5WGFxMaJrew4zn_RB9mcbreZzY=w2400)

*BeagleBone Blue SBC Components*

The BBBlue has two TB6612FNG dual motor drivers, and are controlling two Pololu [78:1 Metal Gearmotor 20Dx43L mm 6V](https://www.pololu.com/product/3453). As well, the board is powered by a 2S 1000mAh battery, that connects directly with the balance port to the on-board connector.

The components of the BeagleBone Blue used are:

| Name | Component | Usage |
| :------ |:--- | :--- |
| 9 Axis IMU and Barometer | IMU | Velocity, Acceleration and Orientation perception |
| TI Wilink 8 | Wifi and Bluetooth Chip | Wireless Communication |
| USB Host | USB Connector | Connection with Raspberry Pi |
| 2 Cell LiPo Battery Connector | LiPo Battery Connector | Motor Voltage Sourcing |
| Octavo Systems OSD3358 | Microprocessor System-in-Package | Motor and Encoder Control |
| DC Motor Drivers | Motor Drivers | Moving the Motors |
| Quadrature Encoder | Encoder Connectors | Read the Encoder Sensors |

### Encoders

The encoders are using Enhanced Quadrature Encoder Pulse (eQEP) to read both magnetic encoders. The [encoders for 20D](https://www.pololu.com/product/3499) motors were selected. They count with an operating voltage of 2.7V up to 18V. It consists of a dual channel Hall effect sensor board, and a 10-pole magnetic disk. These are incremental encoders.

![Encoder](https://lh3.googleusercontent.com/pw/AIL4fc-0PVx-QAxM_1lX_-5-0U7bIAufCGeziI-vtuCA6q_8F27RXdn_esIsSan-IyZ6VulcsbYeCQB1--xfaRxvHP8ZfCd4EX_BFw_AgrPiSkJH3ToxIzA=w2400) 

*Encoder selected*

This encoder reports a 20 Count Per Revolution, yet as seen in the following image, the number of pulses (or Pulse Per Revolution, PPR), is 4 times the amount of CPR's. The following figure represents the reading of an incremental encoder.
![EncoderGIF](https://lh3.googleusercontent.com/pw/AIL4fc_TfmjUckRxufbgMaMF5kRZhQ-XwwZGFkqK9xRWm7_mGk0aUEd964H2Ekps7XcX8f2OZ4ndetaumULkJJ6toBafXYr89tm3fLmLe257tLaaEch3b2Y=w2400)

*How an encoder reads data*

$$ 1 CPR = 4PPR \\$$
$$ 20 CPR = 80 PPR $$

The absolute encoder instead measures the absolute position, and every step is predefined by a unique set of values combined, in the next image provided by RealPars, we can observe how this type of encoder works.

![AbsoluteEncoder](https://realpars.com/wp-content/uploads/2019/06/Rotary-Absolute-Encoder.gif)

*How an absolute encoder works*

Furthermore, taking into account the reductor on the Gearmotor, and the diameter of the wheel, the equivalency between distance traveled by the wheel and the CPR's by the encoder is given by:

![Encoder revolution diagram](https://lh3.googleusercontent.com/pw/AIL4fc_8nzxd6GgnlTlnInrNYQjQ-Z-ZLHm1bUhUs77IdI_3GZmln0q32n9ABfgszKuwzp42wMu3iV85Sug31KXji_x-sRcdJnqq08N10KIu5mVHVJwB6VM=w2400)

*Diagram of the revolutions across de traction system*

$$\frac{Wheel \, Perimeter}{Encoder \, CPR \cdot N \, Relation}$$

There was an error identified where the robot drifts towards the left side of the robot, in order to solve this issue an statistc evaluation of the ticks recorded on a turn command were analyzed. In next figure you can see the graph of the difference in the ticks recorded and there is a constant proportional value, so the average error was used to find a constant value to multiply the left wheel velocity command.

![Encoder Correction](https://lh3.googleusercontent.com/pw/ADCreHdL5yXikIAE2dfzJcTimnKQYGsVZFEWi-3XZQgT64xIaPbFxbtx9Fpj8RS0kbiLjHoEDB0ECKoxjxh97lU9p7ntJWtoU4c_KiDW_-cNoDtucdcq8Vk=w2400)


*Encoder Correction*


## Electrical Design <a name="Electrical-design"></a>

### Battery

For the Raspberry Pi, any 5V, 2A or greater current will be enough to power the board. The current model uses an Adata P10000QCD such as the one represented in the image.

![Power Bank](https://falabella.scene7.com/is/image/FalabellaCO/gsc_117715690_1868380_1?wid=800&hei=800&qlt=70)

*Example of a power bank*

As for the BeagleBone Blue, the board supports using a 2S (2 Cell battery) directly trough its connector. There are multiple storage capacity presentations, this design uses 1000mAh by Turnigy.

![LiPo Battery](https://www.vistronica.com/10769-large_default/bateria-lipo-turnigy-1000mah-7-4v-30c.jpg)

*LiPo Battery*

### Motors

Two [78:1 Metal Gearmotor 20Dx43L mm 6V](https://www.pololu.com/product/3453) were selected. They have a 78:1 reduction ratio, operate on 6V and count with an extended motor shaft. The maximum speed is 180 rpm under no load, and a 2.4 kg-cm peak torque.

![Motor](https://a.pololu-files.com/picture/0J7474.1200.jpg?0157d52ce89f0ed9ff3b279e72b632fa)

*Image of the Pololu motors used*

The manufacturer provides the following graph for it's rpm, current consumption and eficciency. The robot works only at 0.6 m/s therefore, the efficiency is around the midpoint of it's maximum, given that this linear speed has a lower load on the drivers, and allows for a soft movement profile.

![MotorEfficieny](https://lh3.googleusercontent.com/pw/ADCreHc-g5o6UkkiFM_Il4L4WMUv4dz-BwJDKJ75Hya4d7rO5q9W1iY75iCzWrrf2HJuocA0K1vUhWyWJnxA2Fhqzl6Nxs3KzEUum1e-2ZOIn3MUd0G4V7I=w2400)

*Motor Efficiency Diagram*

### Connections

The electrical diagram the connections is the following:

<figure class="video_container">
<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vTwKvrwoscWyVKc64vXgdrojiPnni7DepgVOsFLHG_R3yJlaKua6ostb0L2eAxcHrXRgf3eYZnjnBms/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</figure>

As a simpler diagram the following image illustrates the connections that should be perfomed.

![Electrical Connections](https://lh3.googleusercontent.com/pw/ADCreHdYkgntXdndOTYpGhulEnjEqPvdUF_jNj6WnQ0y0ro7WAbsvN-Hus66u4L-h2e2Vkah89xYevX4YiTc_ubp-T8qJpWTU8Zw71BkTxvCzJmVkANX9gU=w2400)

*Electrical Connections*


## Assembly

As for the assembly here is shown the assembly process for the Hykabaï V1, a prototype designed for the Manufacturing Processes Automation class at UNAL.

![AssemblyV1](https://lh3.googleusercontent.com/pw/AIL4fc9u4K2Q9BEcAQNGzbTW2ixEP9HojInIdUGSTZ--NV_WabCnxr6KaEdqGv9drvLHY2Hy1t67FYtsAr1vmBfKdkaxSyLR5bI59AR1uvhBc9KNaSDa-DQ=w2400)

*Assembly of Hykabaï*

The second assembly corresponds to the actual robot at the lab. It also has the same colors as seen in the first part od the page.

![Assembly of Hykabai V2](https://lh3.googleusercontent.com/pw/ADCreHcD0zvb1TboD0HqLfT3C_GS_VtEv4qT0F9BKF6Cd8_E1SeyzX24G3vfmU3ngjBilbXByE0DZhZwR-Lz2V0RRtUSHeonYxlVVM1SdPWAXHJuHCmPCfg=w2400)

*Assembly of the actual Hykabaï robot*

Here we can see the robot in two subassemblies, the upper chassis where the Raspberry Pi and RPLidar are located, and the inferior one.

<script src="https://embed.github.com/view/3d/aojedao/microfactory/dev/assets/stl/HykabaiBigSubassembly.stl"></script>

Some of the tools required to assemble the robot are:
- Screwdriver (the tip depends on the screws for your robot) for each screw type. This robot used three, one for M3 screws, one for the motor, and one for the LiDAR M2 screws.
  
- Pliers, to block the nut from moving when fastening the screw (embedded nuts could be used while manufacturing the base)

- Soldering Iron (to solder the motor and encoder wires)
  
- Soldering Paste (For a better soldering quality)
  
- Multimeter, to check continuity while soldering the pins

The inferior chassis holds both motors and the BeagleBone Blue. Motor supports can be placed over or under the base. It is recommended to screw the motor supports to the base first. Either plastic or metallic screws can be used, Hykabaï uses M3 screws and nuts to fix the supports. It is recommended to build this subassembly first.

![Subassembly1](https://lh3.googleusercontent.com/pw/ADCreHcKSMmxujTSfV9F7TuUsx4jvizfNT2XBleC4dS3adUl5Bhnwn-4fd2yd1ysjKMpqXLYSux2k-dwFahxBYwtURSF4pdDLkoLy5ZPfWJiFIRLCiE0Zfo=w2400)

This is the base of the inferior chassis. The holes are to pass the motor and encoder wires, and to fix the battery strap if it is planned to be located at the bottom.

<script src="https://embed.github.com/view/3d/aojedao/microfactory/dev/assets/stl/HykabaiBase1.stl"></script>

*Inferior chassis base*

And this is the motor support:

<script src="https://embed.github.com/view/3d/aojedao/microfactory/dev/assets/stl/HykabaiMotorSupport.stl"></script>

*Motor support*

This is the second version of the support design, as the first one failed due to the load, and broke trough the motor fastener holes, new versions are welcome to improve its life cycles. If the wheels are not parallel to each other,(check Troubleshooting), the motor supports may be too weak for your robot's weight r payload.

The superior chassis is better built by mounting the Raspberry Pi and the superior base first, and the other components afterwards. The RPLiDAR uses M2 screws, insted of M3 ones, therefore either it's standoffs must be changed, or M2 screws should be used. Height of the RPLiDAR may be adjusted as the robot's enviroment changes.

![SubAssembly2](https://lh3.googleusercontent.com/pw/ADCreHdTWYHNMTFozDwL_WWUqArvbzbDwpxU_q-qFIM04tqYLCHIgZ7DcSuZZUCyFyHpVz1w6pbVMH4taeFC9d-pDoS_Zz5ddMXoYYLT6ADsy4miUlzqwR4=w2400)

*Assembly of the superior components*

<script src="https://embed.github.com/view/3d/aojedao/microfactory/dev/assets/stl/HykabaiBase2.stl"></script>

*Model of the superior base*



## Software Design <a name="Software-design"></a>

### Install Steps

1. Installing OS on the Single Board Computers.
   1. Install Debian Buster on the Raspberry Pi using [RPi Imager](https://www.raspberrypi.com/software/). Instructions on how to install it can be found [here](https://www.pragmaticlinux.com/2021/08/install-the-raspberry-pi-imager-on-ubuntu-debian-fedora-and-opensuse/).
   2. Install ROS Melodic from [source](https://www.linkedin.com/pulse/easiest-way-install-ros-melodic-raspberrypi-4-shubham-nandi) 
      Use the sources for ros from [Seedstudio](https://www.seeedstudio.com/blog/2019/08/01/installing-ros-melodic-on-raspberry-pi-4-and-rplidar-a1m8/)
   3. Clone the BeagleBone Blue OS using this [command](https://emteria.com/kb/clone-sd-cards-linux) or install it trough this [image](https://rcn-ee.net/rootfs/). If installed by yourself don't forget to configure ROS_IP, ROS_MASTER_URI and all packages. Also use this [fork](https://github.com/aojedao/bbblue_drivers_microsdv) for the motor drivers
2. Install RPLidar
   1. Configure the [serial port](https://github.com/berndporr/rplidar_rpi) and use the following [package](https://github.com/Slamtec/rplidar_ros)
   2. Check using the following commands: `ls -l /dev/ |grep ttyUSB` to check the correct connection and `sudo chmod 666 /dev/ttyUSB0` to give the required permissions
3. Install Hykabaï packages
   - For the general [robot](https://github.com/aojedao/APM20221-MicroRobot)
   - For [simulation](https://github.com/aojedao/hykabai_simulations)
   - For [mapping](https://github.com/aojedao/hykabai_mapping)
   - For [navigation](https://github.com/aojedao/hykabai_navigation)

Installation of these packages can be done using Windows or Debian OS, and each SBC requires a microSD card, at least 16gb is required, but 32gb is recommended.

### Block diagram <a name="Block-diagram"></a>
Currently the mechatronic system of the robot works as following: 
![BlockDiagram](https://lh3.googleusercontent.com/pw/AIL4fc_F9VMqPcS74A4crEyAyH7VdnpWcjJgJ62QavjkrozBp2qwMLwni1CPYkPXM1Cai4bZs5PcT2XWLXHBJA9qeybvydu5YNtquNH-kn04GBhtwfHc8lU=w2400)

*Block diagram of the Mechanical, Electrical and Communication systems*

The Powerbank, provides energy for the Raspberry Pi, which connects to the RPLidar A1 via microUSB port and serial communication, and to the BeagleBone Blue via USB. This connection uses the USB port as a virtual ethernet port, assigning the 192.168.7.1 IP address to the RPi, and 192.168.7.2 address to the BeagleBone. Afterwards, to move the motors, the topics ran in the RPi (that is executing the ROS Master node), are captured by the bbdrivers package in the BeagleBone. This package implements directly the rc_library files to both, capture the encoder readings, and send movement signals to the motor drivers. As a side note, the robot is capable of executing all of the program without feeding the BeagleBone with it's power supply (a 2S Lipo board in this case, using the balance port) yet it won't be able to move.

### Control

The robot uses multiple control options, whether it's using our own PID controller for speed and position control, or integrated using the ROS Control package.

#### On/Off Control <a name="Onoff-Control"></a>
Initally, to verify that the robot is moving the wheels accordingly to the direction it's given, a simple order-two system was modeled to identify possible values for a speed controller, directly on the bbdrivers package. It was done to model the following control sequence.

![onoffBlockControl](https://lh3.googleusercontent.com/pw/AIL4fc-toEcV0n29m8TNOab-brin9YcRseVjtOT43LVTBJwn56Kjo7QaBcY9ZIaCNkXq4aBqdzBJp-gt7_T7fh0M6jq3BdOhgIYKKnLNWGcGPjY_Ypy8dCw=w2400) 

*On-Off control design*

Two types of pulses were analyzed, in order to contrast if the perceived position due to odometry was correct, first, an angular speed pulse. A python node was coded to send pulses of an specific speed command, alternating between a top value and 0, to check the hardware's ability to follow it. (Given it's differential architecture, the speed considers the wheels turning in different directions simultaneously).

![SpeedPulses](https://lh3.googleusercontent.com/pw/AIL4fc9JxBown9uVlf_2tmlwwCSy6nHRB1g3Ekb_STTlAP146GsIGeQnum1AhMR3leha9Nly3b95r2a-HK7vWf1WhCHcdgi1D9YL6sZTZrK4b017yxIed4M=w2400)

*Speed pulses Designed vs Experimental*

And afterwards, only linear speed pulses were analyzed. 

![LinearPulses](https://lh3.googleusercontent.com/pw/AIL4fc8N96IjVzN3JuxchqSO9h8HKaYNzztR2-CGXU_SHxQ78UiD85cvTIMz8FPN9l1JU5P_phYAFiYf38rDGgOYN19cR35TwoOpU9wjxamXtp8tmxQUvR8=w2400)

*Theoretical vs Experimental Linear Velocities*

## ROS

ROS is a communication framework to connect nodes that execute tasks, such as moving a motor, locating the robot or sending a goal, with other nodes through a topic, which is the name of the channel where certain message types, such as images, veloctities or point clouds are transferred.

The nodes running on the robot when performing mapping with Hector SLAM algorithm is presented in the next image, using RQT, a common graph tool for nodes and topics in ROS.

![Mapping RQT Graph](https://lh3.googleusercontent.com/pw/ADCreHdue1xNt7oAtGBCV2VUxceSGFc7qdR6AOLfWzRtYbJIB7IxwPBLOChIXw9esOEWFEP8ZnFopoDj4Himc3ybzuobk-WLNGeR5_bz-PCyFQnkQABDPuI=w2400)

*RQT Graph of the mapping process*

The robot is communicated with PRIA UN using the following architechture, where the ROS Logic uses the generic PRIA package to communicate with an action server, and the robot locally uses the same nodes mentioned before. For more information on the connection in the Lab network visit this [link](https://aojedao.github.io/Microfactory/announcement/2022/05/10/Networking-Structure.html)

![PRIA Integration](https://lh3.googleusercontent.com/pw/ADCreHcHKF-NW766VyY5rqTJ8dzbJJBWLWgr2mSOQM9P_fqVxiYxngXsfyJqZPMoYzwr6MDpyQql7Yyf1evNFkNNHvCIKAlECZ_3rjckxIZtOiywjNMw_7M=w2400)

*Integration in PRIA*

The robot can be observed navigating in a square trajectory, simulating tranport between the modules of the microfactory, it is done using a PI controller to follow a predefined n-sided figure, and scale-adjusted as the user may need. The square figures show the placement where the other modules may be placed.

![Robot Executing Trajectories](https://lh3.googleusercontent.com/pw/ADCreHc6tRByGsYoPe2S6NQD71Xi6iDtqzZ6COGu6hSulasijknnBeCNT-23c7ZTaZpp3CVs7SQZYqWQzTCS4HaOyKmCLFxpv57SJ7HcwWMQykVGE3dIfwc=w2400)

*Hykabaï executing trajectories*

The route the robot uses to know when to be activated and start a transportation routine is described in the following Process Oriented Analysis (POA) diagram, where the central section illustrates the inner Hykabaï workings, and in the exterior area, it's integration with a PCB Microfactory manufacturing process.

![POA Process](https://lh3.googleusercontent.com/pw/ADCreHc_xQ_PWzzt1QaUdJNSaRpljCmqnLrQ8_vZWDwlmWZ3nxLPIbrRb1gj7UqvUL3UPhGVLs0EPQ36F2ckuwXXAWTnA3-Jvq2_Z3NJzl2Eje1deJNJh7k=w2400)

*Process Oriented Analysis Diagram of the robot*

## Testing

The robot has multiple operational modes.

To launch mapping using HectorSlam node by node do this:

1. Execute `roscore`
2. Execute `roslaunch rplidar_ros rplidar.launch`
   1. Execute `ls -l /dev/ |grep ttyUSB` (to check usb)
   2. Execute `sudo chmod 666 /dev/ttyUSB0`
   3. Execute `roslaunch rplidar_ros view_rplidar.launch` (if you want to verify it's working)
3. Execute `rosrun bbblue_drivers vel_control_odom` (on the BBBlue)
4. Execute `roslaunch hykabai_simulations rviz.launch` (load config on hector_slam_launch folder)
5. Execute `roslaunch hector_slam_launch tutorial.launch`


To run HectorSLAM with Launch
1. Config RPLidar
   1. Execute `ls -l /dev/ |grep ttyUSB` (check usb)
   2. Execute `sudo chmod 666 /dev/ttyUSB0`
2. Execute `rosrun bbblue_drivers vel_control_odom` ( on BBBlue)
   1. Execute `roslaunch hector_slam hykabai_hector_slam.launch`
   2. Navigate using `rosrun teleop_twsit_keyboard teleop_twist_keyboard.py`, at a 0.104 linear speed
3. When you're satisfied with the map, execute `rosrun map_server map_saver`

## Troubleshooting

If the robot is operating incorrectly, or issues are identified, the following FMECA analysis is presented, and the recommended solution for each case.

<style>
.responsive-wrap iframe{ max-width: 100%;}
</style>
<div class="responsive-wrap">
<!-- this is the embed code provided by Google -->
  <iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vTPWCqU09mavsYeHc9mMIke2vDJ_xBY_6HWYhL0JbsP9D7fc7dqRVr1valfcYS63WeDf3NSehouM9HY/pubhtml?gid=0&single=true" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
<!-- Google embed ends -->
</div>


<!---

## Here is a secondary heading

Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

-->

