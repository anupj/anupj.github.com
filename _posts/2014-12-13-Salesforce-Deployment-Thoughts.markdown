---
layout: post
title: "Thoughts on Automating Salesforce Deployment"
category: Deployment
comments: true
---

<img src="/images/deploy-button.jpg" height="300px" width="300px" alt="Deploy button" />

Deployment is one of the most important parts of any software project. It is very hard to justify the success of your project being “delivered on time” if your deployment fails (or takes longer than what is acceptable to the business) and requires a rollback. Depending on the size of the project, the deployment could be a two minute activity for one person or a two day endeavor for an army of resources. Of course the holy grail of deployments is achieving the Expert (Level 5) maturity level in Continuous Delivery. To reach this goal, you should always be looking for ways to automate the deployment process. For example, you want an automated process that checks out code from the master branch of your Version Control System (VCS), runs all tests, validates the deployment and then deploys it to the production environment (ideally) without human intervention. All of this will lead to Continuous Delivery nirvana.
Unfortunately, it is currently not possible to implement such a strategy on the Salesforce1 platform. Salesforce.com has introduced a lot of innovation in how we build, compile, debug and test code in the cloud. However, one of the things with which both newbies and seasoned developers on the platform still struggle is automating deployments, regardless of how simple or complex they may be.
So what’s the solution? To start, let’s look at the deployment options currently available on the platform:

  1. Manual migration - Manually repeat the exact modifications in every development or production organization. This approach is obviously tedious and time-consuming, therefore it’s only recommended for very small changes and for components that cannot be deployed by Metadata API or Change Sets.
  2. Eclipse IDE plugin - Deploy components (that can be deployed using Metadata API) from the Eclipse IDE using the Force.com plugin. This approach is not recommended for large deployments because you will be locked out of your IDE, which sometimes has a tendency to crash for long running operations. Additionally, this approach requires you to track the components being deployed separately very diligently since the IDE does not track this information for you.
  3. Ant Migration Tool - A command-line utility for moving metadata between a local directory and a Salesforce organization using the Metadata API. This approach is recommended for multi-stage release processes and/or if you prefer to write automated scripts for the deployment. Although this approach isn’t perfect, for example there are Metadata API limitations, it is the only option that brings some element of automation to the deployment.
  4. Change Sets - Send customizations and modifications from one organization to another. Change sets can only be sent between organizations that are affiliated with a production organization -- for example, a production organization and a sandbox or two sandboxes created from the same organization can send or receive change sets. I sometimes prefer change sets because, with the right process, you can keep track of deployments being made to different environments.

Why can’t any of these approaches be fully automated? The answer is five-fold:

  1. Not all components can be deployed via the Metadata API. There are some changes, such as Account Teams, Case team roles, Console layout etc., that you must deploy manually. While the Metadata API does allow you to automate the deployment of important components like Apex code and sObject metadata, it falls short of 100% automation.
  2. Large deployments take an inordinate amount of time, and a single unit test failure requires aborting the deployment process, fixing the test class and starting over again.. Some of the reasons that affect deployment time are:
      * Number and size of files - no brainer really, the more you have to deploy, the longer it takes to deploy
      * Type of components - Some components like custom fields, custom junction objects etc. take longer to deploy than others
      * Processing time - if you make a change that requires updating data records, then it'll take more time e.g. changing field type
      * Test Execution - more tests you have, more time it'll take
      * Network and server availability - if you deploy during peak hours, then you might notice a slight delay in deployment
  3. Again, for large deployments that involve deleting and/or renaming a field or object, creating a new sharing rule, etc., you end up doing manual pre- and post-deployment steps.
  4. It’s impossible to rollback your changes from production if your deployment goes horribly wrong or if you find a show-stopper bug in your post-deployment smoke tests. You cannot revert back to a previous version of code, metadata and data, so the only strategy is roll forward.
  5. You can deploy sObject schema changes, but you cannot automate the deployment of data changes along with it.

While these obstacles to a completely automated deployment are not ideal, they're not unique to Salesforce. In fact, these types of obstacles are common among cloud platforms. This leaves salesforce.com in a unique position to innovate and lead the charge in improving cloud deployment capabilities. If I had a wish list, I'd ask for the following:

  1. Ability to rollback a deployment
  2. Ability to deploy (almost) everything under Setup via the metadata API
  3. This one is controversial amongst my peers, but why not make 75% code coverage mandatory when deploying to sandboxes. More often than not, I have seen developers worry about this constraint at the end of the development lifecycle even in projects that claim to follow Agile methodologies like TDD. Making this mandatory will force the developer to tackle this issue early on and if they do it right it will create robust and better quality code. I think it'll make a lot of developers upset initially but they'll get over it.
  4. Ability to create and trigger change set deployment via Ant Migration toolkit.

It is a hard problem to solve and Salesforce will not be able to solve all of the deployment issues or be able to provide tools to enable 100% automated deployment. Some of the deployment issues will have to be solved by the developers by creating a deployment strategy that is aligned with your organisations internal Release management strategy.

A deployment strategy should enable you to automate as much of your deployment process as possible:

  1. A version control system to support deployments to different environments
  2. A mix of Ant Migration tools (for components that can be deployed via the Metadata API) and change sets (for components that cannot be deployed via the Metadata API).
  3. The tools should be supported by a process or a set of processes that make deployments easy without affecting developers time
      * Developers should commit their code daily in the project/dev branch
      * The master branch should be linked to the prod instance
      * Any changes to master branch should be reverse integrated with other dev branches
      * Consider using an Integration branch, if you are working on multiple parallel releases
      * A release manager should write automated scripts to message metadata and deploy from VCS branches to different environments. Only deploy deltas to other environments.
      * Maintain a template to track changes that are manually migrated or deployed using change sets
  4. A continuous integration process to run tests automatically whenever something is committed to the project/dev branch or the master branch to ensure the branch is always deployable.
  5. Depending on the size of the project and team, you may also need a dedicated release management team to manage deployments and releases by enforcing the process and policies.

In conclusion, even though we cannot automate deployments on the Force.com platform yet, we can still build processes and work flows that'll automate upto 75% of it. However, there is still scope for Salesforce to innovate in this space and release features to decrease the pain in deployments.
