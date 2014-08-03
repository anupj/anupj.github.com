---
layout: post
title: "Summer 14 Release Highlights"
category: SalesforceRelease
comments: true
---

Summer '14 release is here and it is full of awesome features. I have listed some of the features that I find interesting and useful.

###Salesforce1 - Access More Custom Objects
Previously you could only access custom objects in the Salesforce1 app that were included in the app but now you can view any custom object
that has a tab in the full Salesforce site. You can also see up to 200 of your list views for an object.

###Salesforce1 - Access Publisher Actions without Chatter Enabled
This is a subtle but very useful feature that allows Salesforce1 users to create and access Publisher and Publisher Actions even if your organisation has decided
to turn off Chatter. If chatter is disabled you can create global actions, actions list, object-specific actions and actions list.
Please note that this feature is only available on the Salesforce1 mobile browser app and not on the standalone Salesforce1 app.

###Salesforce1 - Drill down to reports and records from  Dashboards
Another convenient feature if you access your reports and dashboards on the go. You can now drill down to individual records and reports from Dashboards.
Reports can be viewed in tabular, summary, or Top N format and you can see a maximum of 100 records. 

###Force.com - Light or Enterprise Custom Objects
This is an interesting feature that you need to be aware of because it might have some license implication especially if you use Force.com Light App or
Force.com Enterprise App Licenses. A custom object is classified as Enterprise Application Object if they have these settings enabled - Allow Sharing, 
Allow Bulk API access, Allow Streaming API Access, otherwise it is a Light Application Object. Ofcourse, objects created before Summer '14 have these
settings enabled by default and are therefore classified as Enterprise Application Object.

###Force.com - New Rich Text Editor for HTML Area Home Page Components
The new version of the rich text editor in HTML Area home page components supports more markup but does NOT allow HTML to be manually entered. If your HTML
Area home page component contains Javascript, CSS, iframes or other unsupported markup will be supported and continue to work till Summer '15. It will
stop working in Summer '15 and later versions. Depending on your use case you can either deprecate the functionality or move it to the new Visualforce Area component instead.
Visualforce Area home page components can be added to the narrow or the wide column of the home page layout, andthe referenced Visualforce page 
can use a standard or custom controller

###Apex - Describe Limits Removed
This is great news, especially for ISV developers, but also for developers who use Dynamic Apex a lot. You are no longer limited to describing 100
sObjects or to executing 100 describe statements. You don't have to write weird workarounds for this limit anymore because all describe limits have been 
removed. 

###Apex - Submit more Batch Jobs with Apex Flex Queue (Pilot)
Another exciting feature that I can't wait to get my hands on is the new and shiny Apex Flex Queue. I find apex batch jobs extremely useful especially on Enterprise projects that deal
with large volumes of data. If you enable this pilot feature, you are now no longer limited to running up to 5 batch jobs simultaneously. The Apex Flex Queue pilot enables you to submit
batch jobs beyond the allowed limit of five queued or active jobs. If the submitted jobs aren't processed immediately by the system, they are put in
holding status and are placed in a seperate queue (the Apex Flex queue). You can have up to 100 batch jobs in the holding status. When system resources
are available the system will move the job from flex queue to the batch job queue by changing the status from 'Holding' to 'Queued'.
The jobs are processed first-in first-out by default, but Administrators also have the option to modify the order of jobs held in the Apex Flex queue.

These are some of my favourite features. If you have a favourite feature(s) list it/them in the comments. 
