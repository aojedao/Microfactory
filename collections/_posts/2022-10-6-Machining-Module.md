---
layout: article
title: Machining Module
abstract: An overview of this theme, including highlights and instructions of use
categories: misc
tags: antarctica theme
eyeCatcher: https://refstatic.sk/article/od-antarktidy-sa-odtrhol-jeden-z-najvacsich-zaznamenanych-ladovcov-v-historii~25d92685f6b3b93aefe6.jpg?is=1440x513c&ic=0x439x1579x562&c=2w&s=e0efd5864ab1b7e41b8c7f5e0912f84b404ca30a53df8461106b377c19a24a21
---

---
![4U6cuV.png](https://z3.ax1x.com/2021/09/22/4U6cuV.png)

The machining module has two machines integrated in one model, the CNC and admission module.

The CNC machine was designed by [Julian Luna](https://github.com/juflunaca) and [Alejandro Ojeda](https://github.com/aojedao) for the Sensors and Actuators class at UNAL. You can view the design document in the following link.

[Design Paper](https://www.overleaf.com/read/fgbdprymfzjc)



## CNC 
* The machine has a 30 cm x 20 cm work area. 
* The machine mills using a Dremel 4000 and a 30° V type milling bit.
* The machine controller uses an Arduino UNO and a CNC shield board. The firmware running is [GRBL](https://github.com/grbl/grbl).


## File preparation

 But before that the process needed to design the board requires the following steps.

1. ECAD files: A normal gerber file must be loaded per face to mill, this microfactory was concieved using only one face of the PCW, therefore only one is needed. There are multiple Electronic Computer Aided Design (ECAD) tools like [KiCAD](https://www.kicad.org/), [Proteus](https://www.labcenter.com/) and [Autodesk Fusion 360](https://www.autodesk.com/products/fusion-360/overview?term=1-YEAR&tab=subscription). Export the gerber files for the next step.

2. CAM files: Here the is where the Gcode files are produced, so there are multiple options to generate these files. Due to the application I recommend using [FlatCAM](http://flatcam.org/), as it has automatic tools for PCB production, like automatic cutting depht calculation given a mill angle and type. 

3. Gcode Sender: Files must be loaded into the Gcode sender software, they only recieve G code files, like `.gcode` or `.nc` files. There are two Gcode sender softwares being used in the moment, [Universal Gcode Sender](https://winder.github.io/ugs_website/) and [CNCjs](https://cnc.js.org/). CNCjs offers the capability of being installed in a debian device (like Raspberry Pi) to be used over a local network via UI at the device IP. Both have the capaciblity of controlling the machine position and reconfiguring GRBL parameters, like `mm/rev` or maximum velocities.


## Roadmap

| Feature                       | Module           | Status |
| :---------------------------- | :--------------: | :----: |
| CNC Machine Assembly          | CNC              | √      |
| CNC repetability test         | CNC              | √      |
| CNC PCW milling test          | CNC              |        |
| Admission module assembly     | Admission        |        |
| Admission module integration  | Admission        |        |
| MicroSDV integration          | CNC + Admission  |        |
