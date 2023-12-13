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
* The machine has a 50 cm x 25 cm work area. 
* The machine mills using a Dremel 4000 and a 30° V type milling bit.
* The machine controller uses an Arduino UNO and a CNC shield board. The firmware running is [GRBL](https://github.com/grbl/grbl).


## File preparation

 But before that the process needed to design the board requires the following steps.

1. ECAD files: A normal gerber file must be loaded per face to mill, this microfactory was concieved using only one face of the PCW, therefore only one is needed. There are multiple Electronic Computer Aided Design (ECAD) tools like [KiCAD](https://www.kicad.org/), [Proteus](https://www.labcenter.com/) and [Autodesk Fusion 360](https://www.autodesk.com/products/fusion-360/overview?term=1-YEAR&tab=subscription). Export the gerber files for the next step.

2. CAM files: Here the is where the Gcode files are produced, so there are multiple options to generate these files. Due to the application I recommend using [FlatCAM](http://flatcam.org/), as it has automatic tools for PCB production, like automatic cutting depht calculation given a mill angle and type. 

3. Gcode Sender: Files must be loaded into the Gcode sender software, they only recieve G code files, like `.gcode` or `.nc` files. There are two Gcode sender softwares being used in the moment, [Universal Gcode Sender](https://winder.github.io/ugs_website/) and [CNCjs](https://cnc.js.org/). CNCjs offers the capability of being installed in a debian device (like Raspberry Pi) to be used over a local network via UI at the device IP. Both have the capaciblity of controlling the machine position and reconfiguring GRBL parameters, like `mm/rev` or maximum velocities.

## Mechanical Design
### Torque Calculation

The calculation of torque is carried out based on the torque equation for lifting a piece according to Shigley. \cite{shigley1985mechanical}

$$T_R=\frac{F d_m}{2} (\frac{l + f \pi d_m}{\pi d_m - f l})$$

Where:

$T_R$ is the required torque.
$d_m$ is the mean diameter, or $d_m = d - 0.5p$, where $d$ is the outer diameter and $p$ the pitch
$f$ is the friction coeficient
$l$ is the lead
$F$ is the axial force

And reordering the equation it was obtained that

$$\frac{2 T_R}{d_m} (\frac{ \pi d_m - f l} {l + f \pi d_m}) = F$$

The obtained force for the machine is $T_R=0.308 \, Nm$.

### Angle Brackets  

Angle brackets were designed to support the base structure forces, all the aluminium profiles connect the ends where angle brackets provide support. Due to the load they willl support during the machine operation, forces over the bracket were analyzed, to provide a Factor of Safety (FoS) on the design. The tool used to analyze the FoS was finite element simluation in Autodesk Fusion 360. The force over the bracket is calculated as follows:

$T_R=0.014 \, Nm$
$d_m=0.007 \, m$
$f=0.15$, the coeficient between steel and bronze.
$l=0.008 \,m$
$$F=(\frac{2 T_R}{d_m}) (\frac{\pi d_m - fl}{l + f\pi d_m})=7.636 \,N$$

In the scenario where only one bracket is tightened correctly the following figure shows the FoS at the end of the aluminium profile. In this case is $2.948$.

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=90mm]{Kap2/PCBMill/anglebracket1.png}
    	\caption{FoS of the aluminium profile due to the failure of angle bracket}
    	\label{fig:}
    \end{figure}

    ![anglebracket1](anglebracket1.png)

    For the bracket itself (in the same scenario) the response is shown in the following figure, with a value of $1.209$.

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=90mm]{Kap2/PCBMill/anglebracket1support.png}
    	\caption{FoS on the Angle bracket for the worst case scenario}
    	\label{fig:anglebracket2}
    \end{figure}

    In the general case, the structure will count with two brackets, each with 4 fasteners, so the model obtained for this is shown in the next figure, where the minimal FoS is $8.45$.

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=90mm]{Kap2/PCBMill/anglebracket4support.png}
    	\caption{FoS on the Angle bracket in its standard functioning}
    	\label{fig:anglebracket4}
    \end{figure}


### Additive Manufacturing

All the printed parts were manufactured in the FDM printer ar the LabFabEx. White PLA by 3DBots was used, with $0.8\,mm$ walls, $70\%$ cubic infill, raft support and a $50\,mm$ printing speed.

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=120mm]{Kap2/manufactura15.jpg}
    	\caption{Axis support printing}
    	\label{fig:manufactura15}
    \end{figure}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=60mm]{Kap2/manufactura13.jpg}
    	\caption{X axis support fit in the aluminium profile}
    	\label{fig:manufactura13}
    \end{figure}
    
    
    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=60mm]{Kap2/manufactura7.jpg}
    	\caption{Motor, support and base corner fit.}
    	\label{fig:manufactura7}
    \end{figure}
