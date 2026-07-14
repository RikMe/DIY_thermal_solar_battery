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
- B2500 meter by Tom Quist : https://github.com/tomquist/b2500-meter
### Home Assistant integrations
- Shelly (standard Home Assistant integration) 
- HA-EVCC by Matthias Marquardt : https://github.com/marq24/ha-evcc

## Configuration
### Plumbing
The Marstek app is not a textbook example of stability. It is however sufficient for initial configuration. Personally i don't use the app for anything after that. In order to access the modbus interface, the battery needs to have a wired connection to the network. No specfic settings are required for the modbus connection.
