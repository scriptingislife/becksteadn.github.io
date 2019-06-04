---
title: Creating a URL Scanner with Selenium and Docker
date: 2018-08-28 00:00:00 Z
layout: post
---

I used [urlscan.io](https://urlscan.io) a lot this summer at my internship with a SOC. Every time I used it I wondered how it worked and if I could build something similar. The week after my internship ended I created [HTTP-Info](https://github.com/becksteadn/HTTP-Info) as my attempt to gather as much information about a website as I could.

### The Container

Creating a Docker container for this project enables the scanner to run anywhere. I get a lot of AWS credit and have an EC2 micro instance running in almost every region for my [Honeystash](https://scriptingis.life/Honeypot-Infrastructure/) project. I hope to write a web application to run the scanner and have an option to run the scan from different locations.

Selenium, Python, and Browsermob Proxy have a lot of dependencies and configuration so creating the scanner in a Docker container simplifies the setup. The base image I started with is joyzoursky's [python-chromedriver](https://hub.docker.com/r/joyzoursky/python-chromedriver/).

### Taking a Screenshot

I already had a snippet of Python code to get a screenshot using Selenium. The script can be found [here](https://github.com/becksteadn/Snippets/blob/master/screenshot.py). This was the file I started with for the HTTP-Info project.

I didn't have any kind of backend running to host screenshots and scan information yet so I decided to use the [puush.me](https://puush.me/) image upload service temporarily. Luckily, there is a `puush` module for Python that only requires an API key and filename and then returns the URL for the hosted image.

### Capture HTTP Requests

One of the most useful features of urlscan.io is the log of all HTTP requests that happen when visiting the URL in question. When I looked up how to get this information using Selenium, almost all answers mentioned Browsermob Proxy. Other proxies were mentioned but this seemed like the easiest to get up and running inside a container and get transaction data into Python.

#### Browsermob Proxy

This proxy is a small Java JAR file and was made with Selenium in mind. There is a Python module that runs the proxy server and collects the data. The Dockerfile downloads the zip file with the `.jar` and other code inside. The Python script starts the server, configures Selenium to use the proxy, and then shuts it down when the page has been loaded. 

To get Chrome to trust the proxy and to make sure all pages are loaded I added these lines to the Chrome options. This turns off all Chrome security options so that pages with self-signed certificates or a bad reputation on Google Safe Browsing are not blocked. 

```python
chrome_options.add_argument('--proxy-server={}'.format(proxy.proxy))
chrome_options.add_argument('--ignore-certificate-errors')
chrome_options.add_argument('--disable-web-security')
chrome_options.add_argument('--allow-running-insecure-content')
chrome_options.add_argument('--no-sandbox')
```

#### The HAR Data

The HTTP Archive is a JSON formatted log of web transactions. This data includes request URLs, cookies, response MIME type, etc. Currently, I just format the data into a clean looking output and discard it when the script ends. 

![Sample Formatted Output](https://puu.sh/Bm2bw/dad243043f.png)

### Conclusion

This is just the beginning of the project. I have dreams of a global infrastructure like my [Honeystash]({{site.baseurl}}/Honeypot-Infrastructure/) or [Log Mapper](https://github.com/becksteadn/Mapper-Server) projects. There is a lot to be done for backend storage and processing. As far as for the actual scanner, I want to record the hashes of each file returned from a GET request like URLScan does.
