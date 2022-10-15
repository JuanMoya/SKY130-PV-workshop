# SKY130-Phisical Verification Workshop Using SkyWater 130nm Technology

<img src="https://github.com/JuanMoya/SKY130-PV-workshop/blob/main/Images/PV.jpg" width="800" height="300" />


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

To see the possible styles and the current styles in Magic, we type the folowing commands.

![Docker command](/Day2_images/1.PNG)

To read the gds files (library), we use the command in Magic **gds read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/gds/sky130_fd_sc_hd.gds**.

To see the name of the top cells, we run the command **cellname top** or we in the magic menu we can go to "Options" and then click on "Cell Manager".

If it is desirable to change the style of a cell, we can type the following commands in Magic: **cif istyle sky130()** and then rerun **gds read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/gds/sky130_fd_sc_hd.gds**.

If we don't want to overwrite existing cells when reading from gds, we type the command **gds noduplicates** to see if it is activate or not. If it not activate, a "0" should appear, the we type **gds noduplicates true**.

![Docker command](/Day2_images/2.PNG)

If it is desirable to deactive the noduplicates option, we type **gds noduplicates flase** and rerun **gds read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/gds/sky130_fd_sc_hd.gds**.

To see the port index, we select a port (only once at a time) and we type **port index**.

If we want information about a specific port we type the following commands:

* **port first**
* **port 1 name**
* **port 1 class**
* **port 1 use**

To get to the lef data we use the following command in Magic: **lef read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/lef_sky130_fd_sc_hd.lef**. We run the following commands to get more information about the ports, which was not possible with the gds file.

![Docker command](/Day2_images/3.PNG)

To read the spice file we use the folowing command in Magic: **readspice /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/lef_sky130_fd_sc_hd.spice**.

With the latter, we identify that the port names match with the spice netlist.

![Docker command](/Day2_images/4.PNG)

Now, let us look the abstract view.

We type the following commands:

* **lef read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/lef_sky130_fd_sc_hd.lef**.

![Docker command](/Day2_images/5.PNG)

If there is a port order problem, we can solve with **readspice /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/lef_sky130_fd_sc_hd.spice**.

![Docker command](/Day2_images/6.PNG)

We create a new magic file named test with the command in Magic **load test**.

To call the and2_1 cell, we use the command **getcell sky130_fd_sc_hd__and2_1**.

If we quit and type **gds read test** we observe again the test cell. But as expected there is an error since magic cannot write abstract views into gds.

![Docker command](/Day2_images/7.PNG)

Use the command **property** to see all the metada information.

To force the vendor cell to be editable, we type in Magic the commands:

![Docker command](/Day2_images/8.PNG)

Then we type the following commands to see that the cell has the gds properties, which makes it readonly cell:

* **load sky130_fd_sc_hd__and2_1**
* **property**

Now, let us look at extraction:

Command for extraction:
* **extract all**

Then, commands for lvs:

* **ext2spice lvs**
* **ext2spice**

Then, commands to consider parasitics capacitances:

* **ext2spice cthresh 0**
* **ext2spice**

For the RC parasitics extraction:

* **ext2sim labels on**
* **ext2sim*
* **extresist tolerance 10**
* **extresist**

![Docker command](/Day2_images/9.PNG)

To get the final value with both resistances and capacitances parasitics, we use the following commands:

* **ext2spice lvs**
* **ext2spice cthresh 0.01**
* **ext2spice exresist on**
* **ext2spice**

An then we open the file "sky130_fd_sc_hd__and2_1.spice"

![Docker command](/Day2_images/10.PNG)

Let us run now batch drc, for this, we use the following commands:

![Docker command](/Day2_images/11.PNG)

As expected, there are some errors due to the lack of connections to substrate.

To see the actual drc style in Magic, type **drc style**. If it is desirable to see the different drc styles, type **drc listall style**.

The batch editor always run the drc(full) style, contraty to the Magic GUI. So to change the drc style to drc(full) we type **drc style drc(full)**. Then, we apply the **drc check** command.

![Docker command](/Day2_images/12.PNG)

To identify the different drc errors, type **drc why** and if it is desirable to see an specific one, type **drc find**.

In another test cell call the **getcell sky130_fd_sc_hd__and2_1** command first and then **getcell sky130_fd_sc_hd__tapvpwrvgnd_1**.

