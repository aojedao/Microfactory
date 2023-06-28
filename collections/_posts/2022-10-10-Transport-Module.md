---
layout: article
title: Transport Module
abstract: MicroSDV design
categories: markdown
tags: test
eyeCatcher: https://lh3.googleusercontent.com/HT8q9vMl5-NA6gO6i66Z7Ij_zU_gob5ZVe9l8X7FHe6MNZZowya9Xx2XMv1UVoE8TKAc1V1ZV3t2XW2qDMAJg7cpwyvkTD31FX3ldHtoeT5fkkNwzBCDYiHDB8Rs2ShbnXb9u6SdhA=w2400
mathjax: true
---

# Transport Module - microSDV

The transport module was designed with the following characteristics in mind:
- Manufacturing
  - Fast manufacturing
  - Replicable
  - Low cost
- Basic unit on a Fleet Management System (FMS)
- Optimized size for desktop manufacturing equipment

## Mind Map
With these considerations, the following mind map was designed, aiming to provide a tree of possible options to different design arquitectures.

[Mind Map]:https://lh3.googleusercontent.com/pw/AJFCJaWGYCNRNBE5FYuvXPsg3QFc-eF-jMe2TvyWld7_uHiC3sMo9i_mCS8AmDO3ic1GaMdp5wOrEzrIrrN9ZGOADHD4huRVXy7cz4OA34TcYo7t2HxWgUs=w2400

## SDV
Likewise, the Self Driving Vehicle model was elected for navigation, instead of an Autonomous Guided Vehicle one, as the routing flexibility (capability of reordering the production steps) of the Microfactory would be the reduced with the second option, as well as the necessity of placing reference beacons or guidelines which would increase the implementation cost.

## Axiomatic Design
Given that this robot will reduce custom built components to replace with Commercial Off The Shelf (COTS) components, the axiomatic design on tihs robot was only applied up to matrix B, because manufacturing processes parameters will be reduced as much as possible.

Matrix A
| Requirement    | ROS | Differential Configuration    | LIDAR | PRIA Integrated | Containerized |
| -------- | ------- | -------- | ------- | -------- | ------- |
| Autonomous Driving         | x | 0 | 0  | 0 | 0  |
| 360 degree movement        | 0 | x | 0  | 0 | 0  |
| 360 degree perceptiom      | 0 | 0 | x  | 0 |0   |
| Lab Integrated             | 0 | 0 | 0  | x | 0  |
| Replicable                 | 0 | 0 | 0  | 0 | x  |
{:class="custom-table"}

Matrix B
| Requirement    | Raspberry Pi | Gearmotor   | RPLIDAR A1 | TurtlePRIA | Docker |
| -------- | ------- | -------- | ------- | -------- | ------- |
| ROS                        | x | 0 | 0  | 0 | 0  |
| Differential Configuration | 0 | x | 0  | 0 | 0  |
| LIDAR                      | 0 | 0 | x  | 0 |0   |
| PRIA Integrated            | 0 | 0 | 0  | x | 0  |
| Containerized              | 0 | 0 | 0  | 0 | x  |
{:class="custom-table"}

## Differential Configuration

Afterwards, the traction system was designed. The differential model allows turning over a point, in other words, the robot's central axis perpendicular to the ground. This is important as it reduces space required for maneuvering in small areas, unlike the Ackermann model.

![Kinematic Model Diagram](https://www.researchgate.net/profile/Davide-Spinello/publication/282918322/figure/fig1/AS:337397943422976@1457453346302/Kinematic-model-of-a-differential-drive-mobile-robot.png)

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

## Electronic Equipment

Due to the necessity of a high complexity task item and an integrated process two boards to execute each task group was selected. The first board is a [Raspberry Pi 3B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b/), that works as the ROS Master node, as well as the LIDAR and task sequence execution. The second board is a [BeagleBone Blue](https://beagleboard.org/blue), another Single Board Computer with a robotics cap integrated in it (DC Motor drivers, servo motors, IMU, etc.).

The BBBlue has two TB6612FNG dual motor drivers, and are controlling two Pololu [78:1 Metal Gearmotor 20Dx43L mm 6V](https://www.pololu.com/product/3453). As well, the board is powered by a 2S 1000mAh battery, that connects directly with the balance port to the on-board connector.

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

