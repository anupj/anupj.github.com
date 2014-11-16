---
layout: post
title: "Grokking Force.com - Apex"
category: apex
comments: true
---

Over the past few years, as a [Force.com][force] developer, I have come across quite a few interesting features and gotchas 
on the Force.com platform that are either not documented, or is documented but developers don't know about it.
This post describes some of the quirks and some unknown facts of the Apex language that you might or might not have seen. 
Let's dive in!


**Map accepts `null` as a key**

According to the [Apex docs][Apex docs]:

    A map is a collection of key-value pairs where each unique key maps to a single 
    value. Keys can be any primitive data type, while values can be primitive, 
    sObject, collection type or an Apex object.

And right at the bottom of the page, it also notes:

    A map key can hold the null value

So the following code is a perfectly legitimate apex code:

{% highlight java %}
 Map<Id, String> testMap = new Map<Id, String>();
 testMap.put(null, 'foobar');

 System.debug('the value of the null key is: '+testMap.get(null)); 
 //outputs: The value of the null key is: foobar
{% endhighlight %}

Allowing `null` as a key to a map is a design flaw IMHO, therefore, it is a good idea to check for null before adding 
the key to a map variable. This information is also useful if you are wondering, why your map is returning null values?!?,
when it's not supposed to, especially, when you instatiate a map with a SOQL result where the values returned can be
null unless you explicity check for null values in the where clause. 

**Unit Tests increment your Objects unique Id**

This is not covered in the documentation, or atleast I haven't found it in the Apex docs. When you run unit 
tests that insert/create records for a particular object(standard or otherwise), it increments the value of
the Name field(of type Auto Number) outside of the test context.

Let's say you have a custom object "Invoice Statement" (Invoice_Statement__c) that has a Name field called
"Invoice Number", and is of type "Auto Number". The format of the field is: INV-{0000}, and let's assume that
the latest record number is INV-2058.

If you insert 10 records in the test class as follows:

{% highlight java %}
   // Let's assume that this runs inside of a test method
   List<Invoice_Statement__c> invStatementList = new List<Invoice_Statement__c>();
   for(Integer i = 0; i < 10; i++) {
     invStatementList.add(new Invoice_Statement__c(company='acme ltd');
   }
   insert invStatementList;
{% endhighlight %}

Now, when you insert a new Invoice Statement record outside of the test method via a trigger, or batch, or the User Interface, the
next inserted record will have the id `INV-2069`. So don't rely on the unique name field if you have strict rules
around the auto-increment feature in your application. Instead add a custom field which is auto-incremented in 
a workflow or trigger to have more granular control over how the value is incremented.

**Divide (/) operator gotcha**

What do you think should be the output of this statment?

{% highlight java %}
  system.debug('7 divided by 2 is: '+7/2);
{% endhighlight %}

If you think it should be `7 divided by 2 is: 3.5`, then you are wrong. The actual output is - `7 divided by 2 is: 3`. If the
operands of the divide(/) operator are both integers, the result is an integer.

If you want a decimal output then one of the operands must be a decimal, so the following code:

{% highlight java %}
  system.debug('7 divided by 2 is: '+7.0/2);
{% endhighlight %}

will output 3.5. I have spent many hours trying to figure out what's wrong with my code when I realized that x/y (x and
y being integers) wasn't returning an accurate decimal value. So make sure when you use the divide operator atleast one
of the operands is a decimal.

**Improve SOQL and SOSL performance by adding NULLability check**

This is actually highlighted in the Apex docs, but I have seen many experienced devs(including myself) not following this 
simple, but very effective suggestion to improve SOQL and SOSL query performance.

You can improve the performance of your code by filtering out null values in your SOQL and SOSL queries. 

{% highlight java %}
   List<String> invNumList = {'001', '002', '003'};
   List<Invoice_Statement__c> invStatList = [Select Id, invNumber 
                                            from Invoice_Statement__c where 
                                            invNumber in : invNumList 
                                            and invNumber != null]; 
{% endhighlight %}

That extra check for nullability improves the performance because it avoids searching records that contain null values.

I hope these tips are useful to some of you! I plan to write more of these in the future. Let me know if you've come across 
other interesting Apex code gotchas in the comments.

[Apex docs]: http://www.salesforce.com/us/developer/docs/apexcode/index.htm
[force]: http://developer.force.com
