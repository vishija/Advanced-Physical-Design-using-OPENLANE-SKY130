# Advanced-Physical-Design-using-OPENLANE-SKY130
![Title](https://user-images.githubusercontent.com/22131133/123993965-8b71d580-d9ea-11eb-846b-4ccb3b5dcd8c.PNG)

# Day 2
# Floor Palnning
1. Define width and height of core and die
- Core - All logic is built inside a core. 
- Die - Core is placed on a die. Multiple dies are fabricated on a silicon wafer
- Utilization factor = Area occupied by netlist/Total area of core
- Aspect ratio = Height of core/Length of core
- If aspect ratio is 1 then its a square core
- If utilization factor is 1 then full core is occupied by netlist. If utilization factor is less than 1 then we can place additional component inside a core, used for        optimization.Generally utilization factor should be less than 1

2. Locations of preplaced cells
- Preplaced cells - Suppose a complex combinational circuit, then we can granularize the circuit i.e. cut the circuit into two pieces. These 2 cuts implemented seperately and connected according to circuit. Blocks implemented once and can be resused inside a core multiple times.
- Also some IPs are available such as memory,clock gating circuit, multiplexer etc which are implemented once and reused.
- The arrangement of IPs inside a core is called as floor planning.
- These IPs have used defined locations hence placed before placement and routing. Therefore these cell are called as preplaced cells.
- Automated tools of placement and routing tools places other cells than IPs. Preplaced cells are not moved by tools of placement and routing. The locations of preplaced cells are fixed.
- Usually IPs are related to inputs hence IPs are placed near input pins

3. Decoupling Capacitors
- Need of decoupling capacitors 
  - Suppose there are 3-4 IPs or blocks which are placed near the input pins.When any node inside block is charging, it needs current to be delivered so the capacitor at node can be charged and if node is discharging then needs to sink the current to ground.Each IP or block require power supply to work.The power supply connected to blocks by physical wires which has parasitic resitors,inductors and capacitors, hence the amount of actual supply provided to IP or block is less than actual.If the Vdd' that is available supply to a block lies in indeterminant region or undefined region of noise margin then it may create problem. IP or block cant work properly.

![Noise Margin](https://user-images.githubusercontent.com/22131133/124061643-e4725580-da4c-11eb-8ff3-85c83930936c.PNG)
 
- The decoupling capacitor is huge capacitor filled with charge which decouples the circuit from power supply means whenever switching happens at any node inside a circuit, the coupling capacitor provide nessesary current through charge flowing. Decoupling capacitors are placed close to a circuit block hence no voltage drop.
- The all preplaced cells are surrounded by decoupling capacitors,hence all preplaced cells get their power supply from decoupling capacitors.

4. Power planning
- Some blocks are given power supply with decoupling capacitor but there are interconnect between blocks which cant be powered by decoupling capacitor, has to be supplied by power supply only. So there can be bump at ground wire when lot of interconnect tries to be discharged and there might be voltage drop on Vdd line if large no of wires tries to get charged that is transition from 0 to 1. Hence if we have only one poer supply then these problems occurs.
- Hence in chip, we have multiple power supply which forms rectangular mesh. Any logic or interconnect can take current from nearest power supply line. Hence chances of occuring bump at Vss and drops at Vdd becomes low.

5. Pin placement and logical cell placement blockage
- The pins which has direct connectivity with preplaced cells are placed near of preplaced cells. Generally clock pins sre of larger size than other pins because clock is driving all sequencial components. Hence larger the size, lower is resistance.
- The gap between core and die which is reserved for pins hence it is blocked by some logical blockage so that automated tool cant use that place for placement and routing.

# Floorplan using OpenLANE
Variables for floorplan
![variables for foorplanning](https://user-images.githubusercontent.com/22131133/124070764-f4ddfc80-da5b-11eb-874f-07778c205e03.PNG)
These varibles can be modified according to our need from floorplane.tcl

# Full layout seen in magic after floorplanning
![entire lay_out](https://user-images.githubusercontent.com/22131133/124228070-41dbd480-db29-11eb-8776-adb1918e378b.PNG)

# Uniform distance I/O pins
![uniform distance IO pins](https://user-images.githubusercontent.com/22131133/124228208-7b144480-db29-11eb-8b3e-80ae6c16d854.PNG)

# Horizontal I/O pins metal layer
![layer_io_horizontal_pin](https://user-images.githubusercontent.com/22131133/124228289-a4cd6b80-db29-11eb-89d8-6561bbc1149a.PNG)

# Vertical I/O pins metal layer
![vertical io pins layer](https://user-images.githubusercontent.com/22131133/124228389-c4649400-db29-11eb-8671-94abe6e4f84f.PNG)

# Preplaced cells
![preplaced_cells](https://user-images.githubusercontent.com/22131133/124228469-dfcf9f00-db29-11eb-9a76-68f8f9bca85a.PNG)

# Palcement and Routing

1. Bind netlist with physical cells
- shape of gate determines functionality of gate but in reality we have rectangular box with specific width and height for all gates and components
- library has verious gates with different sizes, a gate with lesser size is slower compared with a cell having large size. Laso library has timing information of all cells

2. Placement
- Placement of all blocks is done such that preplaced cells are not moved.
- First try to place cells near to each other which are connected, so no need of repeater/buffers between the cells
- If distance between cells is large then buffers are needed so there is signal integrity maintained. 
- insetring repeaters based on estimating wire length and capacitance, if wire lenght between cells is large then signal loses its strength.
- If some perticular circuit is working on high frequency, then all connected cells are placed very close to each other.
- Placing buffers is called as placement optimization 

3. Congestion aware placement using RePlace
- First global placement then detailed placement Golbal - No legelization i.e cells can overlap. Purpose is to reduce wire length  Detailed placement - cells cant be overlap
- Reduction of Half Parameter Wire Length (HPWL) is main focus in global placement
# Layout after placement
![layout_after_placement](https://user-images.githubusercontent.com/22131133/124259657-61d0bf80-db4c-11eb-974e-7c935f17584f.PNG)
# zoomed version
![zoomed_version_after_placement](https://user-images.githubusercontent.com/22131133/124259698-6eedae80-db4c-11eb-9cfc-f46e789a51c7.PNG)

4. Standard cell design flow
- Standard cells such as gates,flipflop,latch etc kept in library. It has different sizes of different cells also with different functionalities.
- Larger cells has high drive strength  
- Inputs for cell design flow - Process Design Kit (PDK) consist of DRC and LVS rules, SPICE models and library and user defined specs.
   - SPICE models has constant such as Tox, Cox, etc
   - Library & used defined specs such as cell height, supply voltage, metal layers, pin locations
- Design step - Circuit design, layout design and characterization
   - Circuit design has steps such as function implementation using NMOS and PMOS and designing W/L of PMOS and NMOS depending upon given values.We get cercuit design language (CDL) file as a output of circuit design.
   - Layout design has steps such as get the NMOS and PMOS network graph then find euler's path then draw stick diagram