![Docker command](/Day2_images/13.PNG)

Now, let us run LVS with batch.

We create a new folder named "netgen" and we copy the .tcl file as before with the command **cp -/usr/share/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl ./setup.tcl** and open the and2_1 cell in the mag folder.

We then extract parasitics with the commands:

* **ext2spice lvs**
* **ext2spice**

And then quit Magic.

Then we come back to the netgen folder and then run the following command: **netgen -batch lvs "../mag/sky130_fd_sc_hd__and2_1.spice sky130_fd_sc_hd__and2_1" "../usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/spice/sky130_fd_sc_hd.spice sky130_fd_sc_hd__and2_1"**. 

![Docker command](/Day2_images/14.png)

Now, let us do an XOR comparison. For this, go to the /mag file open Magic. Then we load the and2_1 cell. We make our own local version of the cell with the command **save altered**, and then we load it with **load altered**. 

We erase a piece of local interconnect layer connected to the output with the command typed in Magic **erase li**.

![Docker command](/Day2_images/15.png)

We create a third cell for the XOR result with the next commands:

* **flatten -nolabels xor_test**
* **load sky130_fd_sc_hd__and2_1**
* **xor -nolabels xor_test**
* **load xor_test**

![Docker command](/Day2_images/16.png)

## Day 3 -  Labs for all DRC rules (Lab Instance)

To start the drc exercices, we clone the laboraty git with the command **git clone https://github.com/RTimothyEdwards/vsd_drc_lab.git** and then run Magic with **./run_magic** in the vsd_drc_lab folder.

Let us open the first file "exercise_1" in Magic.

A first way to identify the drc errors is in Magic menu, click on "DRC", and then again click on "DRC report". Let us set the grid with the menu option "Window" and then click on "Set grid 0.10um". Additionaly, in the same menu option, click on "Snap-to-grid on" to force the cursor aligned with the grid lines.

* For exercise_1a, we select in a box the metal + the error area and then click on the middle mouse button to fill the complete area and eliminate the drc error. Another possibility is to select in a box the metal and then in magic type **: box width 0.14um**.

* For Exercise_1b, we use the following command to separate even more the two metal pieces. We select the right piece with "S" and then press the "6" key button to increase the distance.

* For Exercise_1c, we use the following command to separate even more the two metal pieces. We select the right piece with "S" and then press the "6" key button to increase the distance.

* For Exercise_1d, we use the following command to separate stretch the metal piece. We select the top piece with "S" and then press the "8" key button to increase the distance.

![Docker command](/Day3_images/1.png)

* For exercise_2a, first select the horizontal or vertical area that you want to strech, click on "A" and then "6".

* For exercise_2b, first select the area with a box, type in Magic **cif see MCON**, to see the different MCON vias placed in the area. 

* For exercise_2c, first select the area with a box, type in Magic **paint M1**. 

* For exercise_2d, first select the metal, the click on space bar, generate a wire, shift key + click on left mouse button and start increasing in metals with the different vias. To go back down, shift key + right mouse button.

![Docker command](/Day3_images/2.png)

* For exercise_3a, first select the horizontal or vertical area that you want to strech, click on "A" and then "6".

* For exercise_3b, first select in a box the inner area, type **erase M1** until the error is corrected.

![Docker command](/Day3_images/3.png)

For exercises_4, important to use the command **stret e 1um** to stretch the layers in a unique direction (e = east) and an specific distance (1um in this case).

![Docker command](/Day3_images/4.png)

For exercice 5, we use the commands in Magic **cif see DIFF**, **cif see POLY**, **cif see NSDM**, **cif see PSDM**, **cif see LVTN**, **cif see HVI**, and **cif see NPC** to see the different layers. To clear the layers, we use the command **feedback cl**.
#### Some useful key bindings and commands for the differente tools.

![Docker command](/Day3_images/5.png)

For exercise 6, we need to abstract the view of the ESD layout to edit the cell. For this, we use the following commands: 

* **I --> to select the cell.**
* **cellname path sky130_fd_io__signal_5_sym_hv_local_5term**
* **cellname path sky130_fd_io__signal_5_sym_hv_local_5term .**
* **> --> go down the hierarchy**
* **save**
* **property**

