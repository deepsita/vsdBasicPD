# vsdBasicPD
#### Workshop on VLSI/SoC Physical Design with concepts mapped to open source tools by VSD.
## Framework of the workshop 
  ### Aim - Learn - Assess - Practice - Bridge the gap - Industry Ready

The workshop is hosted on virtual learning platform called Intelligent Assesment Technology which unlocks the potential of everyone. The course is designed to bring up a solution in bridging the gap of theoretical concepts and practical experience. 
## Bird's Eye view of skills acquired
The workshop starts with an interesting pictorial explanation of System on Chip and various components of the chip. The next target is to understand where the actual ISA, Hardware Design, Physical Design stands while designing a complete system. 
With this note, the focus will narrow down to the details in design of real time SoC Chips. The hands-on introduction to every stage in the flow of the chip design with state of art open-source tools. "NGSPICE" for circuit spice, pre-layout and post-layout simulations , "MAGIC" for layout draw and edit,"QFLOW" for the complete tool chain starting from synthesis were EDA tools utilised for hands-on.



<img width="600" alt="birdseye" src="https://user-images.githubusercontent.com/25682001/99968895-c01c0f80-2dbf-11eb-9682-3030585203f7.png">


## Study  & Review of components of RISC-V based picoSoC.
### Components of a Soc
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

##### Task-1 Area Estimation of the layout
The qflow tool is used display the design in the MAGIC layout viewer with the following commands.


    cd vsdflow
    ./vsdflow spi_slave_design_details.csv
    cd outdir_spi_slave
    qflow display spi_slave
<img width="568" alt="total_layout_area_output_si_slaveeg" src="https://user-images.githubusercontent.com/25682001/99975777-a7fcbe00-2dc8-11eb-93cb-9c4ce92579e2.png">

##### Task-2 Synthesis of reference design 
The reference design is synthesised using the qflow tool chain with the following command. The proper technology, verilog source file and top module name should be provided to the tool in the settings.

     cp ~\vsdflow/verilog/picorv32.v source/.
     qflow gui &


## Chip Planning Strategies

## Design and Charecterisation of Library cells

## Pre-Lyout Timing Analysis

## Final RTL2GDS




 
