---
layout:            post
title:             "Simple SMTP Server with Python"
date:              2016-11-01 15:28:00 +0300
tags:              Simple SMTP Server+Python
category:          Tutorial
author:            Poonam Agrawal
---
You really want to design a simple mail server in python then here is the way you can do that.

This post gives the idea, architecture and implementation of a simple SMTP server in python, where a flask api is used to initialise and start the server, client can use the apis to play around with different type of requests, and send the mail to the server started by api using any of the way to send email(here a small python script client.py to send mail).



## Explaination Of the Architecture

<div>

<figure>
<img src="{{ site.github.url }}/media/img/Architecture_Docker.png" />
<img src="{{ site.github.url }}/media/img/Architecture_VM.png" />
<figcaption>Architecture</figcaption>
</figure>

</div>


#### We have a Flask App which gives api to do the following tasks:

- Initialise Log location.
- Create DB.
- Start-Server.
- Get emails received by a particular id.
- Get emails received after a particular time.

#### We have SMTP server implemented using python smtp library which does the following:

- Receives email from clients.
- Stores emails in database.

#### DataBase store, we are using sqlite db store:

- Stores the email received by Smtp server

#### Client:

- Uses Flask Apis to perform the requests.
- Send email to the server.



## Architecture Of SMTP server

<div>

<figure>
<img src="{{ site.github.url }}/media/img/Architecture_smtp.png" />
<figcaption>Architecture</figcaption>
</figure>

</div>

You can find the code in the github location: <a href="https://github.com/agrawalpoonam/smtpapp">
https://github.com/agrawalpoonam/smtpapp
</a>

