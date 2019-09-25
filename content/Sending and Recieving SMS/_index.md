---
title: "Sending and Receiving SMS"
weight : 15
---


### Handling an incomming webhook

SMS sent to your nexmo number is delivered to your webhook as an HTTP GET request (you can change to POST in the dashboard), To handle this we are going to create a simple NodeRED Flow.

From the `input` section of the pallett drag an `http` node onto your canvas, double tap to open and make sure that the Method is set to `GET` in the URL box enter `/sms` this corresponds to the address we setup in the phone number earlier.

HTTP Input nodes need a coresponding output node, otheriwise the HTTP request won't receive a response, for SMS Webhooks Nexmo doesn't particuarly care about whats int he response so long as it returns a success code. So we will simply connect an `HTTP Response` node from the `Output` section of the pallett.

We also want to see the contents of the webhook, eg our SMS message and sender details etc. We will use a `debug` node from the `Output` section for this to simply log the information within our editor.

Your flow should look like this:

![Inbound SMS](/SMS_Webhook.png)

Click the `Deploy` button in the top left to make this flow live, now send an SMS to your Nexmo number from your phone, in a few seconds you should see the details of the message in the debug window on the right. You may need to click the Bug icon to display this.


### Sending an SMS in response.

SMS doesn't really have the concept of replies, however we can send a new SMS to the sender  triggered by the incomming one.

From the `nexmo` section of the pallett drag a `Send SMS` node onto your canvas and connect the http input node to the Send SMS one. 

Open the properties for the Send SMS node, select your API key as the Nexmo Credentials.

You will notice that nex to the To, From and Text fields there are {} symbols, this is to indicated that these fields support templating. You can either enter static vaules in these fields or use dynamic vaules from previous messages or other parts of your NodeRED flow. 
In the previous section you viewed the details of the incomming SMS in your debug window, this contained the `msg.payload` object with its own properties, `msisdn` was the number that sent the message to your application, `to` is your Nexmo number and `text` contains the contents of the message. We will use these to setup our response message:
In the `To` field for Send SMS enter `{{msg.payload.msisdn}}` so that we use the sender of the original message as our recipient in the response.
in the `From` field enter `{{msg.payload.to}}` so that the response appears to come from the same number that the first message was sent from.
In `Text` enter `You said {{msg.payload.text}}` you will see here that we are using a combination of our own static text and the contents of the original message for our response, this is the concept of templating.

![Response SMS](/Response_SMS.png)
Click the `Done` button Then click `Deploy`

If you now send another message to your Nexmo number you should get back a message in response.

Thats the basics of SMS messaging with Nexmo and Node-RED



