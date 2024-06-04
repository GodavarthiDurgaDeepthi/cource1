### NASSCOM-VSD SoC Design Program

## Day1 : Inception of open-source EDA,openLANE and Sky130 PDK 

Topics:

• SKY130_D1_SK1 - How to talk to computers

• SKY_L1 - Introduction to QFN-48 Package, chip, pads, core, die and Ips

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/a5006670-5c4a-40a3-a3bd-af3a2acefd6d)

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/f29aaa2e-4813-4080-9ef4-4832b7b2990a)


Few terms related to the Floor Plan

• Pads : It is used to send signal from inside and outside of the chip

• Core : It is the place where logical gates and connections are present.

• Die : It is the place where chip is present.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/939087ab-e7ff-45fd-a56d-093367d20e86)


In this image, comes a lot of queries like “why PLL, ADC, SRAM are really called IP’s and not 
macros? And vice-versa”. Now that is a problem statement, which just cannot be solved with 
words or text. There are 2 ways to solve this query

• Designing IP’s and using same IP for PNR

• Let students design this IP

# RTL to GDS FLOW Architecture:

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0a28377a-eb3c-458f-baf7-7d3da298c148)

Here is an overview of the steps involved in the RTL to GDS flow:

Synthesis : It is the process of converting RTL into technology dependent gate level netlist.

Floorplanning and Power planning : In Floorplanning we decide the placement of 
macros,IO’s,core width and height. In powerplanning we decide power ground structure and 
ensures that uniform power is distributed all over the chip.

Placement : In this step, placement of standard cell is done and ensures that if it is properly 
placed in standard cell rows.

CTS : In clock tree synthesis, clock tree structure is defined which has minimal skew.
Routing : In this step, physical connection of metal layers are done for all logical connection 
present in the design.

Signoff : In this step, we decide if design follows DRC rules and meets timing requirement.

From now we start the lab1:

After Installation completed we start the lab

 #                                  Day 1 Lab

# Tool: openLane

openLANE is basically of flow. the main aim of openLANE is to have complete RTL to GDS 
flow. so you put RTL and what u have at the end is GDSII file. This tool is very similar to 
commercial EDA tool. Whenever you want to know more about commands in linux use 

commandd : eg : ls –help

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0ae1305f-f06e-41e6-8452-2c1fc18ef218)

Here in pdks we are using Sky130A

And I am checking remaning folders also in that skywater-pdk folder contains information 
related to timing,cell lef,technology lef. skywater-pdk files are silicon foundry related files. 
This files are made to work with commercial EDA tools and not for opensource EDA tools.so 
open_pdks folder added here to mitigate this problem. basically they are sets of scripts and 
files that converts this foundry level PDK’s to be compatible with open source EDA tools like 
magic(for layout),netgen. Sky130A folder is that folder which has been made comfortable to 
work with opensource environment.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/bf592408-4896-400d-8a7d-5c3784536b82)

➢ libs.ref folder contains file related to cell lef,timing that is specific to technology.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0c79d478-4313-4b9e-84b1-81e69e91741d)


➢ libs.tech folder contains file related to tool that is specific to tool.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/4a13b58a-fde3-48fb-b1de-ba85f705d9fa)


……….After checking every file I am started executing the flow of openLane using “docker”

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/5d5211bb-8688-43df-baea-f0f4ca7d499f)

Next im going to check the package

• Package Extraction Command used : package require openlane 0.9

• Date Preparation Command used : prep -design picorv32a data preparation merge 
both cell lef & tech lef into one file.so after data preparation you can see that there is 
a formation of runs directory.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/a95aba39-0519-4528-ba36-a6eaf3761703)

Synthesis:

Used command: run_synthesis

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/c75ba48a-40d6-4717-be80-1740ad0df773)

Steps to characterize Synthesis Result:

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/1180f698-dca0-4856-b315-bb744c80a3bc)

Calculation of Flop Ratio and DFF % from synthesis statistics report file.

Flop ratio = No. of Flipflop/No. of Cells 

