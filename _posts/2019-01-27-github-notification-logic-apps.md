---
layout: post
title: "Little Hacks: GitHub notifications with Logic Apps"
date: 2019-01-27 01:00
categories: hacks
---

Being the maintainer of a [small Typescript library][react-appinsights],
I need to be notified of new issues that are opened in its GitHub repository. 
Unfortunately, for some reason GitHub does not send any email notification for this
kind of events. So I decided to use this as an opportunity to experiment with 
[Azure Logic Apps][azure-logic-apps], the If-This-Than-That version of Azure.  

It was very easy to come up with a working logic app that sends an email
upon creation of a new issue. First, I copied the example json payload
shown in the [GitHub Webhooks documentation][github-webhook] in an HTTP 
request trigger of the Logic App.
Next, I linked the trigger step to a conditional on a dynamic field of the 
Webhook HTTP request. If this condition is true, the app sends me an email,
using the Outlook integration.
The final step was copying the Logic App trigger URL in the Webhook settings
of the repository in question.  
Here is a screenshot of my app as shown by the Logic Apps designer:

![Azure Logic Apps designer GitHub notification](/assets/github-azure-logic-apps-notification.png)

It was extremely easy and even fun, so I will consider using Logic Apps in 
the future if I'll need to automate other tasks.


 [react-appinsights]: https://github.com/Azure/react-appinsights
 [azure-logic-apps]: https://azure.microsoft.com/en-us/services/logic-apps/
 [github-webhook]: https://developer.github.com/webhooks/
