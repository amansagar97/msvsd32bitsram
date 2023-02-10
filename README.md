# msvsd32bitsram
## task - Characterizing an inverter
We will first create an inverter using the Sky130 PDK that we installed through the OpenPDK. To do that, let's first create an inverter. 

<p align="center">
  <img src="https://github.com/amansagar97/msvsd32bitsram/blob/main/inverter_sch.jpg">
</p>

For the scematic above, we can create symbol in two ways. We generate the symbol through **Symbol->Make symbol from schematic (A)** and it will generate the symbol shown in the left. We can do it manually using the drawing tools provided within the **xschem** and I created the one on the right.

<p align="left">
  <img width=500 src="https://github.com/amansagar97/msvsd32bitsram/blob/main/inverter_sch2.jpg">
  <img width=500 src="https://github.com/amansagar97/msvsd32bitsram/blob/main/inverter_as3.jpg">
</p>

When we create a custom symbol, we have to describe the global schematic property with something like given below. This is to make sure that SPICE recognizes our device when we use in our design. This can be set by clicking anywhere in the blank space and pressing **q** and entering the below informatin in the text box that appears as shown in the figure below.

```
type=subcircuit
format="@name @pinlist @symname"
template="name=X1"
```
<p align="center">
  <img width=600 src="https://github.com/amansagar97/msvsd32bitsram/blob/main/inverter_as4.jpg">
</p>

I also created a buffer from the inverter that I created above as shown below. 

<p align="left">
  <img width=500 src="https://github.com/amansagar97/msvsd32bitsram/blob/main/buffer_level0.jpg">
  <img width=500 src="https://github.com/amansagar97/msvsd32bitsram/blob/main/buffer_level1.jpg">
</p>


Now, characterizing the inverter circuit with the following setup leads to the output shown in the waveform.

In order to run the simulation, we have to add a **code** block with the following configuration. This is done so that SPICE can expand the sub-circuit call with the Sky130 library files.

```
name=inverter_spice only_toplevel=false value="

.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.control
  dc V2 0 1.8 0.01
  plot a b
.endc

.save all
"
```

Then, we can add the **code_shown** block with the following transient values. 

```
name=inverter_spice1 only_toplevel=false value=".tran 0.01n 1u
.save all"
```

<p align="left">
  <img width=500 src="https://github.com/amansagar97/msvsd32bitsram/blob/main/inverter_setup.jpg">
  <img width=200 src="https://github.com/amansagar97/msvsd32bitsram/blob/main/inverter_setup_1.jpg">
</p>

And, generating the netlist and running the simulation, we get the following output waveform.

<p align="center">
  <img width=500 src="https://github.com/amansagar97/msvsd32bitsram/blob/main/inverter_setup_2.jpg">
</p>