Here No of flop=1613

No of cells=14876

Flop ratio =1613/14876 =0.1084

DFF % = 0.1084 * 100 = 10.84%

## Day2: Good floorplan vs bad floorplan and introduction to library cells

Define height and width of core and die

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/65641a9c-08f3-4232-80bc-6477eaa14bf4)

Example : Lets consider an example of Netlist shown below

Netlist: It is basically defines connectivity between all the Elements.Dimension of chip is 
mostly depends on dimension of the logic gates.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/1e9dd510-f784-4f0f-84cd-d7e014626276)

• Lets consider the dimension standard cell as 1unit X 1unit.so the area of each standard 
cell will become 1sq.unit.And also assumes same area for Flipflop as shown below

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/56c91d85-c4b2-41ab-9f70-210d3b11f25d)

 so the minimum area Occupied by the netlist is: 2unit X 2unit.

Utilization Factor :

• It is the area occupied by netlist to the total area of the core. 

• When Utilization factor is 1 that is 100% which means core is completely occupied by 
the logic

Utilization Factor=Area Occupied by the netlist/ Total area of the core

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/9f931a3c-9b3b-4227-bdf4-fa8f63057bf8)


Aspect Ratio:

• Height of core to the width of core. 

• When aspect ratio is 1 it indicates that the chip is square shape. when aspect ratio is 
other than 1,itindicates that the chip is rectangular shape.

 Aspect Ratio = Height / Width

 ![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/fa4ee226-fccd-48fe-98e5-63c544cd88fb)

Surround pre-placed cells with Decoupling Capacitors :

Decap cells are placed across the Memories or blocks that will supply the required amount 
of charge to this block whenever they are switching that is current demand of each block or 
macros is handled by decap cells. Basically, it ensures that there should not be any crosstalk 
issue.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/4b5b6968-7fc9-4a0d-b1d8-1151222392bc)

• Power Planning :

Instead of single power supply, multiple power supply can also be built throughout the chip 
to ensure that uniform power should be deliver throughout the chip. So any logic present in 
the design will take power from its nearer power supply or it will drop a current to its nearer 
ground. This is basically called as Power mesh.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/98ee45c3-8431-468c-b241-414a3ca7b950)

• Pin Placement :

Clock ports is bigger in size than data ports since clock ports contineously sends signals to 
flipflop so for that it needs least resistance path.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/4e3c311c-4dc1-40a2-be9e-be3894ae6fdf)

• Logical Cell Placement Blockage : 

This Blockage ensures that there should not be placemnet of cells in between core and die.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/272713e1-f29a-4380-889d-f2e1f7d0ea32)

 #                                             Day 2 Lab

Floorplan:

By using ‘run_floorplan’ command I am running the floorplan

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/db61d40c-d1ce-4870-8bd6-19c94126d124)

(path:Desktop/work/tools/openlane_working_dir/openlane/configuration/floorplan.tcl)

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/203083b2-d0d1-4926-9160-8746cc00c554)

• FP IO mode = 1 means pin possition is random but at equidistance.

• FP IO mode = 0 means pin possition will not be equidistance.Here default core 
utilization is 50%,FP IO VMETAL as 2 and FP IO HMETAL as 3.

• In openlane flow vertial metal layer and Horizontal metal is one more than what is 
specified.

• So VMETAL will be 3 and HMETAL will be 4. sky130A_sky130_fd_sc_hd_config.tcl this 
file will overwrite the default setting present in openlane.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/1f150a92-40f8-47fa-9b38-24b6ee00c60b)

Hence here utilization = 35%

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/87a69d72-c562-44ab-b2fe-a58696218002)


Layout of Floorplan in Magic

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/d8eb810c-6efa-4f80-8cbc-4a80549793cb)

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0089ebb6-bf9b-4d9c-9396-5823bc10edd4)

Key points:

• To zoom out layout using ctrl+z

• To highlight object press s

• press v to fit it on screen.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/7911297d-5712-43f1-bc56-e2b8f721fea4)

