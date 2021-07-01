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


