---
title: "Setting Up"
weight : 05
---
{{% notice note %}}

{{% /notice %}}

## Get Your Vonage Credentials

The first thing you need to do is to setup your Vonage Credentials. Take note of your API Key and Secret from your [dashboard](https://dashboard.nexmo.com), as you'll be using these for authentication.

![Vonage Dashboard](/Vonage_Dashboard.png)

Vonage uses different authentication for its various APIs. For this workshop we will first set up Basic Auth, which is used for SMS and Verify, and get you going with sending your first SMS messages.

We will then move on to create a [Voice application](https://dashboard.nexmo.com/applications) which will provide the auth for the Voice API.