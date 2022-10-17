---
layout: article
title: Machining Module
abstract: An overview of the CNC module, it's design and how it works.
categories: misc
tags: antarctica theme
eyeCatcher: https://lh3.googleusercontent.com/Yo0C6PLjh2yv660AlKndmPzjhnVhvCVqA9ok0jv8Z9l4nC9KBwjdLe2mmRvRK0PBr3kulFGUyM339tQ9Tw2wDxR6iZ2-57AcEUN8oT_UZKkf5wrWqnNpZizUWI4VNJ5flnxtkSLN9w=w2400
mathjax: true
---

---
![CNC.png](https://lh3.googleusercontent.com/Yo0C6PLjh2yv660AlKndmPzjhnVhvCVqA9ok0jv8Z9l4nC9KBwjdLe2mmRvRK0PBr3kulFGUyM339tQ9Tw2wDxR6iZ2-57AcEUN8oT_UZKkf5wrWqnNpZizUWI4VNJ5flnxtkSLN9w=w2400)

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

First set the maximum current allowed by the DRV8825 drivers by adjusting the potentiometer (with a philips screwdriver) voltage according to the following expression.

$$V_{ref} = \frac{I_{max}}{2}$$

Afterward, the $\frac{mm}{rev}$ units must be configured to verify the machine's correct precision. For this use the following equation:

$$\frac{motor steps}{lead or mm per rev on the leadscrew}\cdot microsteps$$
$$\frac{200}{8}\cdot 2 = 50 $$

And afterward configure the advance per axis with the code `$100=50`. Do this as well for 101 and 102 


## Roadmap

| Feature                       | Module           | Status |
| :---------------------------- | :--------------: | :----: |
| CNC Machine Assembly          | CNC              | √      |
| CNC repetability test         | CNC              | √      |
| CNC PCW milling test          | CNC              | √      |
| Admission module assembly     | Admission        |        |
| Admission module integration  | Admission        |        |
| MicroSDV integration          | CNC + Admission  |        |
