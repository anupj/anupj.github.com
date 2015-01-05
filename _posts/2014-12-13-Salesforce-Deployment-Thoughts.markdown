---
layout: post
title: "Thoughts on Automating Salesforce Deployment"
category: Deployment
comments: true
---

<img src="/images/deploy-button.jpg" height="300px" width="300px" alt="Deploy button" />


Deployment is one of the most important parts of any software project. It is very hard to justify the success of your project being “delivered on time”, if your deployment fails (or takes longer than what is acceptable to the business) and requires a rollback. Depending on the size of the project, the deployment could be a 2 min activity by one person or a 2 day endeavour for an army of resources. An organisation attempting to reach the Expert (Level 5) maturity level in Continuous Delivery are always looking at options to automate their deployment process. They want an automated process that checks out code from the master branch of their Version Control System(VCS), runs all tests, validates the deployment and then deploys it to the production environment (ideally) without human intervention. This is the holy grail of deployments that will lead to Continuous Delivery nirvana.

  1. Manual migration - You must manually repeat the exact modifications in every development or production organization. This is obviously tedious and time-consuming. Only recommended for really small changes and for components that cannot be deployed by Metadata API or Change Sets.
  2. Eclipse IDE plugin - You can deploy components (that can be deployed using Metadata API) from your IDE using the Force.com plugin. Not recommended for large deployments because you will be locked out of your IDE which also sometimes has a tendency to crash for long running operations. You will have to diligently track the components being deployed seperately since the IDE does not track this information for you.
  3. Ant Migration Tool - It is a command-line utility for moving metadata between a local directory and a Salesforce organisation and uses the Metadata API. Recommended for multi-stage release process, and/or if you prefer to write automated scripts for your deployment. It is not perfect and is limited by Metadata API limitations, but it is the only option that allows some element of automation to your deployment step
  4. Change Sets - Another way of sending customisations and modifications from one organisation to another. Change sets can only be sent between organizations that are affiliated with a production organization. For example, a production organization and a sandbox, or two sandboxes created from the same organization can send or receive change sets. I sometimes prefer change sets because with the right process we can keep track of deployments being made to diffenent environments.

Some of the obstacles that currently prevent us from automating deployment on the platform:

  1. Changes to apex code that have apex jobs pending or in progress can't be deployed till the jobs have stopped or cancelled. Update: Sebastian pointed out in the comments that in Winter 15, this should now be possible.
  2. Not all components [can be deployed](http://www.salesforce.com/us/developer/docs/api_meta/index_Left.htm#StartTopic=Content/meta_unsupported_types.htm?SearchType=Stem/) via the Metadata API. There are some changes that you have to make manually like Account Teams, Case team roles, Console layout etc. It allows you to deploy the important components like Apex code, sObject metadata etc. But if we are talking about 100% automation, then this is not enough.
  3. Large deployments take an inordinate amount of time, and a single unit test failure requires aborting the deployment process and start over again after fixing the test class. Some of the reasons that affect deployment time are:
      * Number and size of files - no brainer really, the more you have to deploy, the longer it takes to deploy
      * Type of components - Some components like custom fields, custom junction objects etc. take longer to deploy than others
      * Processing time - if you make a change that requires updating data records, then it'll take more time e.g. changing field type
      * Test Execution - more tests you have, more time it'll take
      * Network and server availability - if you deploy during peak hours, then you might notice a slight delay in deployment
  4. Again, for large deployments that involve deleting and/or renaming a field, object or creating new sharing rule etc, you end up doing manual pre-deployment steps and post deployment steps.
  5. It is impossible to rollback your changes from Production if your deployment goes horribly wrong or you find a show-stopper bug in your smoke tests post deployment. You cannot revert back to a previous version of code, metadata and data. The only strategy is roll-forward. 
  6. You can deploy sObject schema changes but you cannot automate deployment of data changes along with it.

This is not ideal, and I hope that Salesforce is working on fixing/improving this situation leading the innovation to improved deployment in the cloud. If I had a wish list, I'd ask for the following:

  1. Ability to rollback a deployment 
  2. Ability to deploy (almost) everything under Setup via the metadata API
  3. This one is controversial amongst my peers, but why not make 75% code coverage mandatory when deploying to sandboxes. More often than not, I have seen developers worry about this constraint at the end of the development lifecycle even in projects that claim to follow Agile methodologies like TDD. Making this mandatory will force the developer to tackle this issue early on and if they do it right it will create robust and better quality code. I think it'll make a lot of developers upset initially but they'll get over it.
  4. Ability to create and trigger change set deployment via Ant Migration toolkit.

It is a hard problem to solve and Salesforce will not be able to solve all of the deployment issues or be able to provide tools to enable 100% automated deployment. Some of the deployment issues will have to be solved by the developers by creating a deployment strategy that is aligned with your organisations internal Release management strategy.

A deployment strategy should enable you to automate as much of your deployment process as possible:

  1. It should have a version control system to support deployments to different environments
  2. You can use a mixture of ant migration tool (for components that can be deployed via metadata api) and Change Sets (for components that cannot be deployed via Metadata API)
  3. The tools should be supported by a process or a set of processes that makes deployments easy without affecting developers time
      * Developers should check-in their code daily in the project/dev branch
      * The master branch is linked to the prod instance
      * Any changes to master branch is reverse integrated with other dev branches
      * If you are working on multiple releases at the same time, consider using an Integration branch
      * Release manager to write automated scripts to message metadata and deploy from VCS branches to different environments. Only deploy deltas to other environments.
      * Maintain a template to track changes that are manually migrated or deployed using change sets
  4. Build a Continuous Integration process to run tests automatically whenever something is checked in into the project / dev branch or master branch to ensure the branch is always deployable
    
In conclusion, even though we cannot automate deployments on the Force.com platform yet, we can still build processes and work flows that'll automate upto 75% of it. However, there is still scope for Salesforce to innovate in this space and release features to decrease the pain in deployments.

