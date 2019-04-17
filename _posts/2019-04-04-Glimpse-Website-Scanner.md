---
published: false
layout: post
title: Glimpse ID - A Website Scanner Built with Modern  Tools
---
What do you do when you come across a strange link? Curiosity takes over and you **need** to see what it leads to. [urlscan.io](https://urlscan.io) is a great tool for this scenario that SOC analysts see every day. Submit a URL and it will return a screenshot of the website as well as information about the network traffic made and files loaded. The only problem is that it runs out of one server in Germany. I wanted to make something similar that was lightweight and able to be placed in multiple locations around the world.

[HTTP-Info](https://github.com/becksteadn/HTTP-Info) was my first attempt. It's a Docker container that uses Selenium and Browsermob Proxy to save a screenshot and record HTTP requests and responses. This was great, but a lot of API programming and container orchestration would be required in order to make it a viable app.

[Glimpse ID](https://glimpseid.com) uses AWS services to provide a modern, scalable API. In this blog post, each component will be explored in depth.

# Glimpse ID

## Computation (Lambda)

This is the computing service that is at the center of Glimpse. Lambda is the serverless function platform for AWS. It's not _actually_ serverless, it just means the user does not need to manage the server that the application is run on. The code and all of its dependencies are packaged in a zip file and stored in an S3 bucket. A function can be invoked a number of ways. Glimpse uses an API Gateway endpoint, but the function can also run when a new entry has been made in S3 or DynamoDB, or simply during a time of day, like a cron job.

The Glimpse function is written in Python and uses [Selenium](https://www.seleniumhq.org/) for most functionality. It will launch Chrome in headless mode, render the supplied URL, take a screenshot, and collect some data about the site such as title and the final URL. The image is uploaded to a public S3 bucket and site data entered into DynamoDB.

## Storage (S3 and DynamoDB)



## Communication (API Gateway)

## Interaction (GitHub Pages)

## Conclusion

## Resources
