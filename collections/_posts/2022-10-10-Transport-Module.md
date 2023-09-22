---
layout: article
title: Hykabaï - Transport Module
abstract: Hykabï MicroSDV design
categories: markdown
tags: test
eyeCatcher: https://lh3.googleusercontent.com/HT8q9vMl5-NA6gO6i66Z7Ij_zU_gob5ZVe9l8X7FHe6MNZZowya9Xx2XMv1UVoE8TKAc1V1ZV3t2XW2qDMAJg7cpwyvkTD31FX3ldHtoeT5fkkNwzBCDYiHDB8Rs2ShbnXb9u6SdhA=w2400
mathjax: true
---

# Hykabaï

The transport module was designed with the following characteristics in mind:
- Manufacturing
  - Fast manufacturing
  - Replicable
  - Low cost
- Basic unit on a Fleet Management System (FMS)
- Optimized size for desktop manufacturing equipment

<!-- Import the component -->
<model-viewer style="width: 100%; height: 100%; background-color: #eee;" src="https://media.keyshot.com/scenes/envoy/envoy-opt.glb" ios-src="https://media.keyshot.com/scenes/envoy/envoy.usdz" poster="https://media.keyshot.com/scenes/envoy/envoy-scan.jpg" alt="3D Export from KeyShot" ar-modes="webxr scene-viewer quick-look" ar-scale="auto" ar auto-rotate camera-controls></model-viewer>

<script type="module" src="https://ajax.googleapis.com/ajax/libs/model-viewer/3.1.1/model-viewer.min.js"></script>

<!-- Use it like any other HTML element -->
<model-viewer alt="Neil Armstrong's Spacesuit from the Smithsonian Digitization Programs Office and National Air and Space Museum" src="https://media.keyshot.com/scenes/envoy/envoy-opt.glb" ar environment-image="shared-assets/environments/moon_1k.hdr"  shadow-intensity="1" camera-controls touch-action="pan-y"></model-viewer>

## Mind Map
With these considerations, the following mind map was designed, aiming to provide a tree of possible options to different design arquitectures.