![Docker command](/Day3_images/6.png)

For exercise 7, the commands used are: 

* **splitpaint sw m1** --> to create the rectangle.
* **cif see VIA2**
* **A + "copy e 3um"**

![Docker command](/Day3_images/7.png)

For exercise 8, we generated the seal ring.

For exercise 9, we must include the tap cells.

* **getcell sky130_fd_sc_hd__tapvpwrvgnd_1**

![Docker command](/Day3_images/8.png)

For exercise 10, we use the following commands for antenna check.

* **extract do local**
* **extract all**
* **antennacheck**
* **feedback why**
* **antennacheck debug**
* **antennacheck**
* **feedback clear**
* **see no via2,m3**

![Docker command](/Day3_images/9.png)

For exercise 11, we use the following commands for density rules.
* **cif cover MET1**
* **cif cover MET2**
* In another terminal: **/usr/share/pdk/sky130A/libs.tech/magic/check_density.py exercise_11.gds**
* In another terminal: **/usr/share/pdk/sky130A/libs.tech/magic/generate_fill.py exercise_11.mag**
* In magic: ** gds read exercise_11_fill_pattern** 
* ** see no * ** 
* ** see allm2 ** 
* ** see m2fill ** 
* ** load exercise_11 ** 
* ** see * ** 
* ** box values 0 0 0 0 ** 
* ** getcell exercise_11_fill_pattern child 0 0 ** 
* ** see no * ** 
* ** see m2,m2fill  ** 

To see the top density:
* ** cif ostyle density **
* ** cif cover m2_all ** 

![Docker command](/Day3_images/10.png)


## Day 4 - Understanding PNR and physical verification (optional)

The OpenLANE flow is presented.

![Docker command](/Day4_images/1.png)

* The OpenLANE design stages

* **1) Synthesis**

* yosys --> For RTL synthesis
* abc --> For Technology mapping
* OpenSTA --> For Static timing analysis (generation of time reports)

* **2) Floorplan and PDN**

* init_fp --> For core area, rows, and tracks definition
* ioplacer --> For macro input and output ports placement
* pdn --> For power distribution network generation
* tapcell --> For welltap and decap cells insertion

* **3) Placement**

* RePlace --> For global placement
* Resizer --> For optional optimizaations on the design
* OpenDP --> For detailed placementof the globally placed components

* **4) CTS**

* TritonCTS --> For clock tree synthesis

* **5) Routing**

* FastRoute --> For global routing to generate a file for the detailed router
* CU-GR --> For global routing (another option).
* TritonRoute --> For detailed routing
* SPEF-Extractor --> For SPEF extraction

* **6) GDSII Generation**

* Magic --> For final GDSII layout file generation from the routed def
* Klayout --> For final GDSII layout file generation from the routed def as a back-up

* **7) Checks**

* Magic --> For DRC and Antenna checks
* Klayout --> For DRC checks
* Netgen --> For LVS checks
* CVC --> For Circuit Validity checks


First of all, we need to install OpenLANE, for this we follow the instructions for the short version in the next link: https://github.com/The-OpenROAD-Project/OpenLane

* First we install Docker with the following link: https://docs.docker.com/desktop/install/ubuntu/

## Day 5 - Running LVS (Labs Instance)

We download the project from git in the repository:

* git clone https://github.com/RTimothyEdwards/vsd_lvs_lab.git

For exercise 1, we use the following commands:

* **cat netA.spice**
* **diff netA.spice netB.spice** --> see the difference between both netA and netB files.
* In netgen **lvs netA.spice netB.spice**

Netgen does not care about what is in particular cell but what is the difference between two netlists. The meaningful output of netgen is the comp.out file.

![Docker command](/Day5_images/1.png)

After editing the netA.spice file, we observe that this time lvs shows that there is a mitmatch between netlists.

![Docker command](/Day5_images/2.png)

For exercise 2, we use the following commands:

* In netgen, each time we run lvs, we must use **reinitialize** to be sure that all the netlists were cleared from memory.

In this case, we use the .subckt cell in both netA.spice and netB.spice. With this option, we observe that there is a problem while running lvs. The subcircuits are not considered active components.

We can open nspice and run the netA.spice file to verify it. We use the command **ngspice netA.spice** and see that there is no error. 

