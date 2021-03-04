---
published: true
layout: post
title: Automate Gmail with App Scripts
---

The inbox - overflowing with promotional offers, updates that interest no one, and other cries for attention. Just like mailboxes the inbox is overflowing with junk mail. Labels, filters, and countless third-party tools attempt to categorize incoming mail and bring important messages to the front.

Gmail's own categories feature can sort emails into different tabs. The "cries for attention" are sorted into the Promotions tab. I rarely want to look there.

![Gmail Settings]({{site.baseurl}}/images/AppScripts-Promotions/settings.png)

Unfortunately, filters cannot automatically mark all promotions as read. Enter Google App Scripts, a platform by Google for extending and automating Gsuite. App Scripts provides libraries and APIs for Google Services and runs the functions in a serverless architecture.

## Create a Project

Make a new project. Immediately you're thrown into the IDE. Change the name and get ready to code.

## Write the Code

This code will mark every unread email in the Promotions tab as read. The function `GmailApp.search` acts just like the search bar in Gmail and will return the same results.

```jsx
function removePromotions() {
  var threads = GmailApp.search('category:promotions is:unread');
  for (var i = 0; i < threads.length; i++) {  
    threads[i].markRead(); 
  }
}
```

## Execute the Function

Test the code by clicking the Run button at the top. Emails in the Promotions tab will be marked as read.

![Test Run]({{site.baseurl}}/images/AppScripts-Promotions/run.png)

## Create a Trigger

In order to routinely run the code, make a Time-driven trigger. It can run as frequently as every 5 minutes.

![Trigger]({{site.baseurl}}/images/AppScripts-Promotions/trigger.png)

## Sit Back and Relax

Any mail categorized as a promotion will now automatically be marked as read. This is a simple function with just four lines of code. Scripts can get way more complex and tie several services together. There's a lot to learn here. [Follow me](https://twitter.com/scriptingislife) for more imminent App Scripts projects.

## References

[Google App Scripts](https://script.google.com/home)

[Google Service Limits](https://developers.google.com/apps-script/guides/services/quotas)

[GitHub - App Script Samples](https://github.com/googleworkspace/apps-script-samples)