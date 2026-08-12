## Sensors, Actuators, ECU decisions, ECU Programming
Engine: CBR600RR 2009  

Author: Jaine Ratush  
Objectives: List Required Sensors, Find MoTec ECU that meets requirements, Check if we have all sensors.  

Required Sensors according to 2007 Service Manual I found online:   
### IO list
| Component | Purpose | How/What | 
|---|---|----:|    
| **Knock** | Detects abnormal combustion| contact, not required but we should have it |  
| **MAP(Manifold Air Pressure)** | Measures intake-manifold pressure to determine engine load | Piezoelectric Diaphram Flexes w/ Pressure |  
| **IAT (Intake Air Temp)** | Measures intake-air temperature for air-density/fueling calculations | NCT Thermistor(Temp sensitive resistor) |  
| **ECT (Engine Coolant Temp)** | Measures engine coolant temperature for fueling, warm-up, and engine-control strategies | NCT Thermistor |   
| **CMP** | Detects camshaft position for engine synchronization and sequential control | Variable Reluctance based on wiring |  
| **CKP** | Detects Crank angle & RPM | Variable Reluctance |   
| **TS(Throttle)** | Controls output based on driver input on gas plate | Potentiometer |  

Note: These are directly from Honda's stock ECM for the 2007-2009 CBR600RR.  
You may notice **Lambda** and **O2** sensors are missing. That's becuase the stock ECM/ECU is Open Loop and was already calibrated by Honda Engineers.     
However, we're using an aftermarket ECU becuase FSAE rules on air-intake would get us a very poor Open Loop results, and the Honda ECM would require extensive re-mapping. So we'll need new schematics including Lambda and O2.  
*Open Loop*- Pre-mapped range of Injector Pulse Widths, no instantaneous correction for an ideal 14.7:1 AFR.    

### Actuator List  
| Component | Purpose | How/What |   
|---|---|----:|    
| **IACV(Idle Air Control Valve)** | Controls the amount of air bypassing the throttle for idle-speed control like during cruising | Solenoid + Plunger or Stepper motor + Lead Screw |    
  
ECU Model: **M150** -Depends on discount, team chose MoTec in general due other FSAE teams also using it. M150 is ideal, as it has a plethora of IO pins and CAN.   

My Reasoning: PE3 was developed with FSAE in mind. It has the required IO connections, stepper drivers, and IACV driver for the CBR600RR. It's also much cheaper and simpler than a MoTec. Perfect for getting our team rolling.    

Missing Sensors *Inventory*: Lambda, O2.   
I'll upload a SpreadSheet/BOM here soon.     
We'll need to build our own wire harness in the near future: https://www.rbracing-rsr.com/wiring_ecu.html
