---
title: "Building an IVR"
weight : 35
---

An IVR or Interactive Voice Response is a menu of options presented to a caller, they then navigate that meny using the buttons on their keypad to send DTMF (Dual Tone Multi Frequency) signals.
Each option on the IVR can direct the call to a different path, for example forwarding the call to a specific destination, playing a recorded piece of information or even triggering another service such as an SMS.
IVRs are the fundimental navigation method of Voice Call appliactions.
An IVR can have multiple levels to it, where selection of one option presents the user with more options, this can go on to an infinite depth! For this activity we will just create a single level IVR.


## Initial Call Handling

We first need to handle the incomming call, like before we will start with a *Voice Webhook* listening on `/answer`, that will then return an NCCO made up of a *talk* action that tells the user the options followed an *Input* action to collect the users choice. We've also kept the */event* webhook from our previous flow so we can see the events and errors.
Your Flow should look like this:

![IVR Inital Call Handler](/IVR_1.png)

Next we need to configure the nodes, firstly the talk node: We need to greet the caller and then offer them our options, something like "Hello please press 1 or 2" will suffice for now, we can go back and edit the greeting once we have all our options.
Choose a voice name or just leave this as the default.
This time we are also going to tick the *Barge In* option, what this does is it allows the caller to send their input before the text has finished being read, its very useful in an IVR an prevents poeple from having to listen to a long list of options.
![Talk Options](/Talk_Options.png)

Next we configure the _input_ node, Once the user has entered the required number of digits those will be sent as a new webhook, so we need to configre that path.
In the URL field we enter the full address of our NodeRED instance followed by /input1 (eg https://monkey.workbench.red/input1) we use /input1 because if we were to later create a second level to our IVR we would need to send that input to a different address, eg input2.
We will set the method to GET, We can leave Submit on Hash unchecked this would cause the input to be sent by the user pressing the # key eg for collecting something like an account number.
Again we will leave the timeout as the deafult, but we will set _Max Digits_ to 1, this means that we only need the user to press a single key for the inout to be sent, which means we can have a maximum of 9 options in our menu.
![Input Options](/Input_Options.png)

## Handling the Input

Next we need to handle that input that will be sent to our application from Nexmo, to do this we will setup another voice webhook  listening for a GET on `/input1` which corresponds to our setting in the *input* node above.

The first thing we will do with this flow is to check what number the caller pressed and then branch our flow accordingly, we can do this using one of Node-RED's built in nodes, the *Switch Node* You'll find switch in the function section of the palette.
We need to then configure the switch node, the first thing we will do is set whcih property we want to switch on, in this case the digit(s) that the user pressed are send in `msg.call.dtmf`.
We then need to setup routes for each option, the first one is pre-set so if the propery equals (==) then we take that path, put a 1 in that box, then click the +add button at the bottom to add addional options, we will setup handlers for 1, and 2, in addition  we will setup a route to handle the call if the user doesn't press any option within the timeout. In this scenario Nexmo will send the webhook with the _timedout_ param set to true and `msg.call.dtmf` will be an empty string. Therefore we can simple setup our route to be *is empty*
![Switch Node Config](/Switch_Config.png) 

## Call Paths

You will notice that your *Switch* Node now has 2 output links, each one of these represents a different option that you configured, you can hover over them with the mouse to check what each one is.
Firstly we will deal with the *is empty* path, the simplest thing to do here is to re-present the user with the menu options, so we can simply connect this back to our initial talk node to read the options and then collect input. 

Next we need to do something if the caller presses 1, for this we will connect the caller to another number. We can do this with the *connect* action from the pallette, We only need to set 2 things in this node to connect the call to another phone number. The Endpoint should be *Phone* the *Number* should be another phone you might have (ask the person next to you if you can use their cell phone number) and the *From* should be your Nexmo number.

![Connect Node Config](/Connect_Node_Config.png)

For the 2nd option in out IVR we are going to send the caller a link via text message, first of all we will play them a short message to tell them the sms is on the way. So connect a *talk* node to the final output of our *switch* and configure this to say "I have sent you the link by SMS". Next we are also going to use the Send SMS node, we can wire this up in paralel to the talk node so that the SMS is sent as soon as the user has pressed their input. 

However we need to do some additional work, the */input* webhook won't get the caller ID of the person that is calling by default, so we need to go back to our input node and configure the URl to send that along with the dtmf chouce.
In the input node edit the url from */input1* to be `/input1?from={{msg.call.from}}` this will then send the vaule of *from* as a new value also called *from* to our */input1* handler.

Now we can add our *Send SMS* node, select your nexmo credentials, set the `To` as `{{msg.call.from}}` and the _From_ as your Nexmo number. In the text field you can enter a URL of your choice eg https://vonage.com/campus

We need to make sure that the 2 paths from the Switch Node are both closed off with a `Return NCCO` node, you can use separate ones or they can both share the same node.
Finally we will add a debug node to the end of the Send SMS to log the response from Nexmo.

Your Flow should look like this:

![IVR Complete](/IVR_Complete.png)

Go ahead and call your nexmo number, then try the different options.


## Expanding your IVR

Here are some suggestions for additional things to add to your IVR:

* Try adding a 2nd layer to the menu
* Add an option to connect the caller into a conversation node, this will give you a conference call
* Try recording a message from the caller
* If you are feeling very adventourous setup the Email node to email you when someone chooses that option.

There will be people around to help you with these.










