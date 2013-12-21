---
layout: post
title: "Create multiple sObjects with one create() call"
category: java
comments: true
---

I have been playing with the partner wsdl a lot lately. One of the things that I was trying to do was to create an Account,
Contact and Opportunity object with one create() call to avoid calling the api multiple times.

After some digging I came across the create() [api documentation][apidoc] that has a very good example of how you can create an Account and Opportunity using external id.
I wrote the following the code the inserts all three objects with one create() call. Hope this helps some intrepid developer who is trying to do something similar.

{% highlight java %}

    // Create the connection object
    ConnectorConfig connector = new ConnectorConfig();
    connector.setUsername(username);
    connector.setPassword(password);
    connector.setAuthEndpoint(this.endpoint);
    PartnerConnection sourceConnection = new PartnerConnection(connector);

    // Set the allOrNone header to true to make the call atomic
    __header.setAllOrNone(true);
    sourceConnection.__setAllOrNoneHeader(__header);

    // Create the SObjects array	
    SObject[] sObjects = new SObject[3];
		
    // Create Account sObject
		SObject parentAccount = new SObject();
		parentAccount.setType("Account"); 
		parentAccount.setField("Name", "TestAccount1");
		parentAccount.setField("AcctExtId__c", "ACCT EXTID 1");
		sObjects[0] = parentAccount;

    // parent account reference - set the ext id same as above
		SObject parentAccountRef = new SObject();
		parentAccountRef.setType("Account");
    parentAccountRef.setField("AcctExtId__c", "ACCT EXTID 1");

		// Create Contact sObject
		SObject childContact = new SObject();
		childContact.setType("Contact"); 
		childContact.setField("LastName", "TestContact1");
    childContact.setField("ContExtId__c", "CONT EXTID 1");
    // Assign the Account field to the account ref
		childContact.setField("Account", parentAccountRef);
		sObjects[1] = childContact;

		// Create Opportunity sObject
		SObject childOpp = new SObject();
		childOpp.setType("Opportunity");
    childOpp.setField("Name", "TestOpportunity1");
    Calendar dt = sourceConnection.getServerTimestamp().getTimestamp();
    dt.add(Calendar.DAY_OF_MONTH, 7);
    childOpp.setField("CloseDate", dt);
    childOpp.setField("StageName", "Prospecting");
    childOpp.setField("OppExtId__c", "OPP EXTID 1");
    childOpp.setField("Account", parentAccountRef);
		sObjects[2] = childOpp;

    String result;
    // call the create method
		SaveResult[] results = sourceConnection.create(sObjects);
    // log the output
		for (int j = 0; j < results.length; j++) {
      if (results[j].isSuccess()) {
          result = results[j].getId();
          logger.debug("sobject was created with an ID of: " + result);
       } else {
          // There were errors during the create call,
          // go through the errors array and write
          // them to the console
          for (int i = 0; i < results[j].getErrors().length; i++) {
              Error err = results[j].getErrors()[i];
              logger.debug("Errors were found on item " + j);
              logger.debug("Error code: " + err.getStatusCode().toString());
              logger.debug("Error message: " + err.getMessage());
          }
       }
    }
{% endhighlight %}

Some things to note:

* You can only insert objects one level deep with a single create() call. You can insert Account and Opportunity, but you cannot insert
Account, Opportunity, and OpportunityLineItem with one call. 
* This method relies on external ids but you are only allowed upto 3 external ids per object so design your objects carefully.


[apidoc]: http://www.salesforce.com/us/developer/docs/api/Content/sforce_api_calls_create.htm
