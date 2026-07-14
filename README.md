# Turning an electric boiler into a smart thermal solar battery
This repository has the sole purpose of describing my personal attempt to turn a standard electrical boiler into smart thermal storage. 

*Please note:*
- *There are many alternatives and different approaches. There is no single 'right' approach.*
- *Always make sure to comply with local electrical legislation.*
- *Always make sure to comply with good sanitary/plumbing practices (eg. overpressure valves, expansion vessels*

## Hardware
- 'Dumb' electrical boiler (eg. Bulex SDC 200 V Tri) 
  - No electrical controller
  - Bi-metal thermostat which does NOT require electrical power
- Home Assistant Green
- Home Wizard P1 meter
- Home network on the Unifi platform
- Shelly PRO4EM

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

<img width="1211" height="756" alt="image" src="https://github.com/user-attachments/assets/08f909d6-0970-43f8-a19f-76423f7dfd63" />

### Alternative: e-boiler in series with existing storage tank heater
*Please note:*
- *Always make sure to comply with good sanitary/plumbing practices (eg. overpressure valves, expansion vessels*

<img width="1155" height="551" alt="image" src="https://github.com/user-attachments/assets/a6544fbb-89c1-4e79-ae56-4263e8a4af23" />

