# SKY130-Phisical Verification Workshop Using SkyWater 130nm Technology

![Docker command](/Images/PV.jpg)

Here you can find all the material that I developed from the SKY 130 PV workshop during the period 10 October 2022 to 14 October 2022.

## Day 1 - Introduction to SkyWater SKY130 (Lab Instance)

Since the tools are already installed, let us first check the correct operation of the tools.

Let us first try with Magic.

![Docker command](/Day1_images/1.PNG)
We use the "magic" command in the terminal to invoke Magic.

Then, let us try with Netgen.
![Docker command](/Day1_images/2.PNG)
We use the "netgen" command in the terminal to invoke Netgen.

Then, let us try with Xschem.
![Docker command](/Day1_images/3.PNG)
We use the "xschem" command in the terminal to invoke Xschem.

Finally, let us try with Ngspice.
![Docker command](/Day1_images/4.PNG)
We use the "ngspice" command in the terminal to invoke Ngspice. To leave Ngspice, just use type "quit" in the command line.

There exist other possibilities for Magic and Netgen. Let us check magic without the console.
![Docker command](/Day1_images/5.PNG)
We use the "magic -noconsole" command in the terminal to invoke Magic without the console.

Then for Netgen.
![Docker command](/Day1_images/6.PNG)
We use the "netgen -noconsole" command in the terminal to invoke Netgen without the console.

Another possibility for Magic is to run Magic without graphics.
![Docker command](/Day1_images/7.PNG)
We use the "magic -dnull -noconsole" command in the terminal to invoke Magic without the graphic interface, which is used while running scripts.

Next we present an exemple for the batch mode.
![Docker command](/Day1_images/8.PNG)
We create a .tcl file for testing the batch mode with the command "cat > test.tcl". We use the following commands to run the different tools in the batch mode: "magic -dnull -noconsole test.tcl", "netgen -batch souce test.tcl" and "xschem --tcl test.tcl -q".  

For Ngspice, the tool doesn't use the tcl language, so it is necessary to look in the Ngspice documentation for better understanding. It is possible to run it in bash mode but we cannot include an input, so it is not very useful.
![Docker command](/Day1_images/9.PNG)
We use the "ngspice -b" command in the terminal to invoke Netgen without the console.

Netgen has a GUI which can be run with the following command: "/usr/local/lib/netgen/python/lvs_manager.py". The problem with this GUI is that it hides many useful options, so it is not very recommended.

Fo doing a correct layout with SkyWater SKY130 PDK, we must have a correct environment in Magic, which is set with the following command:"".

### First warm-up lab, Inverter circuit.

To start the lab, we organize correctly the project by creating the following folders for each of the tools. Then let we configure and set appropriately each of the tools with the following commands.

![Docker command](/Day1_images/10.PNG)

Now, let us check the setups.

First for Xschem.
![Docker command](/Day1_images/11.PNG)

To see the examples in the right top corner: Select a project and type E.
![Docker command](/Day1_images/12.PNG)
To exit the project, type: CTRL + E.

Then let us go to Magic. We observe that the SkyWater technology is now associated to Magic.
![Docker command](/Day1_images/13.PNG)

When we open Magic with the command "magic -d XR", we observe that the layer colores are more saturated than with the command above, due to the rendering of the Cairo graphics package.
![Docker command](/Day1_images/14.PNG)

Another possible graphics package is set with the commando "", which is a littel bit faster than with Cairo graphics.
![Docker command](/Day1_images/15.PNG)

To paint and erase layers in Magic.
![Docker command](/Day1_images/16.PNG)

Instantiation of a 5V nfet and no guardring.
![Docker command](/Day1_images/17.PNG)

Now, we implement the schematic of an inverter. It is important to mention that L=150nm is only available for the SRAM transistors, thus, we use the minimum length for other applications, L=180nm. Regarding W, it is important to mention that the W parameter in the properties of the transistors corresponds to the width of all the fingers.

We present the schematic of the inverter.
![Docker command](/Day1_images/18.PNG)

#### Some useful key shortcuts for the differente tools.


##### For Xschem
| For Xschem  | For Magic | 
| ------------- | ------------- |
| E --> Down in hierarchy  | Z --> Zoom in  |
| CTRL + E --> Up in hierarchy   | Shift + Z --> Zoom out  |
| F --> Full view of main window   | While mouse on a layer, P --> paint the box with the layer  |
|  Shift + I --> Insert a device  | While mouse on a layer, E --> delete the box with the layer  |
|  Shift + R --> Rotate a device  | V --> For view a complete layout  |
|  C --> Copy a device  | S + "what" in the command line--> Device type  |
|  Q --> Change properties of a device  |   |
|  W --> Wire the devices  |   |

## Day 2 -  (Lab Instance)

