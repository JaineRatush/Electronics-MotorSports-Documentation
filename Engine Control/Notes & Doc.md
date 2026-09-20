## Sensors, Actuators, ECU Decisions, ECU Programming

Engine: **Honda CBR600RR 2009**  
ECU: **MoTeC M150** — replacing the stock Honda ECM  
Team: **Cooper Union Motorsports — Formula SAE ICE**  
Original Notes Author: **Jaine Ratush**  
Updated: **September 17, 2026**
Updated by: **James Kim**

**Objectives**: List Required Sensors, Find MoTec ECU that meets requirements, Check if we have all sensors

These notes expand the original **Notes & Doc.md**. Honda component information is based on the service manual, using the **after ’08** instructions in the manual. M150 connections below are proposed **input/output types**.

### IO List

| Component | Purpose | How/What |
|---|---|---|
| **MAP — Manifold Absolute Pressure** | Measures intake-manifold pressure for engine-load and air-charge calculations |  Piezoelectric Diaphram Flexes w/ Pressure(Note: stock MAP may not be ideal for sports engine)|
| **IAT — Intake Air Temperature** | Measures intake-air temperature for air-density and fueling calculations | NTC thermistor|
| **ECT — Engine Coolant Temperature** | Measures coolant temperature for warm-up fueling, fan control, and protection strategies | NTC Thermistor|
| **CMP — Camshaft Position** | Identifies engine-cycle phase for synchronization and sequential operation | **variable reluctance (VR)**.|
| **CKP — Crankshaft Position** | Detects crankshaft events for RPM and ignition/injection timing | Two-wire magnetic pickup; VR|
| **TPS / TP — Throttle Position Sensor** | Reports throttle opening and changes in driver demand | Potentiometer. It **measures** throttle position|
| **VS — Vehicle Speed Sensor** | Measures transmission-related speed | Powered pulse-output sensor. The Honda supply test specifies **battery voltage**, not 5 V.(IDK what that means lowk) |
| **Knock Sensor** | Measures block vibration associated with combustion knock | Non-resonating piezoelectric sensor with a **three-wire Honda diagnostic arrangement**.|
| **Engine Oil-Pressure Switch** | Indicates whether pressure is below its switching threshold | On/off switch; does not measure continuous pressure.**(ONE OF THE MOST IMPORTANT SENSOR: IF FALSE READING OCCUR ENGINE MAY BE SEVERLY DAMAGED)**|
| **Neutral Switch** | Indicates neutral | On/off contact circuit. |
| **EGCV Position Feedback — if retained** | Measures exhaust-valve servo position | Potentiometer feedback associated with the exhaust-control actuator. |
| **Lambda Sensor-Wideband model preferred** | Measures exhaust oxygen so the controller can report mixture as lambda | Wideband sensor **plus a matching external controller**; controller sends CAN data or a calibrated analog output to the M150.|
| **Oil/Fuel Pressure Transducers ** | Measure continuous pressure for logging and protection | Part-dependent: often powered analog sensors; CAN versions also exist.|
|**Oil Temperature Sensor**|Measures oil temperature can be read and tell the ECU if it is safe to run the engine or not.| Thermoresistor measurement|

