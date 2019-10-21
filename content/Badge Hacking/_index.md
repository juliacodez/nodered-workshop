---
title: "Badge Hacking"
weight : 45
---

We've added a little Easter Egg onto your badge, there's an application in the Menu called *NodeRED Workshop*. If you launch it you'll see that not much happens at first - however the surprise is that you can control your badge from NodeRED!

## MQTT

For this we are using the [MQTT](http://mqtt.org/) protocol which is a lightweight protocol for real time control of internet of things devices. It's whats known as a Publish/Subscribe protocol or "PubSub".

The application on the badge will connect to WiFi and then connect to the MQTT server (known as a "broker") and subscribe to a topic. If the app isn't open it's not connected.

NodeRED has support for MQTT built in by default. So let's go ahead and set up a flow to publish some messages to an MQTT topic and see what we can do with them on the badge.

## Create A Payload

We need something to trigger our message, initially lets just use an "inject" node. Later on you could make this more interesting, for example you could add this to your IVR or control it via a text message.

Within the "inject" node we need to configure the data we will publish to our badge, open up the inject node and set the Payload type dropdown to `JSON` then in the text box enter the following string `[0,127,0,64,127,0,127,0,0,0,0,127,0,96,127]`.

This is a set of values to configure the RGB LEDs on the badge, each LED takes 3 values for red, green and blue between 0 and 255, there are 5 LEDs so we send 15 values.

> These are actually GRB LEDs so the first of the three values is for green, the middle one for red, and the last one for blue.

The badge will also convert the values we sent to the LEDS into frequencies to play on the speaker.

![Data to Inject to the Badge](/Badge_Inject.png)

## Publish to MQTT

Now we need to set up the MQTT Publisher. Start with an "mqtt" node from the "output" section of the palette.

1. First we need to configure the broker we will use, select "Add new mqtt-broker config" and click the pencil icon to edit.
    * Under "Server" enter `test.mosquitto.org` and put the same in the "Name" field.
    * Leave everything else as the defaults, and click the "Add" button.

    ![MQTT Broker Config](/MQTT_Broker_Config.png)

2. Back in the MQTT node we need to set our topic to be the address of our badge, this is shown on the screen when you start the app and will be something like `/AB12DE`.

3. Also in the MQTT config screen, set the QoS to 2.

    ![MQTT Config](/MQTT_Config.png)

4. Your flow should look like this:

    ![Badge Flow ](/Badge_Flow.png)

Now click the button on the inject node and your badge should respond.


## Further Steps

* Try integrating the MQTT event to your IVR to trigger the badge from a call.
* Play around with the different values to make different patterns on the LEDs.
* Can you control the color of the LEDs by SMS? eg Text `Green` or `Red`.


