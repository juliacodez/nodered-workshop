---
title: "Node-RED Workshop"
weight : 1
---
# Node-RED Workshop
Welcome to the NodeRED Workshop! 


## Getting Started with Node-RED

> Node-RED is a programming tool for wiring together hardware devices, APIs and online services in new and interesting ways.  
It provides a browser-based editor that makes it easy to wire together flows using the wide range of nodes in the palette that can be deployed to its runtime in a single-click.

For this workshop, we'll be using a desktop version of Node-RED called [Workbench](https://github.com/sammachin/workbench/releases).  Download the latest version at [https://github.com/sammachin/workbench/releases](https://github.com/sammachin/workbench/releases) and install it on your machine.

When you first open up the application, you'll be prompted to log in. Use the default credentials `admin`/`password` to do so, then set your own under **Settings**.

![Workbench Login Page](/Workbench_Login.png)

Select _Workbench -> Settings_ from the top menu bar, set your new _Username_ and _Password_, then hit *Save*. You'll be using these credentials going forward.

![Workbench Set Credentials](/Workbench_Set_Credentials.png)

Alternatively, you’ll need to install the Node-RED runtime and editor. This could be done either on your local machine, on a Single Board Computer (eg Raspberry Pi), or a number of cloud-hosted options. If you prefer to go this route, follow the getting started instructions on [nodered.org](https://nodered.org/docs/getting-started/).   
This is where you'll also find the [documentation](https://nodered.org/docs/), [example flows](https://flows.nodered.org/) and more.

## ngrok

[ngrok](https://ngrok.com/product) is a tunneling service that we've decided to build into Workbench. It exposes your local server to the internet over secure tunnels—it allows the Vonage APIs to communicate with your application.  

It works out of the box, navigate to _ngrok_ in the top menu bar to start, stop and inspect your tunnel. (1)  
Once your tunnel is running, you can copy the URL to Clipboard by clicking on it---this will come in handy later on when working with webhooks. (2)

![Ngrok Menu](/Ngrok_Menu.png)

This tunnel will stay open for 8 hours without an ngrok account, after that you'll have to restart it, and you'll get a different randomly generated URL.  

To avoid this, sign up for an [ngrok account](https://dashboard.ngrok.com/signup), then fill in your credentials in the _ngrok_ section of the _Workbench Settings_ page. 
{{% notice tip %}}
 Even though it will still be randomly generated, a free account will make your URL persist.
{{% /notice  %}}

🎉 You are now ready to dive in!



