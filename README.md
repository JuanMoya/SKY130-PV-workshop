# SKY130-Phisical Verification Workshop Using SkyWater 130nm Technology

![Docker command](/Images/PV.jpg)

Here you can find all the material that I developed from the SKY 130 PV workshop during the period 10 October 2022 to 14 October 2022.

## Day 1 - Introduction to SkyWater SKY130 (Lab Instance)

Since the tools are already installed, let us first check the correct operation of the tools.

Let us first try with Magic.

![Docker command](/Day1_images/1.PNG)

We use the **magic** command in the terminal to invoke Magic.

Then, let us try with Netgen.

![Docker command](/Day1_images/2.PNG)

We use the **netgen** command in the terminal to invoke Netgen.

Then, let us try with Xschem.

![Docker command](/Day1_images/3.PNG)

We use the **xschem** command in the terminal to invoke Xschem.

Finally, let us try with Ngspice.

![Docker command](/Day1_images/4.PNG)

We use the **ngspice** command in the terminal to invoke Ngspice. To leave Ngspice, just use type **quit** in the command line.

There exist other possibilities for Magic and Netgen. Let us check magic without the console.

![Docker command](/Day1_images/5.PNG)

We use the **magic -noconsole** command in the terminal to invoke Magic without the console.

Then for Netgen.

![Docker command](/Day1_images/6.PNG)

We use the **netgen -noconsole** command in the terminal to invoke Netgen without the console.

Another possibility for Magic is to run Magic without graphics.

![Docker command](/Day1_images/7.PNG)

We use the **magic -dnull -noconsole** command in the terminal to invoke Magic without the graphic interface, which is used while running scripts.

Next we present an example for the batch mode.

![Docker command](/Day1_images/8.PNG)

We create a .tcl file for testing the batch mode with the command **cat > test.tcl**. We use the following commands to run the different tools in the batch mode: **magic -dnull -noconsole test.tcl**, **netgen -batch souce test.tcl**, and **xschem --tcl test.tcl -q**.  

For Ngspice, the tool doesn't use the tcl language, so it is necessary to look in the Ngspice documentation for better understanding. It is possible to run it in bash mode but we cannot include an input, so it is not very useful.

![Docker command](/Day1_images/9.PNG)

We use the **ngspice -b** command in the terminal to invoke Netgen without the console.

Netgen has a GUI which can be run with the following command: **/usr/local/lib/netgen/python/lvs_manager.py**. The problem with this GUI is that it hides many useful options, so it is not very recommended.

Fo doing a correct layout with SkyWater SKY130 PDK, we must have a correct environment in Magic, which is set with the following command: **magic -d XR**.

### First warm-up lab, Inverter circuit.

To start the lab, we organize correctly the project by creating the following folders for each of the tools. Then let we configure and set appropriately each of the tools with the following commands.

![Docker command](/Day1_images/10.PNG)

Now, let us check the setups.

First for Xschem.

![Docker command](/Day1_images/11.PNG)

To see the examples in the right top corner: Select a project and type **E**.

![Docker command](/Day1_images/12.PNG)

To exit the project, type: **CTRL + E**.

Then let us go to Magic. We observe that the SkyWater technology is now associated to Magic.

![Docker command](/Day1_images/13.PNG)

When we open Magic with the command **magic -d XR**, we observe that the layer colores are more saturated than with the command above, due to the rendering of the Cairo graphics package.

![Docker command](/Day1_images/14.PNG)

Another possible graphics package is set with the commando **magic -d OGL**, which is a littel bit faster than with Cairo graphics.

![Docker command](/Day1_images/15.PNG)

To paint and erase layers in Magic.

![Docker command](/Day1_images/16.PNG)

Instantiation of a 5V nfet and no guardring.

![Docker command](/Day1_images/17.PNG)

Now, we implement the schematic of an inverter. It is important to mention that L=150nm is only available for the SRAM transistors, thus, we use the minimum length for other applications, L=180nm. Regarding W, it is important to mention that the W parameter in the properties of the transistors corresponds to the width of all the fingers. Once we finish the schematic, we need to generate the symbol. We click on the Symbol menu and then on "Make symbol from schematic".

We present the schematic of the inverter.

![Docker command](/Day1_images/18.PNG)