• horizontal Ports layer set in config.tcl :- metal 3

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0fd1ec65-9835-431d-9062-8252f5d9925b)

• Vertical ports layer set in config.tcl : metal 4

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/15f0766d-7ca6-43af-8bec-ac27f515f8c0)

• Placement of Decap cells and Tap cells

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/96450a1f-0e62-4b1a-8e64-202f9b18281a)

• Diagonally equidistance tap cells

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/98f89639-68d1-49cc-bbb0-b65707c32791)

• Unplaced standard cells at origin

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/67224009-ac00-44fa-8845-0652c6ea1319)

➢ Placement

 Placement stages :

• Global Placement

• Detail Placement

Global placement aim is to reduce wirelength. In openlane there is a concept of 
HPWL.HPWL stands for half parameter wire length.

 Screenshots of Placement run

 ![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/2e566b5f-bafa-4656-a5af-2a8dae9026aa)

• Number of instances ,fixed instances, utilization

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/f38e829d-7da8-4363-becf-927fabdb816a)

• placement def in Magic

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/f34cda5e-8c5b-4d51-aed5-414674f0f1d7)

• Standard cells placed legally in standard cells rows

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/5c8c441c-d233-4a43-94fd-ccdb8bc71d4e)

In openlane power planning is performed after CTS stage

## Day 3 : Design library cell using Magic layout and 

ngspice characterization

 #                             DAY 3 LAB

Performing SPICE extraction and post layout spice simulation of Inverter we have done 
Clonning of vsdstdcelldesign directory from github link in openlane directory.

• Now you can see the directory vsdstdcelldesign inside openlane as shown below

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/32db1afe-bfff-4545-9fac-8f04bfc03a92)

• Copy magic tech file i.e.sky130A.tech file inside vsdstdcelldesign directory

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/329de029-6961-4a46-a66f-7bb49e700c88)


• Now open sky130_inv.mag file in magic tool

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/3c43d62c-03e3-4f0d-9dca-da12d12abe1f)


• Inverter layout in magic:

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/70ad7811-b55b-49c7-809c-320a56e1c68c)


• nmos : when polysilicon crosses n diffusion then it is a nmos.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/6aa05d0e-941a-4204-beb0-38367db17d9d)


• pmos : when polysilicon crosses p diffusion then it is a pmos.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/6df0538f-a49a-43e9-ac4c-0992ddad5538)


• To check connectivity,press s 3 times Example 1 : check connectivity of pmos drain 
and nmos drain, selected area shows connectivity as shown below

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/b35da4f9-2dbd-4e7f-8f38-9d88e3508ed5)


Example 2 : Connectivity of pmos and vdd (press s 2 times)

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/ac6ef28d-832d-4b53-bc61-a9857dade0b0)

Example 3 : nmos and vss connectivity(press s 2 times)

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0146d8ea-598c-4bbd-81dc-d140cdfb1f2d)

➢ Lab Steps to create standard cell layout and extract spice netlist

Extract the spice netlist from the inverter using Magic tool.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/30c26c51-512b-42c5-b330-9397c6c60b42)

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0ccbabb2-acca-4b36-8471-ffed17ed8cea)


After that use command ext2spice,this will create a spice file which is basically a extracting 
parasitic capacitance in the vsdstdcelldesign directory.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/692ade98-9a82-49de-a976-05d3992dce38)

➢ Lab steps to create final SPICE deck using Sky130tech

After Extracting SPICE file we need to update it according to the design.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/43339d3f-b5b1-4505-b67c-14ba51f0a230)

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/f01cead9-a05b-442c-87d9-14eb2bb3fe40)


• Now the next step is to run the SPICE file in ngspice tool by using command ngspice 
sky130_inv.spice

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/2cce9d8e-5721-4625-8064-9f5cc46092a8)

• Now we need to check output vs time i.e transient response for that use command 
plot y vs time a

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/85e17c3b-24f7-4d5d-9bb6-a4c8af740e6c)

