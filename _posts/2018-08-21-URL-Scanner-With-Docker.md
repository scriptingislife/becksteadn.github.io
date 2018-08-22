---
published: false
---
---
published: false
layout: post
title: Creating a URL Scanner with Selenium and Docker
---
## Creating a URL Scanner with Selenium and Docker

I used [urlscan.io](https://urlscan.io) a lot this summer at my internship with a SOC. Every time I used it I wondered how it worked and if I could build something similar. The week after my internship ended I created [HTTP-Info](https://github.com/becksteadn/HTTP-Info) as my attempt to gather as much information about a website as I could.

### The Container

Creating a Docker container for this project enables the scanner to run anywhere. I get a lot of AWS credit and have an EC2 micro instance running in almost every region for my [Honeystash](https://scriptingis.life/Honeypot-Infrastructure/) project. I hope to write a web application to run the scanner and have an option to run the scan from different locations.

Selenium, Python, and Browsermob Proxy have a lot of dependencies and setup so creating the scanner in a Docker container simplifies the setup. 

### Taking a Screenshot

I already had a snippet of Python code to get a screenshot using Selenium. The script can be found [here](https://github.com/becksteadn/Snippets/blob/master/screenshot.py). This was the file I started with for the HTTP-Info project. 

### Capture HTTP Requests

One of the most useful features of urlscan.io is the log of all HTTP requests that happen when visiting the URL in question. When I looked up how to get this information using Selenium, almost all answers mentioned Browsermob Proxy.

#### Browsermob Proxy

This proxy is a small java executable and was made with Selenium in mind. There is a Python module that runs the proxy server and collects the data.

#### The HAR Data

The HTTP Archive is a JSON formatted log of web transactions. This data includes request URLs, cookies, response data type, etc. 