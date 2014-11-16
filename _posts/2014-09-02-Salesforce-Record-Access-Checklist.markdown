---
layout: post
title: "Salesforce Record Access Checklist"
category: Apex
comments: true
---

<img src="/images/checklist.jpg" class="noclip" alt="Winter 15" />

Ever since I read [The Checklist Manifesto](http://atulgawande.com/book/the-checklist-manifesto/) by Atul Gawande I have been creating checklists for some of the critical dev tasks.
I have created a checklist for Salesforce deployment and for writing unit tests. These checklists are not ready to be shared yet because
they need some tweaks and more real world use. Lately I have been working on several defects and issues around record access, particularly around
why a user cannot access or search for a particular sObject record in Salesforce.

This has resulted in a checklist (or a list of steps to follow) to investigate or debug user access. 
Before we jump into the checklist lets review the different elements of standard [Salesforce security](https://developer.salesforce.com/page/An_Overview_of_Force.com_Security).

* Profiles - Defines application permissions(layout, apps, tabs etc), object permissions(CRUD, field level security), and system permissions(manage users, modify all data etc).
* Org Wide Defaults (OWD) - Used to set default access to sObject records (database rows).
* Sharing Rules - Is used to open up access to sObject records to roles, groups, individual users etc. It cannot be more restrictive than OWD.
* Sharing Sets - Is used to give internal users (with a Salesforce license) access to records created by Communities/Portal users.
* Roles - provides access to records in a hierarchical fashion.
* Groups - Used to give record access to a group of individuals, roles, and other groups.
* Implicit Sharing - also called Built-in sharing gives users (Read only) access to parent account if you have access to its child record and vice versa. Also provides limited account and contact access to portal users.
* Record Ownership - If you own the record, then you can probably do CRUD action on the record.

Without futher ado, I present the `Salesforce record access checklist` to investigate why a user cannot read/view or search a particular record.

1. Check the **owner** of the record. If the user searching for the record is the owner of the record but can't find it using the Global search then 
go to Step 5. If the user trying to access the record is not the owner of the record, go to Step 2.
2. Check if the user's **Profile** has at least 'Read' permission for the sObject. If the user belongs to a Profile that does not have _any_ access
to the sObject then sharing rules and OWD don't apply (even if it is set to Public Read/Write). If the user does not have these Object permissions then you have found the source of the problem, else go to step 3.
3. Check the **OWD** for the sObject and the users **role hierarchy**
   a. If the OWD is Public Read/Write or Public Read Only go to step 5.
   b. If the OWD is Private, check to see if there are any sharing rules that open up record access. If the OWD is private and there are no sharing
  rules opening up access you have found the issue. 
   c. If there are sharing rules opening up access check if the user belongs to a group or role in the sharing rule. If the user is part of the role or group used in the sharing rule, 
   then go to Step 4. If the user is not part of the role or group used in the Sharing Rule, then you have found the issue.  
4. Check to see if the record being accessed is owned by a Communities/Portal user. If not then go to step 5, else check to see if there are **Sharing
Sets** defined to provide access to internal users. If there are no Sharing Sets defined and the owner of the record is a Communities/Portal user, then you have found the problem, otherwise go to Step 5.
5. This step is only applicable if the user can view the record but is not able to search for the record using Global Search. Check the users 
profile to see if the **Tab** is hidden for the sObject. If it is hidden, then the sObject record will not be displayed in the search results and you have
found the problem. Otherwise go to Step 6.
6. If you have reached this step and still haven't found the source of the problem, raise a ticket with Salesforce Support for further analysis.

**_Addendum_**: modified checklist if the user can read/view the record but cannot Update or Delete the record.

1. Check if the user's **Profile** has Update and/or Delete permission for the sObject. If the user belongs to a Profile that does not have these permissions 
to the sObject then sharing rules and OWD don't apply (even if it is set to Public Read/Write). If the user does not have these Object permissions then you have found the source of the problem, else go to step 2.
2. Check the **OWD** for the sObject and the users **role hierarchy**
   a. If the OWD is Private, check to see if there are any sharing rules that open up record access for read/write. If the OWD is private and there are no sharing
  rules opening up access you have found the issue. 
   b. If there are sharing rules opening up access for read/write check if the user belongs to a group or role in the sharing rule. If the user is part of the role or group used in the sharing rule, 
   then go to Step 3. If the user is not part of the role or group used in the Sharing Rule, then you have found the issue.  
3. Check to see if the record being accessed is owned by a Communities/Portal user. If not then go to step 4, else check to see if there are **Sharing
Sets** defined to provide access to internal users. If there are no Sharing Sets defined and the owner of the record is a Communities/Portal user, then you have found the problem, otherwise go to Step 4.
4. If you have reached this step and still haven't found the source of the problem, raise a ticket with Salesforce Support for further analysis.

If you have found an error or would like to improve the checklist please leave a comment or reach out to me on Twitter.

