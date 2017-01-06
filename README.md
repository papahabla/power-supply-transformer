# WORK IN PROGRESS

# Transformer Power Supply

## Specifications

* 24 VDC
* 7.5 AMP
* Full wave rectified
* Regulated
* Fault protection

## BOM

| Name                     | # |
|--------------------------|---|
| Transformer              | 1 |
| Diodes                   | 4 |
| Smoothing Capacitor      | 1 |
| Output Capacitor         | 1 |
| LDO Regulator            | 1 |
| LDO Heat Sink            | 1 |
| ~.5 Ohm 10W Resistor     |*1 |
| Fuse                     | 1 |
| Fuse Cartridge           | 1 |
| Power Switch             | 1 |
| Perf Board               | 1 |
| Hook Wire                | 2 |
| Quick Connects           | 6 |


## Transformer

[VPS20-8800](http://catalog.triadmagnetics.com/Asset/VPS20-8800.pdf)



## DC Regulator

[MIC29752WWT](http://ww1.microchip.com/downloads/en/DeviceDoc/20005685A.pdf)

### Minimum Load Current

If the output current is too small, leakage currents dominate and the output voltage rises. The minimum load current for the choosen regulator is 10 mA.

### Adjustable Output Voltage

The adjustable regulator versions, MIC29xx2 and MIC29xx3, allow programming the output voltage anywhere between 1.25V and the 25V. Two resistors are used. The resistor values are calculated by
```
vout = 24.
r1 = r2 * (vout / 1.24 - 1.) 
```
The resistor values can be scaled to provide the mimimum load requirement described above.

## Useful Tutorials

* [Variable Voltage Power Supply](http://www.electronics-tutorials.ws/blog/variable-voltage-power-supply.html)
* [Unregulated Power Supply](http://www.electronics-tutorials.ws/blog/unregulated-power-supply.html)