![Docker command](/Day5_images/3.png)

We need to give to netgen the name of the subcircuits to avoid errors.

We use the commands in netgen:

* **reinitialize**
* **lvs "netA.spice test" "netB.spice test"**

![Docker command](/Day5_images/4.png)

Finally, we run netgen with a batch script with the following commands: 
* **cat > run_lvs.sh
#!/bin/sh
 netgen -batch lvs "netA.spice test" "netB.spice test" /usr/share/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl exercise_2_comp.out -json | tee lvs.log
 echo ""
 ../count_lvs.py exercice_2_comp_json | tee -a lvs.log** 

* **chmod a+x run_lvs.sh**

To run it, just type **./run_lvs.sh**


For exercise 3, we use the following commands:
* **./run_lvs.sh**
* **vi exercises_3_comp.out**

![Docker command](/Day5_images/5.png)

Creation of the proxy pins when some pins are missing.

![Docker command](/Day5_images/6.png)

Changing cell1 by cell4.

![Docker command](/Day5_images/7.png)

Including the blackbox option in ./run_lvs.sh

![Docker command](/Day5_images/8.png)

For exercise 4, we use the following commands to swap pins:

First, we copy the .tcl file in exercise_4.

* **cp /usr/share/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl .**

Then do the changes in the run_lvs.sh file.

![Docker command](/Day5_images/9.png)

Then do the changes in the sky130A_setup.tcl file.

![Docker command](/Day5_images/10.png)

Then do the changes in the netA.spice file.

![Docker command](/Day5_images/11.png)

Then we look the final output file.

![Docker command](/Day5_images/12.png)

For exercise 5, we use the following commands to create the netlists:

* For Xschem:

* **Click on Netlist button**

* In Xschem, go to simulation menu option and then click on "LVS netlist: Top level is a .subckt"

* ** Then again click on Netlist button** --> it is possible to observe that the first subcircuit is not commented.


* For Magic:

Open the "user_load_analog_project_wrapper.mag" file.

![Docker command](/Day5_images/13.png)

For extraction, do the following commands:

* **extract do local**
* **extract all**
* **ext2spice lvs**
* **ext2spice**

* For Netgen:

There is a script to run lvs comparison.

* **./run_lvs_wrapper.sh**

We observe in the output file that there is a problem instantiating the standard buf_8 cell, creating proxy pins.

![Docker command](/Day5_images/14.png)

We come back to Xschem and run the following command: **xschem analog_wrapper_tb.sch**.

Click on "Netlist".

We edit the lvs launch script to point to the tb file. 

We come back to Netgen and edit the "run_lvs_wrapper.sh" file.

We observe that there are unmatched pin errors. Follows the errors

![Docker command](/Day5_images/15.png)

![Docker command](/Day5_images/16.png)

![Docker command](/Day5_images/17.png)

![Docker command](/Day5_images/18.png)

We load the user project wrapper with the command **load user_analog_project_wrapper**.

We need to separate both nets with a metal resistor with the command **paint rmetal3**.

We reextract with the following commands:

* **extract do local**
* **extract all**
* **ext2spice lvs**
* **ext2spice**

And then we run **./run_lvs_wrapper.sh**

Since  there are a lot of errors, we need to go to Xschem to include the metal resistor. To open Xschem, we use the command **xschem analog_wrapper_tb.sch**.

![Docker command](/Day5_images/19.png)

We rerun the "Netlist" and then we go to Netgen to run the command **./run_lvs_wrapper.sh**. As it is possible to observe, now there are only a few errors regarding unmatched pins.

Then we go again to Xschem. It is posiible to observe that "io_oeb[26:0]", "io_clamp_high[2:0]" and "io_clamp_low[2:0]" are flotting pins.


We go in Magic to the specific pin io_oeb[11] with the command **goto {io_oeb[11]}**. Then we go run the command **getnode**.

![Docker command](/Day5_images/20.png)

We should include the metal 3 resistances in both schematic and layout to get LVS clean.

For exercise 6, we go to Magic. Then we load the digital PLL with **load digital_pll.mag**.

We do the extraction with the following commands:

* **extract do local**
* **extract all**
* **ext2spice lvs**
* **ext2spice**

![Docker command](/Day5_images/21.png)

