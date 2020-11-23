# vsdBasicPD
#### Workshop on VLSI/SoC Physical Design with concepts mapped to open source tools by VSD.
## Framework of the workshop 
  ### Aim - Learn - Assess - Practice - Bridge the gap - Industry Ready

The workshop is hosted on virtual learning platform called Intelligent Assesment Technology which unlocks the potential of everyone. The course is designed to bring up a solution in bridging the gap of theoretical concepts and practical experience. 
## Bird's Eye view of skills acquired
The workshop starts with an interesting pictorial explanation of System on Chip and various components of the chip. The next target is to understand where the actual ISA, Hardware Design, Physical Design stands while designing a complete system. 
With this note, the focus will narrow down to the details in design of real time SoC Chips. The hands-on introduction to every stage in the flow of the chip design with state of art open-source tools. "NGSPICE" for circuit spice, pre-layout and post-layout simulations , "MAGIC" for layout draw and edit,"QFLOW" for the complete tool chain starting from synthesis were EDA tools utilised for hands-on.



<img width="500" alt="birdseye" src="https://user-images.githubusercontent.com/25682001/99968895-c01c0f80-2dbf-11eb-9682-3030585203f7.png">


## Study  & Review of components of RISC-V based picoSoC.
### Components of a SoC
* The chip has the core where all the logic, Macros, Foundary IP's lies and pads for signal I/O and the total die after the floorplanning, placement, routing etc are completed. 
* Interface between an application and actual hardware is brought in by the system software. 
* However, the hardware understands only the binaries, and hence an abstract interface which starts with Intruction Set Architecture, then the RTL Description.
* Once RTL description is done, then the process of conversion of RTL to physical chip is done. 
### Introduction to RISC-V picoSoc
RISC-V is an open-source standard instruction set architecture (ISA). Each SoC has a RISC-V processor, memory, a range of I/O, and interfaces for embedding user functions. PicoSoC example components are UART, SPI memory controller, Scratchpad SRAM memory, SPI flash memo.
### Design Flow 
* The RTL description of the Soc is done as the initial step. The reference design of Raven PicoSoc - picorv32 is considered for further steps.
* The RTL is converted to gate level netlist with Synthesis tool called Yosys embedded in the QFLOW tool chain.
* Then comes the floorplanning, placement, Routing, CTS.
* The most important step is Static Timing Analysis (STA) which should be performed at each and every juncture of the design step in order to meet the requirements of the design.
#### Open-Source tools for chip design
| Design Step                                | Tool      |
|--------------------------------------------|-----------|
| Synthesis                                  | Yosys     |
| Floorplanning                              | Graywolf  |
| Placement                                  | Graywolf  |
| Clock Tree Synthesis (CTS)                 | Graywolf  |
| Layout Edit, view                          | MAGIC     |
| Pre-layout, post-layout, spice simulations | NGSPICE   |
| Static Timing Analysis                     | OpenTimer |

### Hands-on 
Clone the vsdflow repo with the following command in the terminal. 
    


    git clone https://github.com/kunalg123/vsdflow.git

#### Task-1 Area Estimation of the layout
The qflow tool is used display the design in the MAGIC layout viewer with the following commands.


    cd vsdflow
    ./vsdflow spi_slave_design_details.csv
    cd outdir_spi_slave
    qflow display spi_slave

<img width="800" alt="qflow_manager" src="https://user-images.githubusercontent.com/25682001/99986120-d1234b80-2dd4-11eb-81f3-35c7ca1f2df7.png"> 
<img width="800" alt="total_layout_area_output_si_slaveeg" src="https://user-images.githubusercontent.com/25682001/99975777-a7fcbe00-2dc8-11eb-93cb-9c4ce92579e2.png">

#### Task-2 Synthesis of reference design 
The reference design is synthesised using the qflow tool chain with the following command. The proper technology, verilog source file and top module name should be provided to the tool in the settings.

     cp ~\vsdflow/verilog/picorv32.v source/.
     qflow gui &


<img width="800" alt="qflow_synth" src="https://user-images.githubusercontent.com/25682001/99977192-53f2d900-2dca-11eb-9868-72dd4bf17a4c.png">



## Chip Planning Strategies
The concepts of chip floor planning considerations, Binding the library to the design, cell design flow and timing charecterisation parameters were presented. 
### Height and width of core
* Core is the section of chip where the fundamental logic is placed
* Die consists of core and it is a ssemiconductor material on which the circuit is fabricated.
* Utilisation factor is the ratio of Area occupied by netlist and the Total Area of core. The 100% utilisation refers to Utilisation factor being 1. However, in the practical scenarios only 50-60% utilisation is considered in order to provide place for other routing and filler cells etc.
* Aspect Ratio is the ratio o the heught and width of the core. The Aspect ratio of 1 refers to a square chip. 
### Pre-placed Cells
* Pre-placed cells are those which are implemented once and instantiated many times. The arrangement of these IPs in the chip is referred to as Floorplanning.
* These IPs/blocks have user defined locations and hence are placed in chip before the automated placement and routing.

