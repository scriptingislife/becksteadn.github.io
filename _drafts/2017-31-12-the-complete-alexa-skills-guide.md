---
published: false
layout: post
title: The Full Guide to Making an Alexa Skill with Flask-Ask
---


I've written several skills using the Flask-Ask framework but found Amazon's basic tutorial to only work for short-term projects that would not be officially published. Recently, I wrote a post [here](/My-Alexa-Skills-Environment/)&nbsp;about the environment I created to be a fast and reliable host for my published skills, specifically [Explain Like I'm Five](https://www.amazon.com/Reddit-Explain-Like-Im-Unofficial/dp/B077ZQWYX3). That post explains the components and my reasons for using them. This is the technical guide to setting up that environment.

## The Host

I chose to develop the code using Amazon's Cloud9 service because&nbsp;