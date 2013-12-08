---
layout: post
title: "Convert Salesforce id in Java"
category: java
comments: true
---

I am working on a project which requires integrating with Force.com via the SOAP wsdl. One of the things that we want to 
ensure is that we always work with 18 character ids. The SOAP api always returns 18 character ids, but the input to the 
java app could be a csv file which may contain 15 character ids. 

I created the following utility method in Java that converts 15 character id to 18 character id. The logic for the code was derived
from this [Salesforce StackExchange post][post].

{% highlight java %}
  // Converts 15 char salesforce id to 18 char id
  static String convertto18CharId(String original15charId){
  	
  	if(original15charId == null || original15charId.isEmpty()) return null;
  	original15charId = original15charId.trim();
  	if(original15charId.length() != 15) return null;
  	
  	final String BASE = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456";
  	StringBuilder result = new StringBuilder();
  	
  	try {
    	for(int i = 0; i < 3; i++){
    		StringBuilder tempString = new StringBuilder(original15charId.substring(i*5, i*5+5));
    		tempString.reverse();
    		String binary = "";
    		for(char ch: tempString.toString().toCharArray()) {
    			binary += Character.isUpperCase(ch) ? '1' : '0';
    		}
    		result.append(BASE.charAt(Integer.parseInt(binary,2)));	
    	}
  	} catch(Exception ex) {
  		  ex.printStackTrace();
  	}
  	
  	if(result.length() == 0) return null;
  	
  	return original15charId + result.toString();
  	
  }
{% endhighlight %}



[post]: http://salesforce.stackexchange.com/questions/1653/what-are-salesforce-ids-composed-of