![Mind Map](https://lh3.googleusercontent.com/pw/AJFCJaWGYCNRNBE5FYuvXPsg3QFc-eF-jMe2TvyWld7_uHiC3sMo9i_mCS8AmDO3ic1GaMdp5wOrEzrIrrN9ZGOADHD4huRVXy7cz4OA34TcYo7t2HxWgUs=w2400)

*Mind Map of the Robot design*

## SDV
Likewise, the Self Driving Vehicle model was elected for navigation, instead of an Autonomous Guided Vehicle one, as the routing flexibility (capability of reordering the production steps) of the Microfactory would be the reduced with the second option, as well as the necessity of placing reference beacons or guidelines which would increase the implementation cost.

## Design Characteristics

Hykabaï has the following Key Performance Indicators (KPI):

![Robot KPIs](https://lh3.googleusercontent.com/pw/AIL4fc--RAqqyvKbAOGQ4tbamXyovSsYcGzlak5Ck1VoMXK9QXds8FabHVYlppiBShBfgqEp7Wp9mWeSmjqzpiIR447XBga63XyZ0X-tmnoU28bWJRB8pMg=w2400)

*Hykabaï Key Performance Indicators*

## Axiomatic Design
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

## Mechanical Design
### Differential Configuration

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

### Assembly

![Assembly](https://lh3.googleusercontent.com/pw/AIL4fc9u4K2Q9BEcAQNGzbTW2ixEP9HojInIdUGSTZ--NV_WabCnxr6KaEdqGv9drvLHY2Hy1t67FYtsAr1vmBfKdkaxSyLR5bI59AR1uvhBc9KNaSDa-DQ=w2400)

*Assembly of Hykabaï*

## Electronic Design 

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

The encoders are using Enhanced Quadrature Encoder Pulse (eQEP) to read both magnetic encoders. The [encoders for 20D](https://www.pololu.com/product/3499) motors were selected. They count with an operating voltage of 2.7V up to 18V. It consists of a dual channel Hall effect sensor board, and a 10-pole magnetic disk.

![Encoder](https://lh3.googleusercontent.com/pw/AIL4fc-0PVx-QAxM_1lX_-5-0U7bIAufCGeziI-vtuCA6q_8F27RXdn_esIsSan-IyZ6VulcsbYeCQB1--xfaRxvHP8ZfCd4EX_BFw_AgrPiSkJH3ToxIzA=w2400) 

*Encoder selected*

This encoder reports a 20 Count Per Revolution, yet as seen in the following image, the number of pulses (or Pulse Per Revolution, PPR), is 4 times the amount of CPR's.
![ENcoderGIF](https://lh3.googleusercontent.com/pw/AIL4fc_TfmjUckRxufbgMaMF5kRZhQ-XwwZGFkqK9xRWm7_mGk0aUEd964H2Ekps7XcX8f2OZ4ndetaumULkJJ6toBafXYr89tm3fLmLe257tLaaEch3b2Y=w2400)

*How an encoder reads data*

$$ 1 CPR = 4PPR \\$$
$$ 20 CPR = 80 PPR $$

Furthermore, taking into account the reductor on the Gearmotor, and the diameter of the wheel, the equivalency between distance traveled by the wheel and the CPR's by the encoder is given by:

![Encoder revolution diagram](https://lh3.googleusercontent.com/pw/AIL4fc_8nzxd6GgnlTlnInrNYQjQ-Z-ZLHm1bUhUs77IdI_3GZmln0q32n9ABfgszKuwzp42wMu3iV85Sug31KXji_x-sRcdJnqq08N10KIu5mVHVJwB6VM=w2400)

*Diagram of the revolutions across de traction system*

$$\frac{Wheel \, Perimeter}{Encoder \, CPR \cdot N \, Relation}$$

## Software Design
### Control

### Block diagram
Currently the mechatronic system of the robot works as following: 

![BLockDiagram](https://lh3.googleusercontent.com/pw/AIL4fc_F9VMqPcS74A4crEyAyH7VdnpWcjJgJ62QavjkrozBp2qwMLwni1CPYkPXM1Cai4bZs5PcT2XWLXHBJA9qeybvydu5YNtquNH-kn04GBhtwfHc8lU=w2400)

*Block diagram of the Mechanical, Electrical and Communication systems*

The Powerbank, provides energy for the Raspberry Pi, which connects to the RPLidar A1 via microUSB port and serial communication, and to the BeagleBone Blue via USB. This connection uses the USB port as a virtual ethernet port, assigning the 192.168.7.1 IP address to the RPi, and 192.168.7.2 address to the BeagleBone. Afterwards, to move the motors, the topics ran in the RPi (that is executing the ROS Master node), are captured by the bbdrivers package in the BeagleBone. This package implements directly the rc_library files to both, capture the encoder readings, and send movement signals to the motor drivers. As a side note, the robot is capable of executing all of the program without feeding the BeagleBone with it's power supply (a 2S Lipo board in this case, using the balance port) yet it won't be able to move.

#### On/Off Control
Initally, to verify that the robot is moving the wheels accordingly to the direction it's given, a simple order-two system was modeled to identify possible values for a speed controller, directly on the bbdrivers package. It was done to model the following control sequence.

![onoffBlockControl](https://lh3.googleusercontent.com/pw/AIL4fc-toEcV0n29m8TNOab-brin9YcRseVjtOT43LVTBJwn56Kjo7QaBcY9ZIaCNkXq4aBqdzBJp-gt7_T7fh0M6jq3BdOhgIYKKnLNWGcGPjY_Ypy8dCw=w2400) 

*On-Off control design*

Two types of pulses were analyzed, in order to contrast if the perceived position due to odometry was correct, first, an angular speed pulse. A python node was coded to send pulses of an specific speed command, alternating between a top value and 0, to check the hardware's ability to follow it. (Given it's differential architecture, the speed considers the wheels turning in different directions simultaneously).

![SpeedPulses](https://lh3.googleusercontent.com/pw/AIL4fc9JxBown9uVlf_2tmlwwCSy6nHRB1g3Ekb_STTlAP146GsIGeQnum1AhMR3leha9Nly3b95r2a-HK7vWf1WhCHcdgi1D9YL6sZTZrK4b017yxIed4M=w2400)

*Speed pulses Designed vs Experimental*

And afterwards, only linear speed pulses were analyzed. 

![LinearPulses](https://lh3.googleusercontent.com/pw/AIL4fc8N96IjVzN3JuxchqSO9h8HKaYNzztR2-CGXU_SHxQ78UiD85cvTIMz8FPN9l1JU5P_phYAFiYf38rDGgOYN19cR35TwoOpU9wjxamXtp8tmxQUvR8=w2400)

*Theoretical vs Experimental Linear Velocities*

## Troubleshooting

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

