---
published: false
layout: post
title: The Full Guide to Making an Alexa Skill with Flask-Ask
---


I've written several skills using the Flask-Ask framework but found [Amazon's basic tutorial](https://developer.amazon.com/blogs/post/Tx14R0IYYGH3SKT/Flask-Ask-A-New-Python-Framework-for-Rapid-Alexa-Skills-Kit-Development) to only work for short-term projects that would not be officially published. Recently, I wrote a post [here](/My-Alexa-Skills-Environment/)&nbsp;about the environment I created to be a fast and reliable host for my published skills, specifically [Explain Like I'm Five](https://www.amazon.com/Reddit-Explain-Like-Im-Unofficial/dp/B077ZQWYX3). That post explains the components and my reasons for using them. This is the technical guide to setting up that environment.

## The Host

I chose to develop the code using Amazon's Cloud9 service because it integrates with AWS and gives the ability to code in a browser. A new EC2 instance can be created when initializing a Cloud9 environment.

#### Create a New Cloud9 Environment

After clicking&nbsp;**Create Environment&nbsp;**give it a name and description. If you have a server you want to develop and/or host on, specify the user and host then add their public key to the authorized\_keys file. Otherwise, create a new EC2 Instance. The default t2.micro type should be fine for only hosting several skills. The cost-saving setting is useful during development, but is not necessary with the large amount of credit Amazon offers. Review your settings and start the instance.

![](/uploads/versions/environment-details---x----749-762x---.png)

#### Add Ports to Security Group

By default, only port 22 (SSH) is open. Alexa skills operate over HTTPS on port 443 so we need to add some rules to the EC2 security group![Alexa Skills EC2 Host Security Groups](/uploads/versions/security-groups---x----1659-240x---.png)

The rules for port 80 are optional and just a precaution since Caddy redirects to HTTPS anyway. Ports used by Flask servers do not need to be opened because they will be behind the reverse proxy.

#### Connect To Your Environment

Go to the Cloud9 panel and the new environment will be visible. Select&nbsp;**Open IDE&nbsp;**to connect.

![](/uploads/versions/cloud9-panel---x----1546-503x---.png)

The welcome screen offers a few main configuration options like themes and keyboard mode as well as several ways to get started like cloning a git repository or creating a new file.

### The Code

We'll be creating the Memory Game skill used in Amazon's tutorial. It's a simple skill that covers all of basic aspects a programmer may need to understand.

#### Install Dependencies

To run a Flask server, Python needs to be installed. If Python or pip is not installed, you can run the following commands.

&nbsp;