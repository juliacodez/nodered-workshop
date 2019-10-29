---
title: "Badge Hacking"
weight : 45
---

We've added a little Easter Egg onto your badge, there's an application in the Menu called *NodeRED Workshop*. 
As a reward for getting this far we've got an additional board for your badge to add WiFi functionality, ask someone to get you a board and they will show you how to plug it in, Then open the NodeRED Workshop app from your badge menu.
If you launch it you'll see that not much happens at first - however the surprise is that you can control your badge from NodeRED!

## MQTT

For this we are using the [MQTT](http://mqtt.org/) protocol which is a lightweight protocol for real time control of internet of things devices. It's whats known as a Publish/Subscribe protocol or "PubSub".

The application on the badge will connect to WiFi and then connect to the MQTT server (known as a "broker") and subscribe to a topic. If the app isn't open it's not connected.

NodeRED has support for MQTT built in by default. So let's go ahead and set up a flow to publish some messages to an MQTT topic and see what we can do with them on the badge.

## Add a custom package

We have created a nodered package just for the campus badge, you will find it along with all sorts of other nodes in the pallette manager.

Click the menu in the top right corner of the dashboard, and select **Manage Pallette**

![Manage Broker Pallette](/Manage_Pallette.png)

Now select the **Install** Tab and search for **campusbadge**, you will be offered one package, click the install button and wait a few moments.

![CampusBadge Package](/campusbadge_package.png)

Once the package is installed you can close the pallette manager, in your pallete you will now have a new node in the Function section, its green and named **campusbadge**

Drag this node onto your flow

## Configure badge payload

The campusbadge node helps you to setup the payload that will be sent to your badge, you can set the color of each of the LEDs, a short message to be displayed and the frequency and duration of the tone played on the speaker. You also set the badge 4 digit ID reference in this node.

Go ahead and setup a pattern of colors for the lights aling with your message and speaker tone, some ideas are below.
You will find your badges 4 digit ID displayed on the badge screen once once you launch the NodeRED Workshop app and it says *MQTT Connected*.

![Badge Config](/badge_config.png)


## Publish to MQTT

Now we need to set up the MQTT Publisher. Start with an **mqtt out** node from the *network* section of the palette.

First we need to configure the broker we will use, select "Add new mqtt-broker config" and click the pencil icon to edit.
    * Under "Server" enter `demo.nexmodev.com` and put the same in the "Name" field.
    * Leave everything else as the defaults, and click the "Add" button.

![MQTT Broker Config](/MQTT_Broker_Config.png)

Connect the output of your *campusbadge* node to the *MQTT Out* node, and add an **Inject** node to the campusbadge input to kick off the flow.

![Badge Flow](/Badge_Flow.png)

Now click the button on the inject node and your badge should respond, don't forget to **Deploy** your config first.

## Further Steps

* Try integrating the MQTT event to your IVR to trigger the badge from a call.
* Can you control the color of the LEDs by SMS? eg Text `Green` or `Red`.
* Your badge can also publish events back through MQTT to NodeRED when the buttons are pressed, use an **MQTT In* node and subscribe to `/badge/[Your BadgeID]/events` to receive them.


