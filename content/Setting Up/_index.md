---
title: "Setting Up"
weight : 5
---

## Setting Up

You will find on a separate sheet on your desk the URL, username and password for a node red instance hosted on our shared server. You should be able to login to this address and be presented with the NodeRED Editor.

The first thing you need to do is to setup your Nexmo Credentials, you should have a note of your API Key and Secret to hand.

Nexmo uses different authentication for certain APIs for this workshop we will setup Basic Auth whcih is used for SMS and Verify and then create a Nexmo Voice application which will provide the auth for the Voice API.

### Basic Auth
To setup basic auth from your pallet drag a 'Send SMS' node onto the canvas, double click the node to open the properties, and then click on the pencil icon where it says `Add new nexmobasic`

![SMS Node Config](/SMS_Node_Auth.png)

In the window that opens enter your API Key and API Secret, Then Click `Add`

![Basic Auth](/Basic_Auth.png)

When you are returned to the SMS node properties you can then click delete, this will mean that you have a basic auth config that will then appear when you use the relevent nodes.

### Voice Applicaton Auth
Next we will create a nexmo voice application from within Node-RED, the voice appilcation contains a bundle of config used for both routing incomming calls to your Node-RED instance and also the authenication credentials to make outbound calls and other requests to the voice API.

This time we will select a `CreateCall` node from the pallette, and open up its properties, click the pencil icon next to the `Add new nexmovoiceapp` menu.

![Create Call Node Config](/Create_Call_Config.png)

We need to enter a few more details this time:
Name - This is what the application will be called in your Nexmo Dashboard, we reccomend something like "NodeRED - [Name of your Instance]"
API Key & Secret  - Same as before
AnswerURL - This is the URl that will be called for incomming calls to numbers linked to the applicaiton, you need to enter the web address of your NodeRED instance followed by /answer eg https://monkey.workbench.red/answer
EventURL  - LIke the AnswerURL this is where call events are sent to, we will use `/event` for this, eg https://monkey.workbench.red/event

Now click on `Create New Applicaition`, after a few seconds the 2 grey boxes will be filled with an APP ID and Private Key, this means the application has been created on the nexmo plaform. Click the `Add` button to save this config. You now have a Nexmo Voice Application created on your account and the credentials stored within NodeRED.

![Voice Applicaiton Auth](/Voice_Auth.png)

### Phone Number Setup

Next we will purchase a Number and link that to our Applicaiton and a Webhook for SMS, for this you will need to login to the nexmo dashboard at https://dashboard.nexmo.com

From the right hand menu select `Numbers` then `Buy Numbers`

In the Dropdown lists select your country (United States), Then under Features select `SMS & Voice`  and Type `Mobile`, Then click `Search`

![Buy a Number](/Buy_Number.png)

Choose a number and click the `Buy` button. After a few seconds you will see a popup confirming the number has been assigned to your account.

Nexmo from the right hand menu select `Your Numbers` You should then see the number you purcahsed listed. Click on the cog icon under `Manage`
![Your Numbers](/Your_Numbers.png)

Now we will be presented with a box to setup where incomming calls & SMS to this number should be sent.

For SMS we configure the Webhook URL directly, the Nexmo platform will make an HTTP request to this address for each SMS, enter the hostname of your instance followed by `/sms` eg `https://monkey.workbench.red/sms`

For Voice we need to select the Applkicaiton we created earlier, in the dropdown you should see the name that you chose when creating the application.
 
![Configure Number](/Config_Number.png)


### Congratulations, 
Your Node-RED instance is now configured and we can build your first workflows