➢ Lab steps to characterize inverter using sky130 model files

Characterizing a cell means to find out 4 parameters such as:

• rise transition : Time taken for output waveform to transit from a value of 20% of a 
maximum value to 80% of maximum value.

• Fall time : Time take to fall output from 80% to 20%

• Propagation time

• Cell fall delay

Rise time : Plot of output vs input time a here maximum value is 3.3v so 20% of 3.3 is 0.66

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/e6b8f32d-3362-43ab-95ab-1d4eee3cbcdc

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/a92c6aa1-0f32-4735-9c15-91f8b141c5ab)


• So, Rise time = 2.244-2.181=0.063ns 

• Fall time = 4.095-4.051=0.044ns 

• Cell rise delay : It is the time taken for the 50% of transition from 0 to 1 at the output 
for the 50% transistion from 1 to 0 at the input side.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/5c619eb5-0a2f-4489-b0bd-a372986644e9)

• Cell rise delay = 2.21-2.15= 0.06ns 

• Cell Fall delay : It is the time taken for the 50% of transition from 1 to 0 at the output 
for the 50% transistion from 0 to 1 at the input side.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/8103ca1d-d1ee-4578-81dd-cc01034f231f)

• Cell fall delay = 4.076-4.049=0.027ns 

This is how characterization of cell is done at room temperature 27 degree celcius, normal 
temperature = 27 degree celcius, voltage = 3.3v

Now next objective is to use layout of this inverter and create a lef file and that lef file we 
will be using in openlane and will make a custom cell & also gives a custom name to that 
cell and will use this cell into picorv32a core and checks whether if openlane takes this cell 
or not and we will see what are the challenges we have to face.

Lab Intoduction to Sky130 pdk’s and steps to download labs

Download the lab files in the home directory using command : tar xfz drc_test.tgz

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/e5001bef-90b6-4e1a-a8f1-a1a5325fe48e)

 Wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz Now to extract the 
labs from downloaded zip file use command : tar xfz drc_test.tgz In the downloaded files,
there is a file called .magicrc that file serves as the start-up script for MAGIC.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/c01b2c68-c505-42af-8b7d-a641d333a31d)

• .magicrc file


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/84b384ad-c925-4171-b8e3-e27fd516ca81)

➢ Lab Introduction to Magic and steps to load Sky130 tech-rules

• To open the magic tool use command : magic -d XR


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/fc73c83d-8950-400b-9f36-9060f8b993d5)

• set of rules for metal 3: For that load file metal3.mag


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/ed90be1f-94c2-474f-a3ec-54aed4586cdb)


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/1c6d91b4-351b-44b9-a7ed-9a9432bcd65a)


• Here m3.1, m3.2, m3.5, m3.6 are the independent example layout geometries and 
most of them shows some kind of drc’s error. M3.6 shows minimum area drc,m3.5 
shows via overlapping, m3.2 shows metal spacing,m3.1 shows metal width drc as 
shown below


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/9f814d2b-29d9-41f2-88a0-399103ae1264)

• Now select an area in gui and fill the selected area with metal 3 by clicking on metal3 
option. Then type a command cif see VIA2 so that area which was initially filled with 
metal 3 get filled with VIA2 mask as shown below.


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/2631dd1c-fab0-4056-9441-4eb40db3a1f8)

➢ Lab exercise to fix poly.p error in Sky130 tech file

• Load poly file using command:- load poly Use what command to check name of 
selected mask layer

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/30d5d7c8-07b0-40e1-9908-748342a3b7fa)


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/d71b62e8-0e72-4031-ba1d-9a897e9f6b94)


• use box command to get width and height of selected box


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/cd9c2793-2a7d-4cf9-8615-2e0b0d901e6b)

• check the spacing between poly resistor and poly and compare this value with actual 
value in Skywater website. It can be clearly observe that there is a spacing error.Lets 
fix this spacing error. Now make changes in Sky130a.tech file as shown below

Before

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0f2991d7-6a1e-4ea1-8d6f-9fae68366417)

