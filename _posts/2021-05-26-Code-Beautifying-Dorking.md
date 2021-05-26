---
published: true
layout: post
title: Information Disclosure via a Code Beautifying Site
---

The internet is full of awesome free tools. I use [CyberChef](https://gchq.github.io/CyberChef/) almost every day. Another example is [Have I Been Pwned](https://haveibeenpwned.com). These are great services and they do a lot of good. The tool we'll focus on is [codebeautify.org](https://codebeautify.org) which among other things, beautifies code.

I've used sites like this extensively to beautify JSON. Throw in a mess of brackets and quotes and get out perfectly formatted data. Where is it transformed though? Is it sent to a server? Is it stored on the server? Free tools are great but it's important to know what they're doing.

A lot of the things I write about now are a result of [interesting things](https://scriptingis.life/Sinkholing-Botnet/) I've seen while handling alerts as a security analyst. This is one of those things. While researching a malicious file, a Google result was for [codebeautify.org](https://codebeautify.org). It contained some text that wasn't really relevant. What stuck with me was that this person had beautified some code and somehow it was indexed by Google.

## What You Came Here For

Obviously the next step was to see what other uploads could be searched. Passwords?

![Search for Passwords]({{site.baseurl}}/images/Code-Beautifying-Dorking/password.png)

Now `password` is a common word and yields too many uninteresting results. That was kind of expected. What about `apikey`?

![Search for apikey]({{site.baseurl}}/images/Code-Beautifying-Dorking/apikey.png)

That's more like it. 71 results isn't much, but it's honest work. Other keywords with results are `accesskey`, `accesstoken` and `token "username"`. The results contain credentials for cloud databases, paid APIs, and in one case, an Azure Active Directory instance.

![Lots of Credentials]({{site.baseurl}}/images/Code-Beautifying-Dorking/credentials.png)

There is more information than just credentials. It could be a good starting point to find cloud resources or API endpoints.

## Why?

At first I thought every upload was given a link. It turns out there's a `Save & Share` button. Although a link is generated, most people probably don't expect it to be viewed by anyone with access to a search engine.

## Wrap Up

This is nothing groundbreaking. Google dorking has been around for a long time. Searching `site:pastebin.com apikey` yields 1,190 results rather than just 71. Code Beautify is the top result for beautifying though and could yield valuable information that was supposed to be kept private. Add it to your toolbox and see what's out there.