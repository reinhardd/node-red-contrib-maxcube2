# node-red-contrib-maxcube2
A collection of node-red nodes to control the eQ-3 Max! Cube

## Introduction
### History
node-red-contrib-maxcube2 is a fork of https://github.com/ivesdebruycker/node-red-contrib-maxcube using https://github.com/normen/maxcube2.

### Changes from node-red-contrib-maxcube
- Settings to temporary disable maxcube connection (the cube can handle only one TCP connection at a time, since maxcube/maxcube2 use a permanent connection if you want to temporary use another client you have to stop node-red...now you can disable in node settings)
- Node to query devices configurations
- Set mode to all devices with a single payload ({"mode" : "AUTO"} without rf_address)
- I'm planning add some additional features like schedule settings

The old API didn't change currently so it's a drop-in replacement.

## Installation
```
cd $HOME/.node-red
npm install node-red-contrib-maxcube2
```
Restart Node-RED.

## Nodes
### maxcube config node
Set ip and port of your Max! Cube

### maxcube (input)
A node to query the eQ-3 Max! Cube for device states.

Whenever an input message is received, device states are updated from the Max! Cube, and sent as separate messages (one for each device) with following structure:
```
{
  "rf_address": "0abc12",
  "initialized": true,
  "fromCmd": false,
  "error": false,
  "valid": true,
  "mode": "MANUAL",
  "dst_active": true,
  "gateway_known": true,
  "panel_locked": false,
  "link_error": false,
  "battery_low": false,
  "valve": 0,
  "setpoint": 5,
  "temp": 15.4,
  "device_name": "Termosifone"
  "room_name": "Bagno"
}
```

### maxcube configuration (input)
A node to query the eQ-3 Max! Cube for device states.

Whenever an input message is received, device configuration are updated from the Max! Cube, and sent  and send it as single message with following structure:
```
{
   "18b444":{
      "rf_address":"18b444",
      "device_type":1,
      "serial_number":"OEQ1131666",
      "comfort_temp":21.5,
      "eco_temp":16.5,
      "max_setpoint_temp":30,
      "min_setpoint_temp":4.5,
      "temp_offset":0,
      "max_valve":100
   },
   "1709a1":{
      "rf_address":"1709a1",
      "device_type":1,
      "serial_number":"NKF0004984",
      "comfort_temp":21.5,
      "eco_temp":16.5,
      "max_setpoint_temp":30,
      "min_setpoint_temp":4.5,
      "temp_offset":0,
      "max_valve":100
   }
}
```


### maxcube (output)of a device
A node to set the temperature and/or the mode of a device.
Valid modes are "AUTO", "MANUAL", "BOOST" and "VACATION".

Accepts messages with payload of type object with following structure:

MANUAL on specific device
```
{
  "rf_address": "0abc12",
  "degrees": 20,
  "mode":"MANUAL"
}
```
MANUAL on all devices
```
{
  "degrees": 20,
  "mode":"MANUAL"
}
```
AUTO on all devices
```
{
  "mode":"AUTO"
}
```
VACATION on all devices until specific date (ISO 8601)
```
{
  "mode":"VACATION",
  "untilDate" : "2019-06-20T10:00:00Z"
}
```
