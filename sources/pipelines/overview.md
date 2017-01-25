page_title: Unified Pipelines Overview
page_description: Overview of Shippable Pipelines
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc


# Shippable Continuous Deployment Pipelines

Shippable's continuous deployment pipelines provide an integrated platform, built from the ground up, to defragment and streamline the process of shipping applications.

Using our platform, software-powered organizations can accelerate innovation by shrinking their software release lifecycle. We enable this by giving you a continuous deployment platform that helps you:

#####Improve productivity

Most deployment pipelines today need to be cobbled together with tools that solve part of the end to end requirement. These pipelines are mostly homegrown and DevOps engineers spend a lot of time writing thousands of lines of imperative code to create them. As a result, companies either automate only part of their deployment workflow or bite the bullet and spend the time required to build these pipelines. Both choices are suboptimal and slow down your release cycles.    

Shippable gives you an **end to end declarative platform** that will get your deployment pipeline up and running in less than an hour. No custom coding required!

#####Always use the best tools for the job

Our pipelines are designed to work for your processes and tools, with out-of-the-box, native integrations for popular tools and services.  Want to switch between  HipChat and Slack? Configure it in minutes. Want to deploy to a different cloud endpoint? Also configured in minutes! In addition to our native integrations, you also have complete flexibility to adapt to your specific use cases or integrate with any service or tools, including those you have internally developed.

#####Ship cloud native and traditional applications in a unified pipeline

Cloud-native is the new buzzword and for good reason as it represents tremendous opportunities to attain new levels of flexibility and efficiency in your organization. But if you're like most organizations, you're going to transition over time and need to also support your existing apps, such as your suite of Java applications. With Shippable, you can use the same pipeline to ship both types of applications.

#####View all your deployments in a Single Pane of Glass

Our pipelines give you an end to end visual representation of your deployment workflows across your entire organization, with the ability to zoom in and out of any part of it. You can view version history, logs, and also upgrade and rollback in no time.

To get started, read our [Getting Started Guide](gettingStarted/).

---

## Why do I need Continuous Deployment?

Today, most organizations find it challenging to innovate fast enough to satisfy consumers. DevOps is a set of principles that tries to solve this problem, with the ultimate goal to achieve full automation of your software delivery pipelines, also called Continuous Deployment.

The picture below shows a high level maturity model for Continuous Deployment. As shown below, there are multiple stages of capability required to achieve Continuous Deployment, starting with Continuous Integration.

<img src="../images/cdMaturityModel.png" alt="Shippable Continuous Integration and Delivery" style="width:800px;"/>

Most organizations today are at L1 or L2 of this maturity model, but are constantly looking to improve to higher levels of maturity.

Successful companies like Facebook, Netflix, and Amazon have achieved L3 and L4 maturity and, as a result, deliver more value through software at a faster pace than traditional software organizations. However, organizations that have achieved this maturity level have often invested significant time and resources to create their continuous deployment pipelines, often with highly-customized designs that piece together specific, fragmented tools via thousands of lines of custom code. Once created, these customized pipelines achieve the desired automation and speed, but are often inflexible and difficult to change.

The best approach is to achieve the needed maturity without significant investment or unwanted rigidity.

**Shippable helps you mature from L1 to L4 at your own pace, without needing to build homegrown pipelines.** Your pipelines are configured in a human readable, declarative language, making it very easy to create, update, and share your release workflows. We offer the only integrated platform that addresses your end to end needs from Continuous Integration (CI) through Application Release Automation (ARA) for a complete solution for the least investment.

To get started, read our [Getting Started Guide](gettingStarted/).

---

## Sample use case

Let us consider the end to end deployment for a single service, with source code maintained in a repository called gitRepo. In Shippable, you easily construct pipelines by defining 'resources' (gray) and 'jobs' (green). For example, here's a sample pipeline scenario you could construct:

<img src="../images/codeToProdPipelines.png" alt="Shippable Continuous Integration and Delivery" style="width:1200px;"/>

In this scenario:

* Every time your code changes, it triggers CI. This job is represented by `ci`.

* Upon a successful CI run, a versioned docker image for your service is built and pushed to a Docker registry. This is represented by the `image` resource.

* This triggers creation of a new version of your service manifest, represented by the `manifest` job. This job also takes additional parameters like docker options, environment variables, etc.

* The updated manifest version automatically triggers a deployment to your Test environment. This is represented by the `deploy-Test` job and deploys to the 'cluster-test' resource specified for the test environment (though not pictured, you could also configure Shippable to trigger functional tests each time this environment is updated).

* Each time the Test environment is updated, your Test team is notified through a Slack channel.

* Once ready (e.g. all tests have passed), the `release` job executes, creating a new release candidate and incrementing the version number. Your Ops team is notified about the new release creation through a Slack channel.

* The new release is then automatically deployed via the `deploy-beta` job to the Beta environment running on the `cluster-beta` cluster.

* After further testing in the Beta environment (e.g. stress tests, smoke tests, etc.), it's ready to deploy to Prod (if needed, we could have also inserted another `release` job between Beta and Production deploy jobs).

* **Release day**! You trigger a manual deploy to production! This is represented by the `deploy-Prod` job.

---
## Deploying a sample application
To get started with using pipelines and to understand how they work, you should try deploying our sample application. It will take less than 30 minutes and is a great example to showcase how a simple application can be deployed with Pipelines.

Check out the sample application in our [Pipeline Tutorials section](../../tutorials/pipelines/samplePipeline/)

---

## Tutorials

A complete list of CI tutorials is provided on our [CD Tutorials Index page](/tutorials/continuous-delivery-pipelines/)
