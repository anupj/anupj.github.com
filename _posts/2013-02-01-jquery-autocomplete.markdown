---
layout: post
title: "You Autocomplete Me - Visualforce+jQuery UI"
category: tutorial
comments: true
---

![Demo][1]

One of the most common functionalities of a typical web application or website is the ability to search for
things on the site. In the old days, it was sufficient to provide a simple text box where the user can
enter the key word or a phrase to search. The latest paradigm, however, is to add some kind of auto-suggest or 
auto-complete capability to the search text box that predicts one or more possible words or statements and displays in a list,
when the user writes the first letter or letters of the word in the box. If the intended word or statement is displayed as
one of the options in the list, then the user will select the word or statement. This is a very useful feature
that leads to less typing, and better directed search thereby improving the overall user experience.

As a Visualforce developer, you would definitely want to harness the awesomeness of this nifty feature to improve 
your websites user experience. You can also replace that uncool drop down list containing 500 items which is difficult if not
impossible to navigate, with a search box and a baked-in auto-suggest feature. This post will demonstrate how to implement the 
auto-complete (or auto-suggest) functionality using Visualforce and jQuery UI. I assume that you are comfortable with both Visualforce 
and jQuery. If you are still new to both of these technologies, I would recommend reading a bit about them before proceeding.

Let's say you are building a Visualforce page that allows users to search for their favourite movie and rent it (ala Netflix). You 
also want to help the user to search for the right movie, by automatically suggesting the movie name as they type it in the
search box. Create a visualforce page called `AutoComplete.page` and the corresponding controller `AutoCompleteController.cls`.

The first step is to add jquery.js, jquery-ui.js, and jquery-ui.css to the header.

{% highlight html %}
# AutoComplete.page
<apex:includeScript value="https://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js" />
<apex:includeScript value="https://ajax.googleapis.com/ajax/libs/jqueryui/1.10.0/jquery-ui.min.js" />
<apex:styleSheet value="http://ajax.googleapis.com/ajax/libs/jqueryui/1.10.0/themes/smoothness/jquery-ui.css" />
{% endhighlight %}

Then add an apex form component that contains the search text box.

{% highlight html %}
# AutoComplete.page
<apex:form id="autoCompleteForm" >
		
    <apex:pageBlock id="searchBlock" >
        <apex:pageBlockSection id="searchSection" title="Find me a movie to rent" columns="1" >
             <apex:outputLabel value="Movie Name" for="movieBox" />
             <apex:outputPanel >
                 <apex:inputText id="movieTextBoxId" value="{!searchTerm}" styleClass="placeHolder"/>
                 <apex:inputHidden id="searchMovieId" value="{!selectedMovie}" />
             </apex:outputPanel>
        </apex:pageBlockSection>
    </apex:pageBlock>
    
</apex:form>

{% endhighlight %}

The form component contains a pageBlockSection component inside a pageBlock component that consists of an outputPanel and outputText
component. There is also an inputHidden component inside the outputPanel component that will be used to store the movie name selected
from the list. Please note that all of the components have an id attribute.

Then add this script at the bottom of the page.

{% highlight html %}
# AutoComplete.page
<script type="text/javascript">
    var PLACEHOLDER = 'Enter Movie Name Here'; 
    var movieObjects;
    var queryTerm;
    
    $('[id$=movieTextBoxId]').autocomplete({
        minLength: 2,
        source: function(request, response) {
                    queryTerm = request.term;
                    AutoCompleteController.searchMovie(request.term, function(result, event){
                        if(event.type == 'exception') {
                              alert(event.message);
                        } else {
                             movieObjects = result;
                             response(movieObjects);
                        }
                    });
               },
        focus: function( event, ui ) {
                $('[id$=movieTextBoxId]').val( ui.item.Name );
                return false;
                },
        select: function( event, ui ) {
                    $('[id$=movieTextBoxId]').val( ui.item.Name );
                    $('[id$=searchMovieId]').val( ui.item.Id );
                    return false;
                },
     })
     .data( "autocomplete" )._renderItem = function( ul, item ) {
        var entry = "<a>" + item.Name;
       
        entry = entry + "</a>";
        entry = entry.replace(queryTerm, "<b>" + queryTerm + "</b>");
        return $( "<li></li>" )
            .data( "item.autocomplete", item )
            .append( entry )
            .appendTo( ul );
    };
        
    // Add or remove placeholder values
    $('[id$=movieTextBoxId]').val(PLACEHOLDER);
    $('[id$=movieTextBoxId]').on("focus",  function(event){
        $tgt = $(event.target);
        if($tgt.val() === PLACEHOLDER ){
            $tgt.val('');
            $tgt.removeClass('placeHolder');
        }
    });
    $('[id$=movieTextBoxId]').on( "blur",  function(event){
        $tgt = $(event.target);
        if($tgt.val() === '' ){
            $tgt.val(PLACEHOLDER);
            $tgt.addClass('placeHolder');
        }
    });
</script>
{% endhighlight %}

The magic happens in the autocomplete() method provided by jQuery-UI. You can find the complete documentation [here][autocomp].

The controller is very simple, and contains a single remote action method searchMovie() that retrieves the list of movie from the
database that is dynamically displayed in the dropdown list.

{% highlight java %}
// AutoCompleteController.cls
public with sharing class AutoCompleteController {
	
	// Instance fields
	public String searchTerm {get; set;}
	public String selectedMovie {get; set;}
	
	// JS Remoting action called when searching for a movie name
    @RemoteAction
    public static List<Movie__c> searchMovie(String searchTerm) {
        System.debug('Movie Name is: '+searchTerm );
        List<Movie__c> movies = Database.query('Select Id, Name from Movie__c where name like \'%' + String.escapeSingleQuotes(searchTerm) + '%\'');
        return movies;
    }
}

{% endhighlight %}

You would also have to add the following css for the correct look and feel.

{% highlight html %}
<style>
    .displayNone { 
        display:none; 
    }
    .displayBlock {
        display:block;
    }
    .ui-autocomplete-loading { 
        background: white url(/img/loading32.gif) right center no-repeat;
        background-size:15px 15px; 
    }
    .placeHolder {
        font-style: italic;
    }
</style>
{% endhighlight %}

Finally, a screenshot of autoComplete feature in action.

![Demo][2]

You can view the sample source code on my [github page][github].

[autocomp]:http://jqueryui.com/autocomplete/ 
[1]: http://i.imgur.com/SzEYfMr.gif
[2]: http://i.imgur.com/HpDLdCJ.png
[github]: https://github.com/anupj/Visualforce-jQuery
