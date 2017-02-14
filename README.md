# WORK IN PROGRESS

# Transformer Power Supply

## Specifications

* 24 VDC
* 5 AMP
* Full wave rectified
* Regulated
* Fault protection

## BOM

| Name                     | # |
|--------------------------|---|
| Transformer              | 1 |
| Rectifier Diodes         | 4 |
| Smoothing Capacitor      | 1 |
| LDO Regulator            | 1 |
| LDO Heat Sink            | 1 |
| LDO Output Capacitor (C1)| 1 |
| LDO Capacitor Adjust (C2)| 1 |
| LDO Protection diodes    | 1 |
| Fuse                     | 1 |
| Fuse Cartridge           | 1 |
| Power Switch             | 1 |
| Perf Board               | 1 |
| Hook Wire (14 AWG solid) | 2 |
| Quick Connects           | 6 |
| Circular Terminals(Big Cap) | 1 |

## Transformer

[VPS20-8800](http://catalog.triadmagnetics.com/Asset/VPS20-8800.pdf)

Given a mains peak voltages of 125 VAC, this transformer will output more than the 24+ VDC ultimately needed. As specified, the transformer will output 20 VRMS given a 115 VRMS input. However, it's not too uncommon to see nearly 125 VRMS so I used that as the upper limit.

```
Tratio = 115. / 20. # 5.75
Vmax = 125. / Tratio # 21.73 VRMS
Vpeak = Vmax * 1.414 # 30.7 PVAC
```

The full wave retification will then further attenuate the voltage slightly by two diode drops (`2 * .75` for regular diodes.)



## DC Regulator

### [LM338](http://www.ti.com/lit/ds/symlink/lm338.pdf)

* Good inut voltage range of up to 40 VDC
* 3 VDC headroom is required
* 5 AMP output current
* 3 terminal design
* No fault flag (TODO:)

#### Design

* R1 = 250 Ohm
* R2 = 4800 Ohm
* Cin = 0.1 uF (needed if regulator is more that 6 inches from smoothing capacitor)
* Cout = 1 uF (needed if transient response is a concern)
* Cadj = 10 uF (bypassing the ADJ pin to ground improves ripple rejection)
* D1 protection diode needed if Cout is included in design
* D2 protection diode needed if Cadj is included in design


The output voltage is set using resistor divider between the LDO *OUT* and *ADJ* terminals.
```python
Vout = Vref * (1+R2/R1) +Iadj * R2
```

The R1 value of 250 Ohms should provide the 1.25 VDC reference and the minimum current requirement of 50 mA.
```python
Iadj = .005 #mA
Vr1 = 1.25 #VDC
R1 = Vr1 / Iadj # 250 Ohm
```

Then, the value of R2 determines the voltage output level, *Vout* as the sum of the voltages accross *R1* and *R2*.
```python
Vout = 24. #VDC
Vr2 = Vout - Vr1 # VDC
R2 = (Vr1 + Vr2) / Iadj # 4800 Ohms
```

### [MIC29752WWT](http://ww1.microchip.com/downloads/en/DeviceDoc/20005685A.pdf)

Seemed like a good option but the input voltage is limited to 26 VDC and could be an issue with the 35 VDC output of the bridge rectifier. 

### Minimum Load Current

If the output current is too small, leakage currents dominate and the output voltage rises. The minimum load current for the choosen regulator is 10 mA.

### Adjustable Output Voltage

The adjustable regulator versions, MIC29xx2 and MIC29xx3, allow programming the output voltage anywhere between 1.25V and the 25V. Two resistors are used. The resistor values are calculated by
```
vout = 24.
r1 = r2 * (vout / 1.24 - 1.) 
```
The resistor values can be scaled to provide the mimimum load requirement described above.

## Useful Links and Tutorials

* [Variable Voltage Power Supply](http://www.electronics-tutorials.ws/blog/variable-voltage-power-supply.html)
* [Unregulated Power Supply](http://www.electronics-tutorials.ws/blog/unregulated-power-supply.html)
* [Calculator](http://www.changpuak.ch/electronics/power_supply_design.php)
* [John Errington's tutorial on Power Supply Design](http://www.skillbank.co.uk/psu/trfrec.htm)

