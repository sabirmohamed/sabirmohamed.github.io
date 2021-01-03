[TOC]

# Azure DevOps Service Connection Automation with Azure CLI



### Introduction

Lately encountered some challanges on Automating the creation of a Servince Connection [Endpoint] in Azure DevOps

From Customers Microsoft has learned that Azure CLI is better than DevOps Rest API as it handles the authentication module better, 
since the CLI is a easy absraction model on top of the REST API, 
it is little easier interacting with the CLI over the REST API"

In Terms of Donavan Browns VSTeams, it is more of a preference module like like people 
who require powershell need to commandiong then 
VSTeams woild be a better choice, 
and terms of a proper CLI it might a better couice to go with the Azure CLI.

So lets Dive into see how we can achive this with Azure CLI and when you complete the guide you will be able to do the creation of a Service EndPoint

## Prerequisites

Before you begin this guide you'll need the following:

Azure Subcription
Azure DevOps Organization and a Project
Azure App Registration - Service Pricipal - App ID and Secret



- [number of servers] <OS and OS Version> server(s) <!-- Also specify the amount of RAM the server needs if relevant. -->
- A non-root user with sudo privileges (<insert link to Initial Server Setup article for the OS used in this tutorial>) explains how to set this up.)
- (Optional) If software such as Nginx needs to be installed, link to the proper article describing how to install it.
- (Optional) If the reader needs a fully-qualified domain name (FQDN), mention it here as well.
- (Optional) List any other accounts needed, such as GitHub, Facebook,  or other services.

<!-- Example:
* One Ubuntu 18.04 server with at least 1GB of RAM set up by following [the Ubuntu 18.04 initial server setup guide](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04), including a sudo non-root user and a firewall.
* Nginx installed on your server, as shown in [How To Install Nginx on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04).
* A domain name configured to point to your server. You can learn how to point domains to DigitalOcean Droplets by following the [How To Set Up a Host Name with DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean) tutorial.
-->

## Step 1 — Doing Something

<!-- For more information on steps, see https://do.co/style/#steps -->

Introduction to the step. What are we going to do and why are we doing it?

First....

Next...

Finally...

<!-- When showing a command, explain the command first by talking about what it does. Then show the command. Then show its output in a separate output block: -->

Display the status of your firewall with the following command:

```nginx
az devops security group list --project "Sabir Dev" --output table
```

You'll see the following output:

```
[secondary_label Output]
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
<^>Nginx HTTP                 ALLOW       Anywhere<^>
OpenSSH (v6)               ALLOW       Anywhere (v6)             
<^>Nginx HTTP (v6)            ALLOW       Anywhere (v6)<^>
```

<!-- You can use highlighting in output to point out relevant details -->


<!-- When asking the reader to open a file, specify the file and show the command to open it: -->

Open the file `/etc/nginx.conf` in your editor:

```command
sudo nano /etc/nginx.com
```

<!-- When showing a configuration file, try to show only the relevant parts and explain what needs to change.  -->

Change the `root` field so it points to `/var/www/your_domain`:

```nginx
az devops security group create\
 --project "Sabir Dev" \
 --scope project \
 --name "Service Connection Hashnodeplantir" \
 --description "Manage Service Connection Credentials" \
 --query "{displayName:displayName,descriptor:descriptor}" \
 --output table
```

<!-- See do.co/style#commands-and-code-in-steps for more on how to incorporate command and code in your steps. -->

Now transition to the next step by telling the reader what's next.

## Step 2 — Title Case

Another introduction

Your content

Transition to the next step.

## Step 3 — Title Case

Another introduction

Your content

Transition to the next step.

## Conclusion

In this article you [configured/set up/built/deployed] [something]. Now you can....

<!-- Speak  to reader benefits of this technique or procedure and optionally provide places for further exploration. -->


<!------------ Formatting ------------------------->

<!-- Some examples of how to mark up various things

This is _italics_ and this is **bold**.

Only use italics and bold for specific things. Learn more at https://do.co/style#bold-and-italics

This is `inline code`. Use it for referencing package names and commands.

Here's a command someone types in the Terminal:

```command
sudo nano /etc/nginx/sites-available/default
```

Here's a configuration file. 

The label on the first line lets you clearly state the file that's being shown or modified:

```nginx
[label /etc/nginx/sites-available/default]
server {
    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;

    root <^>/usr/share/nginx/html<^>;
    index index.html index.htm;

    server_name localhost;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Here's output from a command:

```
[secondary_label Output]
Could not connect to Redis at 127.0.0.1:6379: Connection refused
```

Learn about formatting commands and terminal output at https://do.co/style#code

Key presses should be written in ALLCAPS with in-line code formatting: `ENTER`.

Use a plus symbol (+) if keys need to be pressed simultaneously: `CTRL+C`.

This is a <^>variable<^>.

This is an `<^>in-line code variable<^>`

Learn more about how to use variables to highlight important items at https://do.co/style#variables

Use `<^>your_server_ip<^>` when referencing the IP of the server.  Use `111.111.111.111` and `222.222.222.222` if you need other IP addresses in examples.

Learn more about host names and domains at https://do.co/style#users-hostnames-and-domains

<$>[note]
**Note:** This is a note.
<$>

<$>[warning]
**Warning:** This is a warning.
<$>

Learn more about notes at https://do.co/style#notes-and-warnings

Screenshots should be in PNG format and hosted on imgur. Embed them in the article using the following format:

![Alt text for screen readers](/path/to/img.png)

Learn more about images at https://do.co/style#images-and-other-assets
-->
