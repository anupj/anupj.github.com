---
layout: post
title: "Salesforce ID Set Gotcha"
category: Apex
comments: true
---

This is another one of those gotcha posts about the quirks of the apex language that newbies need to be aware of.
Lets consider the following scenario. You have an sObject where you store an id for another sObject as a String. 
For e.g. lets take an sObject called Invoice__c which has a text field (length - 18) called serviceId__c which stores
a reference to a related Service__c object. The reason you are using a text field instead of an ID field 
is because you have reached your limit of adding master-detail or lookup relationships to the Invoice sObject. 

Also, lets say when you create the Invoices you populate it with the Service Id like this.

{% highlight java %}

Invoice__c i = new Invoice__c(Name = '0001', Description__c = 'Invoice 1', serviceID__c = serviceObj.Id);
insert i;

{% endhighlight %}

Then later in the code you do a query to retrieve Service sObject records and use 
the Service record Ids to retrieve the corresponding Invoice records.

{% highlight java %}
  public class SalesforceIDTest {
    public String isIt18CharacterID() {
      List<Service__c> serviceList = [SELECT Id FROM Service__c WHERE Type__c = 'Professional'];
      Set<ID> serviceIDSet = new Set<ID>();
      
      // Add the service id to the serviceIDSet
      // Most importantly if you debug the service id now it will be 15 character id
      for (Service__c s : serviceList) {
        System.debug('The service id is: '+s.Id);
        serviceIDSet.add(s.Id);
      }

      // Now query the Invoice records using the service Id
      // This won't return any records
      List<Invoice__c> invoices = [SELECT ID from Invoice__c WHERE serviceId__c in :serviceIDSet];
      System.debug(invoices.size()); // output is 0
    }  
  }

{% endhighlight %}

Why did the query to retrieve the invoice records did not return any results? Can you guess?

The problem is introduced in **line 36** when you add the service record id to the serviceIDSet.
Apex will automatically convert the _15 character id_ to the equivalent _18  character version_ when you
add it to the Set. This is not an issue if you use the set as a bind variable for an ID field, but 
you need to be aware of this if you are using the ID set bind variable with a text field (as shown above).

The solution is to use a Set<String\> instead of a Set<ID\>. As a general rule if you are using a bind variable
in a soql, try and use the same datatype as the field used in the where clause to avoid issues.