### Geometric Dimensioning and Tolerancing}

In order to verify the manufacturing process multiple tolerancing methods were monitored and executed.

In order to verify the tolerance of the 3D FDM Printer at the laboratory, a test tool to validate the tolerance was designed and printed. The model is shown in figure \ref{fig:tolerance}. 

\begin{figure}[!ht]
      \centering
    	\includegraphics[width=120mm]{Kap2/tolerance.png}
    	\caption{Printing tolerance validation piece.}
    	\label{fig:tolerance}
    \end{figure}

The measurements were registered in table \ref{tab:tolerancemeasure}, and the error was calculated in it. Measurements were taken using a caliper.

|Reference | Measurement No | Value | Mean Value | Error value |  Absolute Error | Nominal Value | Value with tolerance | Relative error  |
| :------  |:---            | :---  | :---       | :---        | :---            | :---          | :---                 | :---            |
| 1        | 1              | 22,10 | 22,13      | 0,12        | 0,37            | 23,00         | 22,50                | 0,25            |
| ~ | 2 | 22,08 | ~ | ~ | ~ | ~ | ~ | ~ |
| ~ | 3 | 22,20 | ~ | ~ | ~ | ~ | ~ | ~ |
| 2 | 1 | 15,12 | 15,18 | 0,14 | 0,32 | 16,00 | 15,50 | 0,18 |
| ~ | 2 | 15,16 | ~ | ~ | ~ | ~ | ~ | ~ |
| ~ | 3 | 15,26 | ~ | ~ | ~ | ~ | ~ | ~ |
| 3 | 1 | 8,12 | 8,13 | 0,16 | 0,37 | 9,00 | 8,50 | 0,21 |
| ~ | 2 | 8,06 | ~ | ~ | ~ | ~ | ~ | ~ |
| ~ | 3 | 8,22 | ~ | ~ | ~ | ~ | ~ | ~ |
| 4 | 1 | 2,60 | 2,64 | 0,10 | -0,14 | 3,00 | 2,50 | -0,24 |
| ~ | 2 | 2,62 | ~ | ~ | ~ | ~ | ~ | ~ |
| ~ | 3 | 2,70 | ~ | ~ | ~ | ~ | ~ | ~ |
| 5.1 | 1 | 5,06 | 4,99 | 0,34 | 0,51 | 5,00 | 5,50 | 0,17 |
| ~ | 2 | 4,78 | ~ | ~ | ~ | ~ | ~ | ~ |
| ~ | 3 | 5,12 | ~ | ~ | ~ | ~ | ~ | ~ |
| 5.2 | 1 | 4,20 | 4,40 | 0,76 | 0,10 | 4,00 | 4,50 | -0,66 |
| ~ | 2 | 4,12 | ~ | ~ | ~ | ~ | ~ | ~ |
| ~ | 3 | 4,88 | ~ | ~ | ~ | ~ | ~ | ~ |
| 5.3 | 1 | 11,08 | 11,69 | 0,92 | -0,41 | 10,78 | 11,28 | -1,33 |
| ~ | 2 | 12,00 | ~ | ~ | ~ | ~ | ~ | ~ |
| ~ | 3 | 11,98 | ~ | ~ | ~ | ~ | ~ | ~ |
| 5.4 | 1 | 1,84 | 1,83 | 0,06 | -0,33 | 2,00 | 1,50 | -0,39 |
| ~ | 2 | 1,86 | ~ | ~ | ~ | ~ | ~ | ~ |



