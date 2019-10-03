---
title: "Sending and Receiving SMS"
weight : 15
---

## Handling an Incoming Webhook

SMS messages sent to your Nexmo number are delivered to your webhook as a HTTP GET requests (you can change to POST in the dashboard), To handle this we are going to create a simple NodeRED Flow.

1. From the "input" section of the palette drag an "http" node onto your canvas. Double click to open the properties, then make sure that the Method is set to `GET`.

2. In the URL box enter `/sms`; this corresponds to the address we configured in the dashboard for your phone number earlier.

    > HTTP Input nodes need a coresponding output node, otherwise the HTTP request won't receive a response. For SMS Webhooks Nexmo doesn't particuarly care about the content of the response so long as it returns a success code.

3. Connect an "http response" node from the "output" section of the palette to your original "http" input node.

4. Add a "debug" node from the "output" section and connect it to the "http" input. This enables us to inspect the contents of the webhook, such as the SMS message and the sender details. Your flow should look like this:

    ![Inbound SMS](/SMS_Webhook.png)

5. **Click the "Deploy" button** in the top left to make this flow live. The "deploy" button makes your canvas changes take effect so you'll need to do this often as you work through these flows.

6. Now send an SMS to your Nexmo number from your phone, in a few seconds you should see the details of the message in the debug window on the right. You may need to click the Bug icon to display this.

## Sending an SMS in Response

SMS doesn't really have the concept of replies, however we can send a new SMS in response to an incoming message, using the sender details.

1. From the "nexmo" section of the pallett drag a "Send SMS" node onto your canvas and connect the http input node to the Send SMS one.

2. Open the properties for the Send SMS node, select your API key as the Nexmo Credentials.

3. You will notice that next to the "To", "From" and "Text" fields there are `{ }` symbols, this is to indicate that these fields support templating. You can either enter static vaules in these fields or use dynamic vaules from previous messages or other parts of your NodeRED flow.

4. In the previous section you viewed the details of the incoming SMS in your debug window, this contained the `msg.payload` object with its own properties. We will use these to setup our response message:
    - "msisdn" was the number that sent the message to your application
    - "To" is your Nexmo number
    - "Text" contains the contents of the message.

5. In the "To" field for Send SMS enter `{{msg.payload.msisdn}}` so that we use the sender of the original message as our recipient in the response.

6. In the "From" field enter `{{msg.payload.to}}` so that the response appears to come from the same number that the first message was sent from.

7. In "Text" enter `You said {{msg.payload.text}}`. You will see here that we are using a combination of our own static text and the contents of the original message for our response, this is the concept of templating.

    ![Response SMS](/Response_SMS.png)

8. Click the `Done` button Then click `Deploy`.

9. If you now send another message to your Nexmo number you should get back a message in response.

Thats the basics of SMS messaging with Nexmo and NodeRED

