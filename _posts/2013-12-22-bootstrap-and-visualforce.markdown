---
layout: post
title: "Using Bootstrap with Visualforce"
category: HTML5 Visualforce Bootstrap
comments: true
---

If you spend your time building web apps then you might be familiar with [Bootstrap][bootstrap], a powerful front-end
toolkit for rapidly developing web applciations. It was orginally created by [Twitter][twitdev], and has since then been [open-sourced][bootgit]
and maintained by the core team along with community.

With its nifty collection of CSS and HTML conventions, it provides the developer with some stylish typography, forms, buttons, tables, 
grids, navigation etc. by using CSS reset along with other common layout features. Bootstrap 3 is responsive(layout graciously adapts to different 
screen sizes) by default with a mobile first approach.

I wanted to wield the power of this powerful framework on my Visualforce page so I began tinkering with it. It turns out that Visualforce and 
Bootstrap work very well together. I have posted a snapshot of the code and some screenshots below.

You can find the source code on my github page [here][ajgit].

The code below describes the main components to be included for the framework.
{% highlight html %}

<apex:page showHeader="false" sidebar="false" standardStylesheets="false" docType="html-5.0" language="en-US" applyHTMLTag="false">
  <html lang="en">
    <head>
      <meta charset="utf-8" />
      <title>Anup's Bootstrap example</title>
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <!-- Bootstrap -->
      <link href="{!URLFOR($Resource.Bootstrap_3_0_3, 'dist/css/bootstrap.min.css')}" rel="stylesheet" />

      <!-- HTML5 Shim and Respond.js IE8 support of HTML5 elements and media queries -->
      <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
      <!--[if lt IE 9]>
        <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js"></script>
        <script src="https://oss.maxcdn.com/libs/respond.js/1.3.0/respond.min.js"></script>
      <![endif]-->
    </head>
    <body>
      <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
      <script src="https://code.jquery.com/jquery.js"></script>
      <!-- Include all compiled plugins (below), or include individual files as needed -->
      <apex:includeScript value="{!URLFOR($Resource.Bootstrap_3_0_3, 'dist/js/bootstrap.min.js')}"></apex:includeScript>
    </body>
  </html>
</apex>
{% endhighlight %}


Please note that the 'applyHTMLTag' is set to _false_ because we are statically inserting our own html, head and body tags.
Also, the docType is set to 'html-5.0', so the page will have the HTML5 docType. I have tried this with other docTypes with
mixed results so I wouldn't recommend it.

Sample code to display the navigation bar.
{% highlight html %}

<body>
    <apex:outputPanel styleClass="navbar navbar-inverse navbar-fixed-top" layout="block" html-role="navigation">
      <apex:outputPanel styleClass="container" layout="block">
        <apex:outputPanel styleClass="navbar-header" layout="block">
          <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
            <apex:outputPanel styleClass="sr-only">Toggle navigation</apex:outputPanel>
            <apex:outputPanel styleClass="icon-bar"></apex:outputPanel>
            <apex:outputPanel styleClass="icon-bar"></apex:outputPanel>
            <apex:outputPanel styleClass="icon-bar"></apex:outputPanel>
          </button>
          <a class="navbar-brand" href="#">Salesforce1 and Bootstrap</a>
        </apex:outputPanel>
        <apex:outputPanel styleClass="navbar-collapse collapse" layout="block">
          <form class="navbar-form navbar-right" role="form">
            <apex:outputPanel styleClass="form-group" layout="block">
              <input type="text" placeholder="Email" class="form-control" />
            </apex:outputPanel>
            <apex:outputPanel styleClass="form-group" layout="block">
              <input type="password" placeholder="Password" class="form-control" />
            </apex:outputPanel>
            <button type="submit" class="btn btn-success">Sign in</button>
          </form>
        </apex:outputPanel><!--/.navbar-collapse -->
      </apex:outputPanel>
    </apex:outputPanel>
</body>

{% endhighlight %}

Some screenshots of the final Visualforce page with Bootstrap framework applied.

  <img src="/images/vfbootstrap.png" class="img-responsive img-rounded" width="100%" style="border:2px solid #fee;" alt="Bootstrap Example Desktop" />

  <img src="/images/vfbootstrapmobile.png" class="img-responsive img-rounded" alt="Bootstrap Example Mobile" />

[Source Code][source]

[bootstrap]: http://getbootstrap.com
[twitdev]: https://dev.twitter.com
[bootgit]: https://github.com/twbs
[ajgit]: https://gist.github.com/anupj/8089606
[source]: https://gist.github.com/anupj/8089606
