 This add-on uses CGate (by Clipsal) to bridge between CBus and CGateWeb. 
CGateweb (by the1laz) bridges between CGate and MQTT.
In Home Assistant's configuration.yaml you can configure MQTT: to process 
the MQTT messages for light, switch, sensor and cover platforms.

Example configuration.yaml
```mqtt:
  light: 
    - name: Dining Room lights
      state_topic: "cbus/read/254/56/34/state"
      command_topic: "cbus/write/254/56/34/switch"
      payload_on: "ON"
      payload_off: "OFF"
      brightness_state_topic: "cbus/read/254/56/34/level"
      brightness_command_topic: "cbus/write/254/56/34/ramp"
      on_command_type: "brightness"
      brightness_scale: 100
      retain: true 
      qos: 0
      unique_id: cBusGroupAddr34
```

The add-on doesn't do auto discovery but there is a little program that you may find useful
at https://github.com/bdnstn/cbusExtractEntities which will read your cbus configuration xml
file to create MQTT entities for Home Assistan's configuration.yaml.

 My CBus lacks a CNI, so I'm using a USB to serial converter (hardware) to connect to 
a CBus serial interface. To bridge from the network to the serial interface I use 
ser2sock (by nutechsoftware). I expect it wouldn't be dificult to modify the add-on
to allow direct connection using a CBuse CNI. Contributions are welcome!

 This is my first add-on, so constructive comments are also welcome. 
 
 I hope you find it useful. I've been using it for more than 2 years and it has be 
quite solid.
