# TSPC_DFF_Using_LTspice
True Single-Phase Clock (TSPC) Flip-Flops, based on dynamic logic implementation, are area-saving and high-speed compared to standard static flip-flops. Furthermore, logic gates can be embedded into TSPC flip-flops which significantly improves performance.

## *Contents*
------------
* [Problem Statement](#problem-statement)
* [Detailed Transistors Sizing Analysis](#detailed-transistors-sizing-analysis)
* [Schematic and Netlist](#schematic-and-netlist)
  * [Schematic](#schematic)
  * [Netlist](#netlist)
* [Simulation Analysis](#simulation-analysis)
* [Calculation of Setup and Hold Time](#calculation-of-setup-and-hold-time)
* [Calculation of Power Delay Product](#calculation-of-power-delay-product)


---------
## Problem Statement
Implement TSPC positive edge-triggered D Flip Flop (Follow the Textbook for the design). Compute its setup and hold time. Optimize its power delay product. Compute the maximum operating frequency.
## Detailed Transistors Sizing Analysis
![trans_sizingf](https://user-images.githubusercontent.com/100671647/234932224-da83b432-47ab-480e-a149-d36a67bd2669.png)
![logic_effort](https://user-images.githubusercontent.com/100671647/234932505-aa8d7bca-e161-4fad-a791-af0eded19ac2.png)
![parasitic_cap](https://user-images.githubusercontent.com/100671647/234933020-a80d923f-d0ee-4371-9e7f-85ad67542e3b.png)
![delay](https://user-images.githubusercontent.com/100671647/234933350-e5583fd8-3dc6-4f40-83ef-d775b062875e.png)


## Schematic and Netlist
### Schematic
![image](https://user-images.githubusercontent.com/100671647/234933536-de4e0ad5-2496-477a-a337-d0b87c700a5c.png)
### Netlist
    M1 VDD D 1 VDD PMOS l=180n w=0.8u 
    M2 1 CLK X VDD PMOS l=180n w=0.8u 
    M3 VDD CLK Y VDD PMOS l=180n w=5.6u 
    M5 VDD Y Z VDD PMOS l=180n w=18u 
    M7 VDD Z Q VDD PMOS l=180n w=68u
    M8 X D 0 0 NMOS l=180n w=0.1u 
    M9 2 CLK 0 0 NMOS l=180n w=9u
    M10 3 Y 0 0 NMOS l=180n w=4.5u 
    M11 Q Z 0 0 NMOS l=180n w=17u 
    M4 Y X 2 0 NMOS l=180n w=9u
    M6 Z CLK 3 0 NMOS l=180n w=4.5u 
    VDD VDD 0 1.8
    V1 CLK 0 PULSE(0 1.8 0 0.1f 0.1f 7n 15n) 
    V2 D 0 PULSE(0 1.8 0 0.1f 0.1f 16n 25n) 
    C1 Q 0 500f
    C2 D 0 2f
   .model NMOS NMOS
   .model PMOS PMOS
   
## Simulation Analysis
![image](https://user-images.githubusercontent.com/100671647/234935217-90eb78b1-8c78-4657-b441-0514cef93257.png)
*RED: CLOCK *BLUE:D-INPUT *GREEN:Q-OUTPUT
![image](https://user-images.githubusercontent.com/100671647/234935615-8a24bbc2-38d5-49f1-80ef-e5916920c51e.png)
*Propagation Delay* = 140ps
## Calculation of Setup and Hold Time
### Setup time

Minimum time requires for which the data should be stable before the active edge of the clock. Tcq is clock to output delay. Tdc is data to active edge difference.
### Setup Rise time

To obtain the set-up time of the register, we progressively skew the input with respect to the clock edge until the circuit fails.The set-up time simulation assuming a skew of 71 psec and 61 psec. For the 71 psec case, the correct value of input D is sampled (in this case, the Q output remains at the value of VDD). For a skew of 61 psec, an incorrect value propagates to the output (in this case, the Q output transitions to 0). Node QM starts to go high while the output of I2 (the input to transmission gate T2 ) starts to fall. However, the clock is enabled before the two nodes across the transmission gate (T2 ) settle to the same value and therefore, results in an incorrect value written into the master latch.The set-up time for this register is therefore 71psec.
![image](https://user-images.githubusercontent.com/100671647/234935924-21de4e72-f6bb-440c-a6b7-5b3f6cc9753e.png)
![image](https://user-images.githubusercontent.com/100671647/234936047-282d0fd8-4788-43f3-9e67-1ed9eaa8754c.png)
*Below analysis shows setup time calculation*
![image](https://user-images.githubusercontent.com/100671647/234936121-03e497e5-4976-4995-88b5-79a6707a3058.png)
#### Set-up time=71ps

### Hold time

Minimum time required for which the data should be stable after the active edge of the clock.
### Hold Rise time
In a similar fashion, the hold time can be simulated. The D input edge is once again skewed relative to the clock signal till the circuit stop functioning.
![image](https://user-images.githubusercontent.com/100671647/234936356-0456815c-6673-4e3e-a447-b76a9290663d.png)
![image](https://user-images.githubusercontent.com/100671647/234936368-87f93d03-439f-4ad9-9e91-400093bee69c.png)
![image](https://user-images.githubusercontent.com/100671647/234936383-b917783a-274d-4f44-82bc-cd901ba2fc83.png)

#### Above analysis result shows calculation of hold time.
#### Hold time=61.2ps 

## Calculation of Power Delay Product
![image](https://user-images.githubusercontent.com/100671647/234936497-0f9b20af-d98e-49ec-9853-c7ffc776f9d2.png)
![image](https://user-images.githubusercontent.com/100671647/234936508-f27a1d44-76d0-4cb4-8c47-ecce184ab24f.png)