We use the **feedback why** to identify the errors. We then go to the netgen directory and run lvs with the **./run_lvs**. There are a several cells missing: tapvpwrvgnd_1, fill_1 and fill_2.

![Docker command](/Day5_images/22.png)

In Magic, we use the command **select cell FILLER_0_11** to look for the instance that is generating one of the errors.

![Docker command](/Day5_images/23.png)

We observe with the command **-usr/share/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl** that the taps are ignored.

![Docker command](/Day5_images/24.png)

Thus we must edit the "run_lvs" file with the following line.

![Docker command](/Day5_images/25.png)

For exercise 7, we run Magic. We load the exercise with the command **load mgmt_protect.mag** and then we run the extraction commands.

![Docker command](/Day5_images/26.png)

Then we run the **./run_lvs.sh** command and see that there is something wrong. The error is "" Module 'mgmt_protect' is not structural verilog, making black-box."".

Let us edit the verilog to line 242, since the inverter is not explicitly implied.

![Docker command](/Day5_images/27.png)

We correct the error by going to the gl subdirectory where the real gate level file is located and then edit the shell script to point to the gate level file.

![Docker command](/Day5_images/28.png)

Then run again the **./run_lvs.sh** command. There are still some errors. Let see in Magic.

![Docker command](/Day5_images/29.png)

We need to include a metal resistor in the ground nets to avoid having a short of vss nets. In other words, some parts of the substrate are not connected to other parts of the substrate.

So, the actual solution is to merge all the vss nodes into a single ground/substrate node. We edit the /verilog/gl/mgmt_protect_hv.v file.

![Docker command](/Day5_images/30.png)

And then we do the merging in the top level cell /verilog/gl/mgmt_protect.v file by adding the same lines above and the two additional vssd lines.

![Docker command](/Day5_images/31.png)

We finally run again the lvs command.


For exercise 8, we go to Magic and then load the cell "digital_pll.mag"

![Docker command](/Day5_images/32.png)

And then run lvs. We observe that there are some mismatch errors.

![Docker command](/Day5_images/33.png) 

We include the **export MAGIC_EXT_USE_GDS=1** line in the run_lvs.sh file. 

![Docker command](/Day5_images/34.png)

And then we rerun lvs. By grepping diode in both verilog and mag files with the commands below we observe that the diode was placed in the layout file to correct some antenna errors.

![Docker command](/Day5_images/35.png)

![Docker command](/Day5_images/36.png)

We solve it by modifying the "../verilog/digital_pll.v" and including the instance.

![Docker command](/Day5_images/37.png)

There are still some decaps errors related to a broken path of power network.

We observe that the power network "VPWR" is isolated.

![Docker command](/Day5_images/38.png)

We include the via3 to connect the power network mentioned above to the global power network. 

![Docker command](/Day5_images/39.png)

We need to do the extraction again and then we observe that there is a net mismatch.

![Docker command](/Day5_images/40.png)

We connect the _328_ cell to the A node.

![Docker command](/Day5_images/41.png)

We observe that the final mismatch correspond to a short since two networks are shorted in a single one. We erase the metal 1 layer shortening the two nodes.

![Docker command](/Day5_images/42.png)

There is a final pin error.

![Docker command](/Day5_images/43.png)

For exercise 8, we run Magic and extract. Then we go to Xschem, extract with the Netlist button and then do lvs. We observe that there are some property errors.

![Docker command](/Day5_images/44.png)

The circuit mismatches are:

![Docker command](/Day5_images/45.png)

Then we modify the Xschem testbench with the correct resistor and capacitor property values. The we click on the "Netlist" button and do the lvs.

![Docker command](/Day5_images/46.png)

There is only one more error.

![Docker command](/Day5_images/47.png)

We go to Magic and identify the mismatched cell.

![Docker command](/Day5_images/48.png)

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
|   | : CRTL + Z --> To zoom in the specific box area ||
|   | : S + S + S --> Selects the node path in a layout ||
|   | : getnode --> Gives the name of the node ||
|   | : select feedback --> selects a path helps avoiding loosing track ||
|   | : fe cl --> to clear the feedback mentioned above ||
|   | : CTRL + Z and then type bg --> to set Magic in the background ||
|   | : fg --> to unset Magic in the background ||

