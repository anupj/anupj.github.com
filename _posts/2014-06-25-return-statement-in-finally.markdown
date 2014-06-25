---
layout: post
title: "To return or not to return"
category: Apex
comments: true
---

Apex allows you to handle Exceptions using the `try-catch-finally` block. The documentation states that the `Finally` block always gets executed.
The documentation also says that when the compiler encounters the `Return` statement it will instantly return the control back to the calling function/block.
But when you use (or in this case misuse) the two constructs together you see some interesting and surprising results.

Consider the following code below. Pretend to be a compiler for a moment and predict the outcome in the final System.debug() statement.

{% highlight java %}
  public class FinallyReturnTest {
    // Function returns from the finally block
    public String returnAString() {
      String returnVar;
      try {
        returnVar = null;
        returnVar.length(); // Will throw null pointer exception
      } catch(Exception ex) {
        // the exception will be caught
        System.debug('Exception caught is: '+ex);
        returnVar = 'I am set in the Catch block.';
        return returnVar;
      } finally {
        returnVar = 'I am set in the Finally block.';
        return returnVar;
      }
      returnVar = 'I am set in the main function';
      return returnVar; // Execution will never reach this statement
    }  
  }

  // Instantiate the class and call the method
  // from anonymous apex 
  FinallyReturnTest frtInstance = new FinallyReturnTest();
  System.debug('The return string is: ' + frtInstance.returnAString());
  
{% endhighlight %}

The odd thing about this code is _not_ that it has more `Return` statements than Homer Simpson has hair on his head.
If you were expecting the outcome to be `The return string is: I am set in the Catch block.`, then you are in for a surprise.

The actual outcome is - `The return string is: I am set in the Finally block.`. Yep, they were not kidding when they said that
the `Finally` block will always get executed. Even overriding the nature of the `Return` statement in the `Catch` block.

I would argue that it is generally a bad practice to return from a `Catch` or `Finally` block and you shouldn't do it. But if you
do decide to return from these blocks then be aware of this idiosyncracy. Since apex is "modelled" (and I use the term modelled very loosely here)
on Java, I decided to test this in Java and _voila_ I could replicate this in Java as well.

Before you return to your daily routine, here is some food for thought. If I modify the code slightly by commenting out the return 
in the `Finally` block then the outcome is as expected - `The return string is: I am set in the Catch block.`. That is it now returns from
the `Catch` block. Go figure!


