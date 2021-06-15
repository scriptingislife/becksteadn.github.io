---
published: true
layout: post
title: Cloud Skills 101 - How To Learn AWS For Free
---

Cloud skills are hot right now, but unlike learning Python or running a Docker container, building in the cloud will cost you. Fortunately there are some secrets to maximizing the AWS free tier and getting credits to cover your monthly bill.

![So Hot Right Now]({{site.baseurl}}/images/Learn-Cloud-Free/SoHotRN.png)

As a college freshman, the [GitHub Student Developer Pack](http://education.github.com/pack) was a treasure trove of premium software development tools and platforms that I could access for free. One benefit was $100 in AWS credit every year.  It's no longer an item in the pack, but the credits are still available through [AWS Educate](https://awseducate.com).

At first I just used the credits for virtual machines that I could run 24/7. As I gained more knowledge about AWS I started exploring other services. Lambda and API Gateway quickly became my favorites. I used them to build [Glimpse ID](https://glimpseid.com), a sandbox for suspicious URLs.

## Pick Your Fighter

Certain services have more powerful free tiers. Think of power as the number of projects you can run while staying inside the usage limit. EC2 allows one free instance. That's good enough for a few continuously running programs. If you're looking to get the cloud experience, a single always-running EC2 instance is not it. We want to use the cool cloud technologies they talk about at conferences.

The services I use the most are Lambda, API Gateway, S3, and DynamoDB. These services are extremely cheap and the free tier has more than enough capacity for hobby projects. Here is my  free tier usage in May.

```
Lambda Compute: 10%
Lambda Requests: 1%
API Gateway: 0%
S3: 0%
DynamoDB: 0%
```

*Note, I am using API Gateway, S3, and DynamoDB. The limit is so high that I haven't even hit 1% of the free tier.*

I mentioned Glimpse ID before. Another example is [kudozo](https://github.com/becksteadn/kudozo), which you can see in action at the bottom of this post. They both take advantage of the same AWS services and will run indefinitely at no cost to me.

### Lambda

`1,000,000 requests`

`400,000 GB-seconds of compute time`

Lambda functions are the core of the application. While serverless functions will not replace all workloads, there are certainly a lot of things you can do with them. Most of my functions are snippets of code that are run on a regular basis. Lambda functions are best for small independent tasks like APIs, chat bots, and data processing. [Serverless Framework](https://www.serverless.com/examples/) has plenty of examples for all cloud platforms.

This free tier has two components —  requests and compute time. Requests are a count of how many times functions were invoked. Compute time considers how much memory is allocated and how long a function runs. To maximize the free tier, make sure to only provision the amount of memory necessary and optimize functions for speed. As shown above, Lambda compute time is the free tier I'm using the most of.  

### API Gateway

`$1.00 per 300,000,000 requests`

Alright I know what you're thinking. You didn't want to see any dollar signs in this post. API Gateway technically doesn't have an "Always Free" tier. The first 1,000,000 requests per month are free for the first 12 months. After the first year, the standard rate applies. But hear me out. Since standard rate is so cheap, you can make up to 3,000,000 requests before being charged a cent.

API Gateway will serve as the public interface for most of these applications. Making a GET or POST request will invoke the Lambda function and provide any input parameters. You can also choose to skip the Lambda function and configure API Gateway to make queries directly to DynamoDB.

### DynamoDB

`25 GB of storage`

`25 Write Capacity Units`

`25 Read Capacity Units`

`~200,000,000 Requests`

If a database is what you're after, DynamoDB may be for you. It's a NoSQL key-value database with near-unlimited capacity. Amazon's DynamoDB use peaked at [80.1 million requests per second](https://aws.amazon.com/blogs/aws/amazon-prime-day-2020-powered-by-aws/) on Prime Day in 2020. That's a little more than what is covered by the free tier.

DynamoDB pricing can get tricky. Like Lambda and S3, it is made up of two components. In this case they are capacity and data storage. Storage is simple. The first 25 GB stored are free. Billing for capacity is messy but the basics should be understandable.

As expected in the cloud, DynamoDB has the ability to scale without losing availability. It can be done on-demand as the requests come in or by using provisioned capacity. In provisioned capacity mode, you tell DynamoDB the number of reads and writes per second that you expect your application to require. The free tier uses provisioned capacity mode. It covers 25 read and write units which is enough for 200 million requests per month.

More information can be found on the [pricing page](https://aws.amazon.com/dynamodb/pricing/).

### Simple Storage Service

`$0.023 per GB`

`$0.005 per 1,000 PUT requests`

`$0.0004 per 1,000 GET requests`

Sorry, back to "not technically free" tiers. S3 pricing is made of the number of requests made and amount of data data stored. This service is a great example of how we can benefit from enterprise pricing. After 8,000 PUT requests, and using 1 GB of storage, my S3 bill was $0.06. That's a small price to pay for getting AWS experience. If the thought of paying AWS any money is repulsive, stick around for the section about Alexa to see how I don't actually pay that $0.06. 

S3 is simple, put bits in and take bits out. It can store files or any other data. It can also be configured to serve a static website. I use S3 as a general data store mostly holding JSON data or other files.

A Lambda function can be invoked when a new object is added to an S3 bucket. An example AWS likes to tout is resizing images when they're added to a bucket. It can also be used to clean or format data before putting into another bucket or service. 

## Put It All Together

A front end, a compute platform, and a storage service offer limitless possibilities for applications. It's been a wall of text up until this point so here's what a basic application could look like. There are other ways to build things in AWS but this is the cheapest by far.

![Example Architecture]({{site.baseurl}}/images/Learn-Cloud-Free/ServerlessAppExample.png)

One advantage of the cloud is that you only pay for what you use. When you build an application with these AWS services, it will always be there ready to go, but will not show up on your bill unless someone uses it. In contrast, if you rent a server from DigitalOcean to host a side project, you pay $5/month whether anyone uses it or not. 

I've only mentioned a few of my favorites. Two honorable mentions are SQS and SNS. The full list of Always Free Free Tiers can be found [at this link](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=tier%23always-free&awsf.Free%20Tier%20Categories=*all).

## Unlimited Power

There are some AWS services that are free regardless of usage. Some examples are IAM, Biliing, and Organizations. IAM is the most popular. While you can't build anything using only IAM, it is a critical part of cloud environments. Identity and Access Management makes up 20% of the AWS Specialty - Security certification exam. Create bucket policies. Assume roles. Make requests to the metadata service. IAM use is free and unlimited.

Billing may seem boring. The truth is when everything has a cost, optimizing architectures becomes crucial. Understanding how your programming decisions impact your bill is a valuable skill to have. Analyze your bill every few months and see if your applications are using the amount of resources you expected. 

## Alexa Is Your Angel Investor

Here's a hack I don't think many people know. You can get AWS credits for building a skill (app) for Alexa. So far I have received $4,000 in promotional credits this way. If the skill is hosted on AWS and costs pass the free tier, you will get $100 in AWS credit expiring at the end of the month. This will be the case every month until the program ends.

Lambda is the typical way to run Alexa skills on AWS. As mentioned earlier, using up the Lambda free tier is pretty darn difficult. The EC2 free tier on the other hand is just one small instance. I wrote a [basic post](https://scriptingis.life/My-Alexa-Skills-Environment/) and a [full guide](https://scriptingis.life/Alexa-Skills-Complete-Guide/) to how I run my skills on EC2. I'm sure it's a bit outdated, but you get the point. Combine Flask, DynamicDNS, and Let's Encrypt and you'll have the HTTPS endpoint Alexa requires.

Running a skill on EC2 will cost $5/month. That leaves $95 every month to play with.

## Wrap Up

It's been about 4 years now and I have yet to pay an AWS bill. I use $60-$80 in credits a month, mostly for EC2. Utilize the generous free tiers. Publish an Alexa skill to fund any projects that may cost money. I prefer building like this to predefined labs because it gives genuine hands-on experience. Try, fail, and learn.

## Resources

[Free Tier Summary](https://aws.amazon.com/free/)

[AWS Promotional Credits for Alexa](https://developer.amazon.com/en-US/alexa/alexa-skills-kit/new/aws-promotional-credits)

[AWS Docs - Using the AWS Free Tier](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html)

[Serverless Framework Examples](https://www.serverless.com/examples/)