After

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/f3c98c68-1d8f-40d3-b712-7d30e4d9096e)

• Now after updating tech file load it again and check for errors using command drc 
check.

• Screenshots of fixed spacing drc issue between polyresistor and poly.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/079c0ee2-8e0f-473a-a0e9-622c216b9cbf)

## Day 4 : Pre-layout timing analysis and importance of good clock tree 

#                                    DAY 4 LAB

• Lab steps to convert grid info to tract info
we previously opened mag file using command – magic -T Sky130A.tech Sky130_inv.mag 
Mag file contains info about power ground,port,logic path information but for PNR the 
only information you need is information about core boundry,power gnd rails,input and 
output port.so LEF file have all this information. So next objective is to extract a LEF file 
from mag file and then check whether we can plug that extracted file into picorv32. To 
make standard cell we need follow some guidelines such as :

• The input and output port must lie on the intersection of vertical and horizontal 
tracks.

• The width of standard cell should be in odd multiple of track pitch and height should 
be in odd multiple of track vertical pitch.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/998e05c9-fbf6-4cec-a057-1030ee66351e)

• open track file

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/cbd33a80-8d84-4137-9029-87f45ebdf8ef)


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/7154878e-9c91-4e0d-9834-520f567aa98b)


Here li1 X Y 0.23 0.46 :---- 0.23 is horizontal track pitch offset,0.46 is horizontal pitch li1 X Y 
0.17 0.34 :----- 0.17 is vertical tack pich offset, 0.34 is vertical pitch X & Y are the metal layer 
direction. converge grid with tack value to check whether input port X and output port Y 
are on the intersection of vertical and horizontal of li1 layer. Since input and output port 
are defined on li1 layer.

grid before : press g

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/8e6f207c-b9d3-4bc6-85f5-55585d8969fe)

grid after :

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/c3e21865-d2ed-412d-8803-fb207fa1b9f4)

So here we converged grid defination into a track defination and routing of li1 layer will 
happen on this layer. Intersection of horizontal and vertical track are on the input and 
output port as show


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0c9b4567-93b5-4ae0-a9f9-f5dd05847255)

This ensures that route can reach on this port from x and y direction.

➢ Lab steps to convert magic layout to std cell LEF

• Save above mag file as sky130_vsdinv.mag Extracting lef file :

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/fbffb626-2735-457e-adef-c46b5e5ab20f)


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/a6ae0539-6718-4328-9b00-425809cc86e4)


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0becb226-b687-40fe-8fae-70b09fa75dd5)



Now we will plug this lef file into picorc32a,copy LEF file into src directory which is inside 
picorv32a.


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/99be4969-b631-4dd4-95d4-95fb6005084d)

• To map vsd cell during synthesis,library file is required.so copy libs which is in libs 
which is inside vsdcelldesign directory.


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/ba44c8ff-095b-4e7a-9bfb-5397406fbd8f)

• Modified config.tcl file which is inside picorv32a directory.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/d778c0f9-123f-48db-b975-cbf3f660a070)

• LIB_MIN,LIB_MAX,LIB_TYPICAL is used for STA analysis. LIB_SYNTH is a abc mapping 
file.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/90e026cf-eafc-4040-9fa3-4e8bb7f63ff6)

• invoke a tool

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/2548939b-b2c6-4969-a4cc-89dd374c8297)

• run_synthesis


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/f66ba38d-b9d0-49fa-8d65-97575390c857)

• Wns is maximum slack Chip area:

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/fd0b6379-6eb8-486b-b8d4-48c101135a60)

• Minimum slack or hold slack :

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/3864c7cb-1c0f-4272-8d1f-059d6b7ab039)

• Fixing setup slack i.e max slack We will balance delay and area by using a 
strategy.first we will check what is the strategy that have set in design.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/c7f830cc-9219-4339-98cd-8d8f026ecd5b)


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/879d3f7c-9db9-4500-a8f8-cd1090dbd3af)