Small holes were also measured using the Zoller machine at the laboratory. The error perceived for the reference $1$ was $1.0740\,mm$, for reference $4$ is $0.088\,mm$, for reference $5.1$ is $0.037\,mm$ and for reference $5.4$ is $0.019\,mm$.

Errors found in the measurements are shown in graph \ref{fig:toleranceeror}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=120mm]{Kap2/Tolerance error in the reference piece.png}
    	\caption{Tolerance error in the reference piece.}
    	\label{fig:toleranceeror}
    \end{figure}

After knowing the tolerances for the machine production quality, and given that multiple Commercial Off The Shelve parts were selected, a Shaft-based system was selected. 

    For clearance fit, parts should be able to move slightly.
    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=120mm]{Kap2/PCBMill/gdtclearanceval.png}
    	\caption{Clearance Fit.}
    	\label{fig:gdtclearance}
    \end{figure}
    Transition fit tries to make objects are firm but not under pressure, making it easy to remove.
    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=120mm]{Kap2/PCBMill/gdttransitionval.png}
    	\caption{Transition Fit.}
    	\label{fig:gdttransition}
    \end{figure}
Interference fit is used to use pressure to place an element on another, typically to be fixed permanently.
    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=120mm]{Kap2/PCBMill/gdtinterferenceval.png}
    	\caption{Interference Fit.}
    	\label{fig:gdtinterference}
    \end{figure}

Every element that was manufactured using Additive Manufacturing defined the type of fit that was required. For the X axis support it's elements fit are shown in figure \ref{fig:gdt1}. Y axis supports are shown in figure \ref{fig:gdt2}.

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=150mm]{Kap2/PCBMill/gdt1.png}
    	\caption{Fit for X axis supports.}
    	\label{fig:gdt1}
    \end{figure}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=70mm]{Kap2/PCBMill/gdt2.png}
    	\caption{Y axis supports fit.}
    	\label{fig:gdt2}
    \end{figure}


    For the Dremel carriage two parts are defined, the Z axis support and the Dremel carraige itself. The first one is shown in figure \ref{fig:gdt3}, and the second one in figure \ref{fig:gdt4} and \ref{fig:gdt5}.

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=70mm]{Kap2/PCBMill/gdt3.png}
    	\caption{Z axis support fit.}
    	\label{fig:gdt3}
    \end{figure}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=70mm]{Kap2/PCBMill/gdt4.png}
    	\caption{Fit for Dremel carriage (Front).}
    	\label{fig:gdt4}
    \end{figure}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=70mm]{Kap2/PCBMill/gdt5.png}
    	\caption{Fit for Dremel carriage (Back).}
    	\label{fig:gdt5}
    \end{figure}

    
For the assembly process the following aspects of geometric control were used:

\begin{itemize}
    \item Shape
    \begin{itemize}
        \item Straightness
        \item Flatness
    \end{itemize}
    \item Orientation
    \begin{itemize}
        \item Parallelism
        \item Perpendicularity
        \item Angularity

    \end{itemize}
