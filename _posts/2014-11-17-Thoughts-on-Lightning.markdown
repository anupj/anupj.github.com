---
layout: post
title: "Thoughts on Salesforce1 Lightning"
category: Deployment
comments: true
---
<img src="/images/LightningLogo.png"  alt="Lightning Logo" />

At [Dreamforce](http://www.salesforce.com/dreamforce/DF14/) this year, the weather outside Moscone centre was bright and sunny, but inside the centre the attendees were struck by Lightning.
I am sure most of you have heard of Lightning by now. Some of you eager beavers might have even started hammering out Lightning components and apps already to
show off your 'early adopter' badge and I look forward to seeing your components. For those of you who are curious or mildly interested in what 
might be as revolutionary announcement as Apex and Visualforce was back in 2008, keep reading.

Lightning is an umbrella marketing term for a number of different technologies:

  * Lightning Framework and components
  * Lightning App Builder
  * Lightning Connect
  * Lightning Schema Builder
  * Ligtening Process Builder



##Lightning Framework and Components

<img src="/images/LightningLayer.jpg"  alt="Lightning Layer" />

Salesforce has been working on a component based UI framework internally called [Aura Framework](https://github.com/forcedotcom/aura) which they open sourced in 2013. Aura comes
with a rich and extensible component set that is optimized for different devices. It is comparable to frameworks/libraries like Polymer (by Google), React
(by Facebook). Comparable, but not similar since Aura is a framework for building custom components whereas Polymer and React are libraries
for building Web/view Components. You can think of Lightning Framework being a port of the open source Aura Framework. Using the Lightning Framework you can build Lightning
 Components - a self-contained and reusable unit of an application that can range in granularity from a single line of text to an entire app.
 With its stateful client and stateless client architecture (more on that later) it is ideal for use with the Salesforce1 mobile app. Infact, the new
 Salesforce1 mobile app itself was built using Lightning Components.

 One of the main differences between Lightning components and something like Visualforce component is **performance**. Good old Visualforce performance
 suffers from viewstate hangover, and is not ideal for building apps for Salesforce1 mobile. LC solves this problem by using a stateful client
 and stateless server architecture that relies on Javascript on the client side to manage UI component metadata and application data.
Client and server exchange data in JSON format and is much faster than vf page and components that manages some aspects of the data using
viewstate.

Also components follow the event driven programming paradigm (similar to Java Swing and javascript in browser) where you write handlers that
respond to interface events as they occur. When a user interacts with the interface, the javascript controller actions will fire either a component event
(handled by the component itself) or an Application event (handled by components that subscribe to these events).

In your typical force.com application you have 2 main components - the visualforce page (handles the M & V in MVC), and the Apex controller.
A Lightning component is comprised of one or more of the following:

  1. Javascript controller
  2. Javascript helper
  3. CSS styles
  4. Component or Application  (this is the only required resource in the bundle)
  5. Server side Apex controller (similar to VF controller)
  6. Documentation

Note that the framework is still Beta and is not GA yet so if you want to play with it you need to spin a new DE org and use the Developer
Console to write Lightning apps. Also, force.com canvas apps will stop working when you enable LC, enables Namespace prefix in your org, and it is only available to build
Salesforce1 apps at the moment.
The purpose of this article is to familiarise you with the high level structure. Head over to the Lightning components [documentation](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/) to grok more
details.

##Lightning App Builder

<img src="/images/LightningAppBuilder.png"  alt="Lightning App Builder" />

LAB is a new UI tool that will allow system administrators to create apps, add components to existing apps and/or standard/custom pages
using out of the box (standard) components provided by SF and custom components built by developers using a drag and drop interface.
They will be able to achieve this without writing a single line of code. LAB is still in development and might change in look and feel.

##Lightning Process Builder

<img src="/images/LightningProcessBuilder.png"  alt="Lightning Process Builder" />

LPB is a UI tool for visualizing and build automated business processes. If workflow rules is C, then LPB is C++. It will replace
workflow rules and approval processes. LPB has additional actions like - create chatter posts with @mentions, create sObject records without
apex, update child/related records. This should let you get rid of some apex code.
It will NOT replace Visual Flow because they are not the same thing.
If you want to play with it is available in beta with the Winter '15 release.

##Lightning Connect (aka Platform Connect aka External Data Sources)
Lightning Connect enables your users to view data stored in external systems on-demand(via RESTful queries) as External Objects.
The data should either be in oData format or imported using the Apex Connector Framework and (obviously) must be accessible via the internet.
It's important to note that
by using Lightning Connect you are not using your (limited and expensive) data storage but requesting and using data on demand using
API queries. Therefore there are limits to queries per hour and request and response size amongst other limits.

<img src="/images/LightningConnectLimits.png"  alt="Lightning Connect Limits" />

External Objects are represented as sObjectName__x (instead of __c) and is Read-only. Once your data is exposed as an oData source, you
can setup External Object(s) declaratively. Checkout this presentation about External Data Sources for more information.

A number of integration partners like Informatica Cloud, Jitterbit, Mule etc have also announced connectors to translate data to oData format
to support Connect integration. This should replace some of the existing integration implementation solution.

##Lightning Schema Builder

Nothing new here, just a rebrand of the Schema builder that we know and love. 


This overview is just a brief description of all that is now possible with Lightning. With Lightning you can now build really awesome mobile and
desktop apps lightning fast thereby reducing the gap between an idea and implementation.