➢ We will set SYNTH_BUFFERING and SYNTH_SIZING to 1

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/63c21376-a66e-4209-9bb1-711d175f26f5)

• Buffering :- inserting a buffer to reduce wire delay.example : reset pin goes 
everywhere in chip so we want those high fanout nets to be buffered in order to 
reduce delay.

• Driving cell :-The cell that drive input ports.If input port has lots of fanout then high 
drive strength cell is needed to drive input port. Now once again we will run synthesis 
to check whether setup slack is reduced or not
run_synthesis

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/5f6c4738-7104-4fdd-8a9d-7828776e0f9f)


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/fef65759-aa74-44e7-94c0-dc4bf38b636d)


• WNS & TNS =0 Chip area:


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/5cccd0fd-aa58-4f99-9bd0-151e146df683)

• Before :
WNS = -22.17 & TNS =-639.39
chip area =147712.918400 
• After:
WNS & TNS =0ns 
chip area= 181730.54400 can observe the custom vsd Inverter cell


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/ba3140c8-f7f0-48a5-9631-92fb53109b3a)

• Can also check whether custom vsd invgerter cell is present in merged.lef file


![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/d8dd7642-5aad-4387-a67c-a91852c0a83a)

• run_floorplan Issues I faced due to failed flow that I had to manually run the flow.

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/0434575c-4380-4789-be2d-814c6824c80e)

• init_floorplan

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/422b8534-4e7d-4c25-9235-66ccc7507e3e)

• Place_io

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/4596c462-79d5-41f1-85ec-56938542005f)

• Tap_decap_or

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/43846b48-bd09-4686-9306-fc3e57f47be1)

• run_placement

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/e4854ee8-c2fa-43ac-bf5e-f0e7933d0e4b)
![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/ac9a7c22-6809-44c1-9a39-04ff097c2039)


• Placement Layout

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/8c59a0eb-348a-4d1d-94bc-651fb27cdcec)

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/c13e8491-08af-46ee-975d-57a4d4dc449d)



• Can see the placement of custom vsd inverter cell

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/c69aae88-de34-41a2-869d-4a6715f24d9d)

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/afceb5ca-bcf6-4df2-97d5-83a711ff49c8)

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/9cfea6af-55e5-47e4-a001-b3b286ab293d)

Can observe that metal1 layer which is taken from library is correctly align with metal 1 
layer of custom inverter cell.

• Lab steps to configure openSTA for post-synthesis timing analysis

• After Placement stage,create file pre_sta.confi file inside openlane directory

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/f64e7ce9-7400-4afb-9311-248c5292ac3c)

• create file my_base.sdc file inside src directory of picorv32a

