---
title: "Badge Hacking"
weight : 45
---

We've added a little Easter Egg onto your badge, there's an applicaiton in the Menu called *Node-RED Workshop* if you launch it you'll see that no much happens, however what this means is that you can control your badge from NodeRED!

## MQTT 
For this we are using the MQTT protocol which is a lightweight protocol for real time control of internet of things devices. It`s whats known as a Publish/Subscribe protocol or PubSub
The applicatin on the badge will connect to WiFi and then connect to the MQTT server, known as a Broker and subscribe to a topic. If the app isn't open its not connected.
Node-RED has support for MQTT built in by default. So lets go ahead and setup a flow to Publish some messages to this topic.


## Create our payload

We need something to trigger our message, initially lets just use an inject node, later on you culd add this to your IVR or control it via a text message.
Within the inject node we need to configure the data we will publish to our badge, open up the inject node and set the Payload type dropdown to `JSON` then in the text box enter the following string `[0,127,0,64,127,0,127,0,0,0,0,127,0,96,127]`
This is a set of values to configure the RGB LEDs on the badge, each LED takes 3 values for Red Green and Blue between 0 and 255, there are 5 LEDs so we send 15 values.
The badge will also convert these vaules into frequecies to play on the speaker.

![Data to Inject to the Badge](/Badge_Inject.png)

## Publish to MQTT

Now we need to setup our MQTT Publisher, use the *mqtt* node in the *Output* section of the palette.
First we need to configure the broker we will use, select `Add new mqtt-boker config` and click the pencil icon
Under server enter `test.mosquitto.org` and put the same in the Name field.
Leave everything else as the defaults, and Click the `Add` button

![MQTT Broker Config](/MQTT_Broker_Config.png)


Back in the MQTT out node we need to set our topic to be the address of our badge, this is shown on the screen when you start the app and will be something like `/AB12DE` We also need to set the QoS to 2.

![MQTT Config](/MQTT_Config.png)

Your Flow should look like this:

![Badge Flow ](/Badge_Flow.png)

Now click the button on the inject node and your badge should respond.


## Further Steps

* Try integrating the MQTT event to your IVR to trigger the badge from a call
* Play around with the different values to make different patterns on the LEDs
* Can you control the color of the LEDs by SMS? eg Text `Green` or `Red` 