### Decoupling Capacitors
* Large complex circuits will have high amount of switching current.
* Noise mArgin specs define the logic'0' and logic '1' valid voltages and undefined regions.
* The high switching current demand can be solved by addition of decoupling capacitors in parallel with the circuit.

### Power Planning
* The power planning should be done in a way that the driver and load be close to each other in the 'L' sense. 
* The improper power planning i.e., single power and Ground lines can lead to 'Ground bounce' and 'Voltage Drrop'.
* So the plan should have multiple 'VDD', 'VSS' lines running through the circuit.

### Pin Placement

* The conectivity of the chip to the outside world.
* The placement of the pins is dependent on the position (near/far) but not on the ordering. 
* The clock ports are found to be bigger than the general data ports in order to necessitate for less resistance. 

### Placement and Routing

* Bind the netlist with the physical cells i.e., library cells
* Placement of the logic
* optimise the placement by inserting repeater

### Hands-on 
#### Task-1 Floor planning and Placement
Once the synthesis is done, then the placement settings are decided based on utilisation factor and aspect ratio. 
The command to invoke the picorv32 design in the QFLOW manager is


     cp ../verilog/picorv32.v source/.
     Qflow gui &
* Then the Placement settings are given as the INitial density of 0.7 and the aspect ratio as 1. 
* The Pins placemnet can be done using the QFLOW Pin Manager.
* Few pins are left with the same positions and few unassigned pins are taken as a seperate group and specify the placement of the pins.

<img width="800" alt="qflow_pin" src="https://user-images.githubusercontent.com/25682001/99987181-29a71880-2dd6-11eb-801a-3bfcad6d95a4.PNG">

Now the placemnet is Run.
<img width="800" alt="placement" src="https://user-images.githubusercontent.com/25682001/99987299-4d6a5e80-2dd6-11eb-9690-36a3c580489c.png">



#### Task-2 Area Estimation of the Design
The layout of the picorv32 design is displayed using the following command


    qflow display picorv32 &
    
The tkcon window and the MAgic window will open with the layout in it and the 'box' command gives the area estimation of the design.


<img width="800" alt="l21" src="https://user-images.githubusercontent.com/25682001/99987801-e26d5780-2dd6-11eb-9392-87f541b74e13.PNG">




## Design and Charecterisation of Library cells
### Intro to Library Standard Cells
* Standard Cells in the library can be ANDGate, Or gate, Buffer, DFF etc.
* Each cell have different functionality
* cells with same functionality can have varied sizes.
* cells with same functionality can have varied switching thresholds, rise and fall delys etc.
### Cell design flow
* Inputs
  * Process design Kits
  * DRC LVS rules of tech node
  * SPICE models
  * Library and User defined Specs like cell-height, supply voltage, metal layers, pin location, gate length
* Design Steps
  * Circuit design - Transitor based implementation of the required functionality
  * Layout Design - stick diagrams
  * Charecterisation
* Outputs
  * Circuit Description Language
  * GDSII
  * Extracted Spice netlist
  * Timing, noise etc.
### Flow of Charecterisation 
 * Model Libraries
 * Spice model
 * circuit model
 * Power Supply
 * Input Stimulus
 * Output Capacitance
 * Define the required Charecterisation (DC/ Transient).
### Timing Charecterisation
 * Timing Threshold Definition
    * slew Rise,fall
    * IN Rise ,fall
    * OUT Rise,fall 
 * Propagation Delay
 * Transition Time
 
### SPICE Simulations
* SPICE Deck
* NGSPICE commands for simulation of spice definitions of the circuits
* Evaluation of Static and Dynamic behavior of CMOS INverter
### Art of Layout - Euler's path and stick diagrams
* Importance of ordering of inputs i.e., poly bars in the layout.
* Increase of complexity and wiring if not properly ordered
* Eulers path defines the best input ordering to meet the needs of layout. 
  * Network graphs are constructed
  * Number the nodes
  * Transistors as edges between the nodes
* The Layout should be drawn in lines with the Design rules specified for tech node under consideration.
### CMOS Fabrication Process
A 16-mask process is explained with each step in detail 
* Selection of Substrate
* Active region for Transistors - Mask1
* N-well, P-well formation  - Mask2 and Mask-3
* Formation of Gate -Mask 4, Mask-5, Mask-6
* Lightly doped drain formation - Mask-7, Mask-8
* Source and Drain Formation - Mask-9, Mask-10
* Contacts and Interconnects - Mask-11
* High level metal formation -Mask-12 to Mask-16

### Hands-on 
Clone the ngspice repo

     git clone https://github.com/kunalg123/ngspice_labs.git
     
 ##### Task-1 Spice Simulation of Inverter - DC and Transient
     cd ngspice_labs
     ngspice inv.spice
The snippet of spice of inverter is as below