![image](https://github.com/GodavarthiDurgaDeepthi/cource1/assets/141111986/3eeb345e-0077-4493-94aa-58992ffd7212)

• to run pre_sta.conf in sta use command : sta pre_sta.conf


• Setup slack : -5.59 (violated) Fixing setup slack


• After Placement stage during pre sta analysis ,you can observe that input transition is 
high due to high output capacitance so we can optimize fanout value in openlane 
flow.


• Yon can see driver pin of net 13292 and it is driving input pins of 2 loads



➢ Clock Tree Synthesis

• Default parameters that are set in CTS are :

• Here target skew is basically a global skew that is set to 200ps.ROOT_BUFFER need 
high driving strength.

• CLOCK_TREE_SYNTH enables CTS and tirtonCTS is the tool which performs CTS. Here 
we are doing analysis for typical corner but not for multi corner. Since one of the 
drawback of tritonCTS is it cannot do optimization for multiple corner. 

CTS_TOLERANCE defines the tradoff between QoR and Runtime. QoR stands for 
Quality of results. Different QoR of CTS are:

• Better skew value

• 50 % duty cycle Higher value of QoR means design is meeting the required 
specification but it will require longer runtime. So higher value of CTS_TOLERANCE 
means lower runtime but worst QoR.

➢ run_cts

• In CTS stage buffers are added. So inside synthesis result directory you can observe 
the modified netlist file that contents previous netlist data and buffers that are 
added during CTS stage as shown below.

➢ Lab steps to analyze timing with real clocks using openSTA
To perform timing analysis, instead of invoking separate openSTA tool we will go inside 
openroad since STA is integrated inside openroad.The benefit of doing this is we are 
actually inside of openlane and we can use environmental variables.

In openroad, timing analysis is performed by first creating db file which is created using lef 
and def(post CTS def).

• Setup slack : Create db file

• Setup slack

• Hold slack

Now we have to check whether this analysis is correct or not? since we have built CTS for 
typical corner and we have given max and min library during timing analysis in openroad i.e 
analyzing for slow and fast corner.so this analysis is incorrect.so we will provide typical 
library in openroad to perform typical corner analysis.

• Setup slack :

• Hold slack:

So from above results of min_max corner and typical corner analysis we can observe CTS 
will not have setup and hold violation for typical corner.
Now we will replace synthesis netlist(post cts) with a new for that we will modify buffer list 
means will remove clk_buf1 from the list and again run CTS and check whether it meets 
skew value or not.

Since we have use post CTS def file.this is the reason that we have got error.so we need to 
set current placement def as shown below.

• Now again we have to create db

• Setup slack :

• Hold slack :

So from above results we can observe that setup slack get improved but area increased 
when we use clkbuf_2 instead of clkbuf_1 and can also see the clkbuf_2 in timing report as 
shown below

• Clock skew : It is the time difference between arrival time of clock at two different 
flop. It should be maximum 10% of clock period(12ns).

• Inserting clkbuf_1 inside buffer list

## Day 5 : Final steps for RTL2GDS using tritonRoute and openSTA

 #                         DAY 5 Lab

➢Lab steps to build power distribution network(PDN)
In openlane PDN is built after CTS stage. gen_pdn is the proc that is used to run PDN. Issues 
that I faced while running PDN are: There are some variables that were not defined in 
config.tcl such as LIB_SYNTH_COMPLETE Since this variable is called by the proc gen_pdn 
which is defined in or_pdn.tcl file And also LEF_MERGED_UNPADDED variable was not set 
in config.tcl

• Power Planning :
Standard cell are in metal1,straps are in metal4 and metal5. After PDN, current def file i.e 
picorv32a.cts.def is now changed to pdn.def which has the information about CTS def and 
PDN.


• Routing :

Default switches set for routing are :
Routing strategy : There are 5 routing strategy -- 0,1,2,3,14 Routing strategy 0 means 
routing will not be converged to zero DRC,but have less runtime and memory 
requirement.Routing strategy 14 required more runtime and memory requirement.In this 
design we are using routing strategy 0.

➢ Entire Routing process is divided into two types :

• Global Route or Fast Route In global route routing is divided into rectangular grid 
cells which can be represented as 3D routing graph and it is used durring global 
route.fast route is basically a engine which performs global route. global route first 
creates a routing guide i.e boxes(pins of cell) as shown below.so global route output 
is set of routing guide for each of the net. Detail Route Detail route is performed by 
engine tritonRoute.In detail route we use output from global route i.e routing guide 
and follow some algorithm to find the best possible connectivity among all this 
points.

In routing alteranate orientation of metal layer is preferred because,If metal1 has non 
preferred routing direction,it will be parallel to metal2 which can create parallel 
capacitance so it can degrade the signal. Hence alternate routing direction is preferred. Two 
routing guides are connected if:

• They are on the same metal alyer with touching edges.

• They are on neighboring metal layers with a nonzero vertically overlapped area.

➢ Detail Routes Inputs and outputs :

• Input : LEF,DEF,preprocessed guides

• Outputs:detailed routing soultion wth optimized wire-length via count.

• Constraints : Route guide honoring,connectivity constrains and design rules.

• Run_routing

• Routing guides : It is the output of fast route. Detail routing ensures that routing 
happen within this routing guides.

Can see that for each net there can be many routing guides.






