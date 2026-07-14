# Turning an electric boiler into a smart thermal solar battery
This repository has the sole purpose of describing my personal attempt to turn a standard electrical boiler into smart thermal storage. This smart storage can be used in conjuction with excess PV energy or dynamic pricing.

*Please note:*
- *There are many alternatives and different approaches. There is no single 'right' approach.*
- *Always make sure to comply with local electrical legislation.*
- *Always make sure to comply with good sanitary/plumbing practices (eg. overpressure valves, expansion vessels*
- *Always take into account minimal temperatures to avoid legionella infections*

## Hardware
- 'Dumb' electrical boiler (eg. Bulex SDC 200 V Tri) 
  - No electrical controller
  - Bi-metal thermostat which does NOT require electrical power
- Home Assistant Green
- Home Wizard P1 meter
- Home network on the Unifi platform
- Digital meters for both electricity and water
- Shelly PRO4EM
- Motorized 3-way valve (eg. Honeywell VC4613)

## Software
- Home assistant OS (updated to the latest version)
- EVCC running as app in Home Assistant (updated to the latest version)
### Home Assistant apps
- EVCC : https://github.com/evcc-io/hassio-addon
### Home Assistant integrations
- Shelly (standard Home Assistant integration) 
- HA-EVCC by Matthias Marquardt : https://github.com/marq24/ha-evcc

## Sanitary installation
### e-boiler in parallel with existing run trough heater (my set-up)
*Please note:*
- *Always make sure to comply with good sanitary/plumbing practices (eg. overpressure valves, expansion vessels*

This is my personal set-up. By controlling the motorized 3-way valve i'm able to choose the preferred heater (e-boiler or gas). By default I activate the e-boiler. Whenever i'm not sure there is sufficient hot water in the e-boiler the system falls back to gas. When used only in combination with excess PV, fall-back situations can occur during wintertime on rainy days.

<img width="1211" height="756" alt="image" src="https://github.com/user-attachments/assets/08f909d6-0970-43f8-a19f-76423f7dfd63" />

### Alternative: e-boiler in series with existing storage tank heater
*Please note:*
- *Always make sure to comply with good sanitary/plumbing practices (eg. overpressure valves, expansion vessels*

As an alternative for heaters with a storage tank, an e-boiler can be put in series. This way the water at cold inlet of the heater is already pre-heated. Best results are achieved when the setpoint of the e-boiler is higher than the setpoint of the existing heater.

<img width="1155" height="551" alt="image" src="https://github.com/user-attachments/assets/a6544fbb-89c1-4e79-ae56-4263e8a4af23" />

## Electrical installation
### 3-phase e-boiler with motorized 3-way valve and Shelly PRO 4PM (my set-up)
*Please note:*
- *Always make sure to comply with local electrical legislation.*
- *NEVER (ever!) take any action that bypasses the internal thermostat or mechanical safety devices inside the e-boiler.*

In my personal set-up i choose for a 3-phase boiler. These boilers consist out of 3 smaller resistors (3x 800W in my set-up) instead of one bigger one (typically 1600W or 2400W). Since the load is resistive, there is no need for actual 3 shifted phases. When connecting the N conductor (in Y) I can safely power all 3 resistors from a single phase. 

By controlling the 3 resistors seperately i can control the total power consumed by the boiler:
- 0W (no resistors powered)
- 800W (1 resistor powered)
- 1600W (2 resistors powered)
- 2400W (3 resistors powered) 

<img width="1360" height="764" alt="image" src="https://github.com/user-attachments/assets/be6e80a1-93f0-4e43-8514-22847e25def3" />

### Alternative: single phase e-boiler without valve with Shelly PRO 2PM
*Please note:*
- *Always make sure to comply with local electrical legislation.*
- *NEVER (ever!) take any action that bypasses the internal thermostat or mechanical safety devices inside the e-boiler.*

Also single phase boilers can be controlled in the same way (with or without 3-way valve). Off course the total power consumed cannot have intermediate values. This is a standard on-off controll pattern.

- 0W (no resistors powered)
- Full power: Typically 1600W of 2400W (1 resistor powered)

<img width="1227" height="758" alt="image" src="https://github.com/user-attachments/assets/1b7cfd7f-f30d-46fc-8c29-df2fec932067" />

## Software control
This section assumes a working instance of Home Assistant with the control device (in my case Shelly PRO 4PM) already set-up. A basic knowledge of both Home Assistant and EVCC is assumed. Please refer to the specific documentation for relevant manuals.
### Controlling the e-boiler trough EVCC
Each individual resistor (when using a 3-phase model) or the main resistor (when using a single phase model) can be added into EVCC. When using Shelly controllers they can be loaded into EVCC either trough the Shelly template in EVCC or as a more generic Home Assistant Switch. 

*Note:*
- *If you want to use 'plans' with the e-boiler (ex. dynamic pricing) you should add the devices as a 'charger', not as as 'heating' device. For the moment charging plans are NOT available for heating devices in EVCC*

Once added into EVCC all standard EVCC functionalities are now usable with the e-boiler
- Solar charging
- Charging plans based on dynamic pricing
- Interaction with Home Batteries
- Load management (eg. to interact with the Flemish 'capaciteitstarief')
- ...

### Home Assistant interactions
The HA-EVCC integration by Matthias Marquardt (https://github.com/marq24/ha-evcc) exposed all relevant EVCC controls and sensors as HA entities
#### Senor : e-boiler fully charged
A very usefull thing to know is when the e-boiler is fully charged. This basically means the controller has switched one or more resistors on but the mechanical thermostat of the e-boiler has interupted the circuit.

*Human logic: The e-boiler is 'at temperature' when at least one resistor is switched on, but the total power consumed has been 0W for at least one minute*


```#Boiler op temperatuur
    - name: "boiler_op_temperatuur"
      unique_id: boiler_op_temperatuur
      delay_on:
          minutes: 1
      state: >
          {% if is_state('binary_sensor.boiler_status', 'on') and (states('sensor.boiler_vermogen_totaal') | int < 10) %}
            True
          {% else %}
            False
          {% endif %}
```
