---
published: false
layout: post
title: Glimpse ID - A Serverless Website Scanner
---
What do you do when you come across a strange link? Curiosity takes over and you **need** to see what it leads to. [urlscan.io](https://urlscan.io) is a great tool for this scenario that SOC analysts see every day. Submit a URL and it will return a screenshot of the website as well as information about the network traffic made and files loaded. The only problem is that it runs out of one server in Germany. I wanted to make something similar that was lightweight and able to be placed in multiple locations around the world.

[HTTP-Info](https://github.com/becksteadn/HTTP-Info) was my first attempt. It's a Docker container that uses Selenium and Browsermob Proxy to save a screenshot and record HTTP requests and responses. This was great, but a lot of API programming and container orchestration would be required in order to make it a viable app.

[Glimpse ID](https://scriptingis.life/glimpseid) uses AWS services to provide a modern, scalable API. In this blog post, each component will be explored in depth.

# Glimpse ID

## Computation (Lambda)

## Storage (S3 and Dynamo DB)

## Communication (API Gateway)

## Interaction (GitHub Pages)

## Conclusion

## Resources
