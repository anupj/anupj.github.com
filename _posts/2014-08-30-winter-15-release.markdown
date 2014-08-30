---
layout: post
title: "Winter '15 Release Highlights"
category: SalesforceRelease
comments: true
---

Salesforce Winter '15 Release is upon us and I have cherrypicked some of the items
that I find interesting and highlighted them below. Let me know in the comments which new feature has got you all excited.

###SalesforceA - App Enhanced with new features
Looks like you can now access system status site from the SalesforceA app. Previously you had to go to trust.salesforce.com in your browser. This 
is a minor but very convenient improvement that will make you use the SalesforceA app even more. 
You can (finally) switch between multiple accounts in different organisations without having to log out and log back in. This is a great improvement
in user experience and will makes the app more usable (a boon for consultants like me who work on multiple projects).
Anoter UI improvement is to show the user task buttons as action icons that are available from the action bar at the bottom of the page. This aligns
it with the Salesforce1 app design. More details can be found 
[here](http://releasenotes.docs.salesforce.com/en-us/winter15/release-notes/rn_mobile_salesforceA.htm).

###Communities - Community Designer
If you have worked with Communities and wanted to build a community site using point and click you had to use Site.com. It looks like it has now
been replaced (only for Communities) with [Community Designer](http://releasenotes.docs.salesforce.com/en-us/winter15/release-notes/rn_networks_comm_designer.htm) that lets you create, brand and publish a custom community site. You can also
configure single sign-on access portals using templates. You get to choose from four templates and then style the pages to match your company's 
branding. If you skip the templates you can use Site.com instead.

###Force.com - Advanced Setup Search
Setup search is one of the most time saving feature in the full Salesforce site. I don't have to trawl through bazillion options under setup anymore 
to find the correct option. Nowadays, I just search for the setup item that I am looking for using Setup search, so much so, that I don't even remember the 
setup structure anymore. Winter '15 now lets you search more items like static resources (yay!), queues, workflow rules and tasks (finally!) and
many more. You can find the full list [here](http://releasenotes.docs.salesforce.com/en-us/winter15/release-notes/rn_forcecom_setup_search.htm).

###Force.com - Deploy with Active Jobs
Another time (and life) saver feature is the new option added to the Deployment Settings that lets you to deploy components (apex class, VF page etc.)
referenced by Active apex jobs that are pending or in progress. Not being able to save code (in sandbox) or to see production deployments fail because there
were 'jobs pending or in progress' was a great source of irritation to me. With this option, I don't have to cancel the jobs anymore (yay!). Great news 
indeed for salesforce developers and release managers everywhere!

###Force.com - Serve static resources from VF Domain
When this update is enabled in your org, all static resources will be loaded from the Visualforce Domain instead of the Salesforce Domain. If you 
are not following the best practice of using the $Resource global variable and the URLFOR() function to reference static resources then this change
to the orgin domain can cause absolute references to static resources to break. Something to bear in mind.

###Force.com - Queueable Interface
This is a new interface that you can implement to run jobs asynchronously and is (arguably) a better way to run apex code asynchronously compared to the @future annotation. 
You can add jobs using this interface to the queue
and monitor them. They are similar to future methods but give you additional benefits like getting an id for your job that you can use to monitor your
job, and you can also chain one job to another by starting a second job from the running job. 
Check out the [release notes](http://releasenotes.docs.salesforce.com/en-us/winter15/release-notes/rn_apex_queueing_jobs.htm) 
for more details and code sample. 

###Force.com - Run more Future Methods and Callouts
You can now run 50 future methods in a single Apex invocation and the maximum number of callouts in transaction has now been increased to 100.

These were some of my favourite features in this release. 
You can find the release notes [here](http://releasenotes.docs.salesforce.com/en-us/winter15/release-notes/rn_included_release_notes.htm).

