---
title: "Voice Calls"
weight : 25
---

Voice calls are more complex than text messages, there are far more possibiliites to control the call, Nexmo has creted an intsruction set known as Nexmo Call Control Objects (NCCOs) that allow you to tell the platform how to handle a call. An NCCO is either served up to Nexmo in response to a webhook for an inccoming call or is passed with the request to make a new call.

### Incomming Calls
To start with we will create a simple incomming call handler. We can leave the SMS nodes as they are and create a new tab on the editor using the + button 

First of all we need to receive the incomming call voice webhook, we will use the `voice webhook` node from the input section for this, this node acts very much like the HTTP node we used for SMS with a few extras for working with Nexmo Voice Webhooks.

Configure the Voice Webhook as a GET with a url of `/answer` which matches with the value we set when creating the application.

Now we can start to build the NCCO that we will return to the incomming call, an NCCO is a series of actions, each action is availble as a darker green node in the Nexmo section of the pallette, for our first call we will use a talk to read some text  followed by a stream to play a sound file, you can connect as many actions together as you need to for your call, and you can have multiple actions of the same type eg `talk>stream>talk` However there are certain requirements that apply to some actions, consult the nexmo developer documentation if you want to know more about that.

Drag the talk action onto the pallet and open up the properties, the 2 fields we are going to set are the `Text` and the `Voice Name` you will notice the `{}` against the text field as this supports templating, For the text we will greet the caller and then read them the caller ID of the number they are calling from, we can access the callerID as a property from the voice webhook called `msg.call.from`. So in our text field lets put: `Hello, thanks for calling from {{msg.call.from}}`
We can also set the voice name, Nexmo supports many different named voices to allow text to speech in multiple languages, the default voice is `Kimberly` which is a US English Female voice, we will change our node to use `Emma`

Next we will create a `stream` node, the `stream` node takes a URL to a WAV or MP3 file hosted on the internet somewhere, (you could host this within NodeRED for example) and plays that to the caller. Drag your stream node onto your canvas and open up the properties, the only thing we need to set is the URL to the wav file, for this test you can enter `https://s3.sammachin.com/fanfare.wav`

Finally we need to close off our webhook in order to return the NCCO we have built up, in the output section you will find a `return ncco` node, add this to the end of your flow.

Make sure that you have wired up your nodes as shown in the diagram then click deploy.

![Basic Voice Call](/Basic_Voice.png)

If you now call your nexmo number you should hear your message.

Now try re arraning the nodes so that the `Stream` is played befoe the `Talk` don't forget to click deploy after you make a change.


### Call Events
The Nexmo platform also sends events related to voice calls to a separate Webhook endpoint, we can setup Node-RED to receive and log those events,

We use a simple HTTP Input node, connected to an HTTP Output node as Nexmo requires a response, then we will log the output to the debug window, 
You HTTP Input node should be configured to accept a POST on `/event` as was setup with the application.
See the picture below for what this should now look like:

![Voice Event Webhook](/Voice_Events.png)

If you now make a call to your Nexmo Number you will see a number of events logged into the debug window, this is also how Nexmo sends error messages about your application to you.


### Outgoing Calls

We can also tell nexmo to place a call to a number and when the call is answered execute an NCCO in much the same way, there are 2 methods you can either supply an AnswerURL with the request to create the call much like an incomming call or you can build the NCCO and pass that directly in the create call request. We will use tha latter as its a bit simpler.

First of all we need _something_ to act as a trigger for the flow, this could be an external service, a webhook, the Node-RED dashboard or even an incomming SMS, initially we will just use the `inject` node this allows us to start a flow by clickin on the button next to the node.


Now we will build our NCCO nodes in the same way as we did for an incomming call, however we won't have values like `msg.call.from` to use for templating in this scenario.

Once you have your inject node and your NCCO actions eg a `talk` use the `createCall` node from the Nexmo section of the pallette, open up the Properties of the node and set the following:
`Nexmo Credentials` - should be the application you created at setup
`Endpoint` - Select `Phone`
`Number` - Enter your mobile number in international format without the + for example `(212) 555-1211` becomes `12125551212`
Leave `dtmfAnswer` empty
`From` should be your Nexmo number again in international format
`Answer` select `msg.ncco` to use the NCCO built in the preceding nodes.
Leave the other fields as default and click `done`

Finally we will add a debug node to the end of the flow so that we can see the response sent back from Nexmo when we create the call.


Your flow should look something like this:

![Create Outbound Call](/Create_Call.png)