*Note:* Not all of the sensors may be inventorized. Listed above is mostly stock sensors, with few exceptions like Lambda, Fuel Presure Transducers(These can help in engine fuel to air ratio and efficiency of the engine). Also reference the engine manual when in doubt. Please check inventory to see if we have inventory of these sensors. 
Note: These are directly from Honda's stock ECM for the 2007-2009 CBR600RR.  
You may notice **Lambda**/**O2** sensors are missing. That's becuase the stock ECM/ECU is Open Loop and was already calibrated by Honda Engineers.     
However, we're using an aftermarket ECU because FSAE rules on air-intake would get us a very poor Open Loop results, and the Honda ECM would require extensive re-mapping. So we'll need new schematics including Lambda/O2.    
*Open Loop*- Pre-mapped range of Injector Pulse Widths, no instantaneous correction for an ideal 14.7:1 AFR. 


### Sensor Functions — Important Details

**MAP:** Measures **absolute** pressure, not just vacuum. With the engine stopped and the manifold open to ambient air, its reading should be close to local atmospheric pressure. verify the calibration.

**TPS:** Calibrate closed and fully open positions, then check that the value moves smoothly through the full range. 

**IAT / ECT:** The ECU measures a voltage produced by the thermistor and a pull-up resistor. The Honda fuel-system specifications give IAT **1–4 kΩ at 20 °C** and ECT **2.3–2.6 kΩ at 20 °C**. 

**CKP / CMP:** Signal should be seperate. Tooth pattern, polarity, detection edge, thresholds, and synchronization should be configured and checked.

**Knock:** Honda’s diagram identifies an input-voltage line for open-circuit detection, an open-circuit detection line, and the vibration-output line. This is **not a generic 5 V / ground / signal sensor**. Resolve the diagnostic loop and signal reference before connecting it to the M150. Reliable knock control also requires suitable filtering and calibrated thresholds. [1, pp. 2-4–2-5 and 6-35–6-37]


### Protocols / M150 Input Types

Honda CBR600rr does not use complex digital hardware protocols such as CAN-bus. They usually use analog voltage signals or direct pulse/frequency signals wired directly to the ECM. 




| Component | Electrical Interface | Proposed M150 Input / Setup | Needs Its Own Signal Wire? |
|---|---|---|---|
| **MAP** | Analog voltage | **AV**; correct voltage-to-pressure calibration | Yes |
| **TPS** | Analog voltage | **AV**; closed/open calibration | Yes |
| **IAT** | Resistive / NTC | **AT**; correct thermistor calibration | Yes |
| **ECT** | Resistive / thermistor | **AT**; correct terminals and calibration | Yes |
| **CKP** | VR waveform / timing events | **UDIG**, configured for verified engine-speed pickup | Yes; dedicated pickup pair |
| **CMP** | VR waveform / synchronization events | Separate **UDIG**, configured for verified sync pickup | Yes; dedicated pickup pair |
| **VS** | Pulse / frequency | Suitable **UDIG**; verify levels, bias, and pulse scaling | Yes |
| **Knock** | Piezoelectric vibration signal | Dedicated **KNOCK** interface after wiring compatibility is resolved | Yes |
| **Oil-pressure / neutral switches** | Open/closed state | Suitable digital input with verified bias and polarity | Yes |
| **Bank-angle sensor** | Powered shutdown-related circuit | Determine retained shutdown design and compatible input before assigning | If retained |
| **EGCV position** | Analog voltage | **AV**, if retained | Yes |
| **Wideband controller** | CAN or analog voltage | Supported **CAN receive function**, or calibrated **AV** | CAN shares the bus; analog needs its own input |

**AV:** Analog Voltage. **AT:** Analog Temperature. **UDIG:** Universal Digital Input. These names identify hardware classes; the installed M1 package determines available functions and assignments. **CAN** is a message-based network used between compatible electronic modules(most of the stock sensors are not CAN-bus compatible, more on that later section).


### Actuator List  
| Component | Purpose | How/What |   
|---|---|----:|    
| **IACV(Idle Air Control Valve)** | Controls the amount of air bypassing the throttle for idle-speed control like during cruising | Solenoid + Plunger or Stepper motor + Lead Screw |    
  
ECU Model: **M150** -Depends on discount, team chose MoTec in general due other FSAE teams also using it. M150 is ideal, as it has a plethora of IO pins and CAN.   

Missing Sensors *Inventory*: O2.   
I'll upload a SpreadSheet/BOM here soon.     
We'll need to build our own wire harness in the near future: https://www.rbracing-rsr.com/wiring_ecu.html
