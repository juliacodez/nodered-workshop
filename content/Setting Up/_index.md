---
title: "Setting Up"
weight : 5
---

Claim one of the printed sheets from the tables containing the URL, username and password for a node red instance hosted on our shared server (one per person). When you log in you will be presented with the NodeRED Editor - this is for you to use during this workshop.

The first thing you need to do is to setup your Nexmo Credentials, you should have a note of your API Key and Secret to hand or you can find these on the [dashboard](https://dashboard.nexmo.com).

Nexmo uses different authentication for our various APIs. For this workshop we will first set up Basic Auth which is used for SMS and Verify, then also create a Nexmo Voice application which will provide the auth for the Voice API.

## Basic Auth

1. To set up basic auth drag a 'Send SMS' node from the palette on the left onto the canvas, double click the node to open the properties, and then click on the pencil icon where it says `Add new nexmobasic`

    ![SMS Node Config](/SMS_Node_Auth.png)

2. In the window that opens enter your API Key and API Secret, Then Click `Add`

    ![Basic Auth](/Basic_Auth.png)

3. We don't actually need the SMS node so you can delete it now by going into the node properties and choosing delete.

## Voice Application Auth

Next we will create a Nexmo voice application from within NodeRED. The voice application contains a bundle of config for routing incoming calls to your NodeRED instance and also holds the authentication credentials to make outbound calls and other voice API calls.

1. To configure auth for Voice API, we will select a `CreateCall` node from the palette, and open up its properties, click the pencil icon next to the `Add new nexmovoiceapp` menu.

    ![Create Call Node Config](/Create_Call_Config.png)

2. We need to enter a few more details this time:
 * Name - This is what the application will be called in your Nexmo Dashboard, we recommend something like "NodeRED - [Name of your Instance]"
 * API Key and Secret - Same as before
 * AnswerURL - This is the URL that will be called for incoming calls to numbers linked to the application, you need to enter the web address of your NodeRED instance followed by `/answer` e.g. https://monkey.workbench.red/answer
 * EventURL  - Like the AnswerURL this is where call events are sent to, we will use `/event` for this, eg https://monkey.workbench.red/event

3. Now click on `Create New Application`, after a few seconds the 2 grey boxes will be filled with an APP ID and Private Key, this means the application has been created on the Nexmo plaform. Click the `Done` button to save this config. You now have a Nexmo Voice Application created on your account and the credentials stored within NodeRED.

    ![Voice Application Auth](/Voice_Auth.png)

## Phone Number Setup

Next we will purchase a number and link that to our new voice application and configure a webhook for SMS.

1. First, log in to your Nexmo account at https://dashboard.nexmo.com

2. From the right hand menu select `Numbers` then `Buy Numbers`

3. In the Dropdown lists select your country (United States), Then under Features select `SMS & Voice`  and Type `Mobile`, Then click `Search`

    ![Buy a Number](/Buy_Number.png)

4. Choose a number and click the `Buy` button. After a few seconds you will see a popup confirming the number has been assigned to your account.

5. From the right hand menu select `Your Numbers` You should then see the number you purchased listed. Click on the cog icon under `Manage`

    ![Your Numbers](/Your_Numbers.png)

6. Now we will be presented with an interface to configure where incoming calls and SMSes to this number should be sent.

7. For SMS we configure the Webhook URL directly, the Nexmo platform will make an HTTP request to this address for each SMS, enter the hostname of your instance followed by `/sms` eg https://monkey.workbench.red/sms

8. For Voice we need to select the Application we created earlier, in the dropdown you should see the name that you chose when creating the application.
 
    ![Configure Number](/Config_Number.png)


## Congratulations! 

Your NodeRED instance is now configured and we can build your first workflows.