Commands to run Dc Analysis are as below


      ngspice 1 -> run
      ngspice 1 -> setplot dc1
      ngspice 1 -> plot out in
      
 ![dc_res](https://user-images.githubusercontent.com/25682001/99997953-e6ec3d00-2de3-11eb-94c4-b592bd4d128c.png)


Commands to run Transient Analysis are as below


    ngspice inv_tran.spice
    ngspice 1 -> run
    ngspice 1 -> setplot tran1
    ngspice 1 -> plot out vs time in
    
* The calculation of rise delay and fall delay is computed at 50% of input. The difference in the time point of input and output at 50% voltage is the delay.
* This rise fall delay varies along with varying i the PMOS sizes
* Various experiments with PMOS size being same as nmos, twice as nmos, thrice, four times as nmos are explored. 
* It is observed that the rise delay decreases wit increase in the PMOS width
* This is due the inverse relation between R and (W/L).
The simulation result with NMOS and PMOS equal sizes is shown below
![inv_wp=wn](https://user-images.githubusercontent.com/25682001/99994377-edc48100-2dde-11eb-9b9e-d5f5b2441b5b.png)


The simulation result with PMOS being three times of NMOS is shown below
![inv_wp=3wn](https://user-images.githubusercontent.com/25682001/99994359-e3a28280-2dde-11eb-9b2f-4cb5e0bb8949.png)


The simulation result with PMOS being 3.75 times of NMOS is shown below

![inv_wp=3 75wn](https://user-images.githubusercontent.com/25682001/99993963-6545e080-2dde-11eb-9a86-61341b3fa136.png)


##### Task-2 Layout Simulation
Complete the layout of a speciic function given.
Verify the pre-layout and post-layout simulations'

The initial layout is as shown below
![drcclean](https://user-images.githubusercontent.com/25682001/99995288-287ae900-2de0-11eb-86e4-0914d1462b63.png)

The completed layout with all the metal connections is shown below
![postlayoutmagic](https://user-images.githubusercontent.com/25682001/99995202-0d0fde00-2de0-11eb-8297-a75af273a448.PNG)


The Pre-layout simulation of the given function is shown below
![prelayoutsim](https://user-images.githubusercontent.com/25682001/99995378-49433e80-2de0-11eb-8d71-4ea91beff3c4.png)



The Post-layout simulation of the given function is shown below
![postlayoutsim](https://user-images.githubusercontent.com/25682001/99995399-53fdd380-2de0-11eb-840a-ca49882e43c8.png)


## Pre-Lyout Timing Analysis
* Delay Tables
* Clock Gating
* Timing Analysis - Modelling is done with the help of delay tables, input slews and output lods. 
   * Each node should drive same load at every level
   * Buffers in one lvel should be identical
* Timing analysis 
  * The clock period should definitely be greater than the combinational delay between launch and capture flops.
  * Slack is the difference between required time and the arrival time
  * clock jitter and Setup/hold uncertainities.
* Clock tree Synthesis - 
   * The aim being clock skew as much low as possible, the best algorithms for clock paths are required. 
   * H-tree Technique.
   * Net Shielding
   * Proper repeaters/buffers
### Hands-on 
The  prelayout and post layout STA results are analysed.

Technology File



     /usr/local/share/qflow/tech/osu018/osu018_stdcells.lib
     cd vsdflow/my_picorv32
     
  Create Constraints
  
  
  
     leafpad picorv32.sdc
     create_clock -name clk -period 2.5 -waveform {0 1.25} [get_ports clk]
     
 Create Configuration file
 
 
 
     leafpad prelayout_sta.conf
     read_liberty /usr/local/share/qflow/tech/osu018/osu018_stdcells.lib
     read_verilog synthesis/picorv32.rtlnopwr.v
     link_design picorv32
     read_sdc picorv32.sdc
     report_checks
     
Run STA



     sta prelayout_sta.conf

The prelayout STA result is as shown below

![qflowsta](https://user-images.githubusercontent.com/25682001/99998293-7691eb80-2de4-11eb-81ec-92ceada030d4.png)




## Final RTL2GDS

* Mze Routing -Lee's Algorithm
* DRC Clean
 * Few rules for wires/metal layers
* Parasitic Extraction
 * Format for represntation is - SPEF( Standard Parasitic Exchange format)
    * Load Capacitance
    * Driver
    * Waveform Shape
    * Lumped Capacitances
    * Reciever
    * Distributed Resistances and Capacitances
    
### Hands-on 
The Routing, STA and backannotation are performed and the frequencies are compared for pre-layout and post-layout STA.
 
 
 
     cd vsdflow/my_picorv32
     qflow route picorv32
     qflow sta picorv32
     qflow backanno picorv32
     

The route of the design is shown below.

![route](https://user-images.githubusercontent.com/25682001/99999259-ec4a8700-2de5-11eb-97a7-b4c772043156.png)

The prelayout Frequency is shown below

![prelayout_freq](https://user-images.githubusercontent.com/25682001/99999342-03897480-2de6-11eb-96df-0f410d75b7d6.png)


The post layout Frequency is as shown below

![postlayout_freq](https://user-images.githubusercontent.com/25682001/99999306-f8cedf80-2de5-11eb-8241-3b55565789cb.png)


The complete steps from Synthesis to Routing and STA are reviewed. 


## Acknowledgement
Kunal Ghosh, C0-founder (VSD corp.Pvt.Ltd)

 
