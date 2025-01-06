# Gigabit-capable passive optical networks (G-PON)

## Abbreviations and acronyms
SFU                - Single Family Unit
HGU                - Home Gateway Unit

## Transmission convergence layer specification
### 10 Activation method
#### 10.2 Activation mechanism at the ONU
Seven ONU states are defined:
O1 – Initial state
O2 – Standby state
O3 – Serial_Number state
O4 – Ranging state
O5 – Operation state
O6 – POPUP state
O7 – Emergency Stop state

## ONT management and control interface
### 3. Definitions
downstream: Downstream is a traffic flow from OLT to ONT.


### 8.2 Managed entity relation diagrams
![](https://i.imgur.com/mL3opqp.png)


### 9. MIB description
This clause defines all ONT managed entities (MEs) of interest to G-PON.

These clauses are organized as follows:
9.1 Equipment management
9.2 ANI management
9.3 Layer 2 data services
9.4 Layer 3 data services
9.5 Ethernet services
9.6 802.11 services
9.7 xDSLservices
9.8 TDM services
9.9 Voice services
9.10 MoCA
9.11 Traffic management
9.12 General purpose MEs
9.13 Miscellaneous services


#### 9.3 Layer 2 data services
![](https://i.imgur.com/wdPctPU.png)

### 9.4 Layer 3 data services
![](https://i.imgur.com/uRnc4e5.png)

### Appendix II, OMCI message set
#### II.2 Message layout
##### II.1.4 Get, get response, create response and set messages
For an attribute mask, a bit map is used in the get, get response, create response and set messages. This bit map indicates which attributes are requested (get) or provided (get response and set). The bit map is composed as follows:

![](https://i.imgur.com/oNzQwaq.png)

#### II.2.9 Set
![](https://i.imgur.com/HCqlL5j.png)



## Reference
G.984.1 General characteristics
G.984.2 Physical media dependent (PMD) layer specification
G.984.3 Transmission convergence layer specification
G.984.4 ONT management and control interface specification