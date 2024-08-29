# CBus <-> CGate <-> MQTT
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

## Folow

1. Button press on CBus wall switch (for example) broadcasts a CBus group address on the CBus.
2. A CBus relay (typically) receives the group address broadcast and turns the light (for example) 
on/off (or dim/bright) according the the event data broadcast to the group address.
3. The group address broadcast traverses the CBus serial interface to the serial port on the Home Assistant host via a serial to USB adaptor.
4. Ser2Sock converts the serial message (at 9600 baud) to a TCP/IP message on port 10001.
5. CGateWeb receives the TCP/IP group address broadcast and translates it into a MQTT message and publishes it to the MQTT broker.
6. MQTT broker sends the message on to subscribers, such as Home Assistant.

### Also in reverse

1. A Home Assistan button changes state.
2. The new state is published by Home Assistant to the MQTT broker.
3. The MQTT broker sends the message to the CGateWeb, which translates it into a CGate group address message.
4. Ser2Sock sends the message to CBus via the serial protocol using the USB to serial converter.

### CGateWeb

Connects to CGate Comport and Eventport by Telnet.
Issues a GET command to CBus

Connects to MQTT broker
Subscribes to cbus/write/#

It apears that HA messages don't get to cbus until a first message has come from cbus. Hence GET ALL. Don't yet understand why. Try without Retain reads.

### MQTT Explorer

cbus/write/ topics are messages from Home Assistant which will be bridged to CBus.
cbus/read/ topics are messages from CBus which will update Home Assistant.