Configuration of the input PWL source.

![Docker command](/Day1_images/19.PNG)

Configuration of the device model path and specification of the tt corner. 

![Docker command](/Day1_images/20.PNG)

Configuration of the transient simulation. 

![Docker command](/Day1_images/21.PNG)

Inverter simulation. 

![Docker command](/Day1_images/22.PNG)

We select the "LVS netlist: Top level is a .subckt" option in the inverter schematic.

![Docker command](/Day1_images/23.PNG)

The we click on "Netlist" and exist Xschem.

For importing the schematic in Magic, open Magic, then click on the File menu and then on the option "Import SPICE". Remember that the schematic is in the xschem folder.

![Docker command](/Day1_images/24.PNG)

Configuration of the PFET guarding and drain and source contacts.

![Docker command](/Day1_images/25.PNG)

Configuration of the NFET guarding and drain and source contacts.

![Docker command](/Day1_images/26.PNG)

Once the layout is finished, click on Save, and then in autowrite.

![Docker command](/Day1_images/27.PNG)

Type the following commands for a first extraction. **extract do local** write all the results in the local directory, whereas **extract all** does the actual extraction.

![Docker command](/Day1_images/28.PNG)

Then set the **ext2spice lvs** command to set the netlist generator for hierarchical spice output, ngspice format and no parasitic extraction, then **ext2spice** to create the spice netlist. We type **quit** to end Magic. In the mag/ folder we remove the intermediate results files with the commands **rm *.ext** and **/usr/share/pdk/bin/cleanup_unref.py -remove .**. 

For LVS comparison, we use the following command in the netgen folder: netgen -batch lvs **"../mag/inverter.spice inverter" "../xschem/inverter.spice inverter"**.

![Docker command](/Day1_images/29.PNG)

Finally, we go back to magic, open the inverter layout with **magic -d XR inverter** and run the following commands.

![Docker command](/Day1_images/30.PNG)

It is important to mention that the **ext2spice lvs** command is used for LVS, whereas using **ext2spice lvs** and  **ext2spice cthresh 0** commands incorporate the parasitics for netgen simulation.

We copy the testbench from xschem in the mag/ folder with: **cp ../xschem/inverter_tb.spice**. Then, we compare the order of the ports of the extracted layout with the ones defined in the inverter testbench. Moreover, we delete the section in red in the following figure to make an inclusion of the layout extracted netlist.

![Docker command](/Day1_images/31.PNG)

![Docker command](/Day1_images/32.PNG)

We create the .spiceinit file in the mag folder like we created in the xschem directory with the command **cp ../xschem .spiceinit**. 

Finally we run the simulation with the command **ngspice inverter_tb spice**.

![Docker command](/Day1_images/33.PNG)



## Day 2 -  Labs for GDS read write extraction, DRC, LVS and XOR setup (Lab Instance)


#### Some useful key shortcuts and commands for the differente tools.


##### For Xschem and Magic
| For Xschem  | For Magic | 
| ------------- | ------------- |
| E --> Down in hierarchy  | Z --> Zoom in  |
| CTRL + E --> Up in hierarchy   | Shift + Z --> Zoom out  |
| F --> Full view of main window   | While mouse on a layer, P --> paint the box with the layer  |
|  Shift + I --> Insert a device  | While mouse on a layer, E --> delete the box with the layer  |
|  Shift + R --> Rotate a device  | V --> For view a complete layout  |
|  C --> Copy a device  | S + "what" in the command line--> Device type  |
|  Q --> Change properties of a device  |  I --> For selecting the desired instance; Once selected with I and then CTRL + P to see the properties of the selected device|
|  W --> Wire the devices  |  I + M to move the desired device |
|   |  S + X --> expand the layout |
|   | > --> Descend in hierarchy |
|   | B --> Show coordinates, area |
|   | ? --> Ask drc error |
|   | : erase "name layer" --> erase the layer selected in the box |
|   | : CIF "name layer" --> shows the layer selected in the box |
|   | : side --> flip shape
|   | : splitpaint sw "name layer" --> create triangular of the layer selected in the box (south west position of the box filled) ||
|   | : rotate 90 --> rotates 90° a figure ||
|   | : CIF cover MET1 --> informs density of MET1 ||