\end{itemize}

    The following coordinate axis was used as reference, shown in figure \ref{fig:gdyt1}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=80mm]{Kap2/PCBMill/gdyt1.png}
    	\caption{Coordinate axis.}
    	\label{fig:gdyt1}
    \end{figure}

    First straightness was checked for the X axis \ref{fig:gdyt2}, using a dial gauge on the surface \ref{fig:gdyt3} and the Y axis \ref{fig:gdyt4}.

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=100mm]{Kap2/PCBMill/gdyt2.png}
    	\caption{Straightness of X axis.}
    	\label{fig:gdyt2}
    \end{figure}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=120mm]{Kap2/PCBMill/gdyt3.png}
    	\caption{Usage of the dial gauge.}
    	\label{fig:gdyt3}
    \end{figure}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=100mm]{Kap2/PCBMill/gdyt4.png}
    	\caption{Straightness of the Y axis.}
    	\label{fig:gdyt4}
    \end{figure}

    Parallellism was checked between the axis rod and the leadscrew \ref{fig:gdyt5} using the method shown in figure \ref{fig:gdyt6}. As well as one Y axis \ref{fig:gdyt7} using the method in figure \ref{fig:gdyt8}.

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=120mm]{Kap2/PCBMill/gdyt5.png}
    	\caption{Aluminium profile cuts for each phase.}
    	\label{fig:gdyt5}
    \end{figure}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=80mm]{Kap2/PCBMill/gdyt6.png}
    	\caption{Aluminium profile cuts for each phase.}
    	\label{fig:gdyt6}
    \end{figure}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=100mm]{Kap2/PCBMill/gdyt7.png}
    	\caption{Aluminium profile cuts for each phase.}
    	\label{fig:gdyt7}
    \end{figure}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=80mm]{Kap2/PCBMill/gdyt8.png}
    	\caption{Aluminium profile cuts for each phase.}
    	\label{fig:gdyt8}
    \end{figure}
    
    Perpendicularity was verified between the X and Y axis \ref{fig:gdyt9}.
    
    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=100mm]{Kap2/PCBMill/gdyt9.png}
    	\caption{Aluminium profile cuts for each phase.}
    	\label{fig:gdyt9}
    \end{figure}

    Finally, angularity was studied between the Z axis vs the XY plane \ref{fig:gdyt10} using the method shown in figure \ref{fig:gdyt11}.

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=120mm]{Kap2/PCBMill/gdyt10.png}
    	\caption{Aluminium profile cuts for each phase.}
    	\label{fig:gdyt10}
    \end{figure}

    \begin{figure}[!ht]
      \centering
    	\includegraphics[width=120mm]{Kap2/PCBMill/gdyt11.png}
    	\caption{Aluminium profile cuts for each phase.}
    	\label{fig:gdyt11}
    \end{figure}


## Parameter Adjustment

First set the maximum current allowed by the DRV8825 drivers by adjusting the potentiometer (with a philips screwdriver) voltage according to the following expression.

$$V_{ref} = \frac{I_{max}}{2}$$

Afterward, the $\frac{mm}{rev}$ units must be configured to verify the machine's correct precision. For this use the following equation:

$$\frac{motor \; steps \; per \; revolution}{leadscrew \; pitch \; (mm \; per \; rev)}\cdot microsteps$$

$$= \frac{200}{8}\cdot 2 = 50 $$

And afterward configure the advance per axis with the code `$100=50`. Do this as well for 101 and 102.

The driver microsteps are configured trough jumpers in the CNC Shield. The pins in the connectors on M0, M1 and M2.

![Shield.png](https://osoyoo.com/wp-content/uploads/2017/04/Arduino-CNC-Shield-V3-Layout.png)

## 3D Model test



<script src="https://embed.github.com/view/3d/aojedao/microfactory/dev/assets/stl/ASX1_AyASCII.stl"></script>


## Roadmap

| Feature                       | Module           | Status |
| :---------------------------- | :--------------: | :----: |
| CNC Machine Assembly          | CNC              | √      |
| CNC repetability test         | CNC              | √      |
| CNC PCW milling test          | CNC              | √      |
| Admission module assembly     | Admission        |        |
| Admission module integration  | Admission        |        |
| MicroSDV integration          | CNC + Admission  |        |
