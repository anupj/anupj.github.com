---
layout: post
title: "Thoughts on Automating Salesforce Deployment"
category: Deployment
comments: true
---

<img src="/images/cloud-deployment.jpg" alt="Deploy button" />

Deployment is one of the most important parts of any software project. Depending on the size of the project, the deployment could be anything from a two minute activity for one person to a two day endeavor for an army of resources. Of course the holy grail of deployments is achieving the [Expert (Level 5) maturity level][maturity] in Continuous Delivery. To reach this goal, you should always be looking for ways to automate the deployment process. For example, you want an automated process that checks out code from the master branch of your Version Control System (VCS), runs all tests, validates the deployment and then deploys it to the production environment (ideally) without human intervention. All of this will lead to Continuous Delivery nirvana.

Unfortunately, it is currently not possible to implement such a strategy on the Salesforce1 platform. While salesforce.com has significantly innovated how we build, compile, debug and test code in the cloud, one of the things with which both newbies and seasoned developers still struggle is automating deployments.

So what’s the solution? To start, let’s look at the deployment options currently available on the platform:

  1. **Manual Migration**: Manually repeat the exact modifications in every development or production organization. This approach is tedious and time-consuming, therefore it’s only recommended for very small changes and for components that cannot be deployed by Metadata API or change sets.
  2. **Eclipse IDE Plugin**: Deploy components (that can be deployed using Metadata API) from the Eclipse IDE using the Force.com plugin. This approach is not recommended for large deployments because you will be locked out of your IDE, which sometimes has a tendency to crash for long running operations. Additionally, this approach requires you to track the components being deployed separately very diligently since the IDE does not track this information for you.
  3. **Ant Migration Tool**: A command-line utility for moving metadata between a local directory and a Salesforce organization using the Metadata API. This approach is recommended for multi-stage release processes and/or if you prefer to write automated scripts for the deployment. Although this approach isn’t perfect, for example there are Metadata API limitations, it’s the only option that brings some element of automation to the deployment.
  4. **Change Sets**: Send customizations and modifications from one organization to another. Change sets can only be sent between organizations that are affiliated with a production organization — for example, a production organization and a sandbox or two sandboxes created from the same organization. I sometimes prefer change sets because, with the right process, you can keep track of deployments being made to different environments.

Why can’t any of these approaches be fully automated? The answer is five-fold:

  1. Not [all components can be deployed][undeployable components] via Metadata API. There are some changes, such as Account Teams, Case team roles, Console layout, etc., that you must deploy manually. While the Metadata API does allow you to automate the deployment of important components like Apex code and sObject metadata, it falls short of 100% automation.
  2. Large deployments take an inordinate amount of time, and a single unit test failure requires aborting the deployment process, fixing the test class and starting over again. Some of the reasons that affect deployment time are:
      * Number and size of files - no brainer really, the more you have to deploy, the longer it takes to deploy
      * Type of components - Some components like custom fields, custom junction objects etc. take longer to deploy than others
      * Processing time - if you make a change that requires updating data records, then it'll take more time e.g. changing field type
      * Test Execution - more tests you have, more time it'll take
      * Network and server availability - if you deploy during peak hours, then you might notice a slight delay in deployment
  3. For large deployments that involve deleting and/or renaming a field or object, creating a new sharing rule, etc., you end up doing manual pre- and post-deployment steps.
  4. It’s impossible to rollback your changes from production if your deployment goes horribly wrong or if you find a show-stopping bug in your post-deployment smoke tests. You cannot revert back to a previous version of code, metadata and data, so the only strategy is to roll forward.
  5. You can deploy sObject schema changes, but you cannot automate the deployment of data changes along with it.

While these obstacles to a completely automated deployment are not ideal, they’re not unique to Salesforce. In fact, these types of obstacles are common among cloud platforms. This leaves salesforce.com in a unique position to innovate and lead the charge in improving cloud deployment capabilities. If I had a wish list, I’d ask for the following abilities:

  1. Rollback a deployment
  2. Deploy (almost) everything under setup via the Metadata API
  3. An elegant way to capture and track code and metadata changes deployed from one environment to another
  4. Make 75% test coverage mandatory in sandbox environments
  5. Create and trigger change set deployment via the Ant Migration toolkit

While seeing this wish list fulfilled would be fantastic, this is a hard problem to solve and will take time. In the meantime, though, there are steps you can take to improve the situation. It all starts by creating a deployment strategy that is aligned with your organization’s internal release management strategy. Doing so should enable you to automate as much of your deployment process as possible. Your deployment strategy should include:

  1. A VCS system to support deployments to different environments.
  2. A mix of Ant Migration tools (for components that can be deployed via Metadata API) and change sets (for components that cannot be deployed via Metadata API).
  3. Processes that make deployments easy without affecting developers’ time:
      * Developers should commit their code daily in the project/dev branch
      * The master branch should be linked to the production instance
      * Any changes to the master branch should be reverse integrated with other dev branches
      * A release manager should write automated scripts to manage metadata and deploy from VCS branches to different environments
      * A template should track changes that are manually migrated or deployed using change sets
  4. A continuous integration process to run tests automatically whenever something is committed to the project/dev branch or the master branch to ensure the branch is always deployable.
  5. Depending on the size of the project and team, you may also need a dedicated release management team to manage deployments and releases by enforcing the process and policies.

So, even though we cannot fully automate deployments on the Force.com platform yet, we can still build processes and workflows that will automate up to 75% of the requirements. Going forward, we can hope that a cloud innovator like salesforce.com will take the lead in this space, improving the process not only for its own platform but for other cloud platforms as well.

[maturity]: http://www.infoq.com/articles/Continuous-Delivery-Maturity-Model
[undeployable components]: http://www.salesforce.com/us/developer/docs/api_meta/index_Left.htm#StartTopic=Content/meta_unsupported_types.htm?SearchType=Stem/
