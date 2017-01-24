page_title: Unified Pipelines Overview
page_description: Overview of Shippable Pipelines
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# Getting started with Continuous Deployment Pipelines

This page will give you enough information to get started with your deployment pipelines. It covers basic concepts and a short guide on how to seed your pipeline.

You can also see pipelines in action by following our tutorial on [deploying a sample application](../tutorials/pipelines/samplePipeline/),

---

##Pipeline building blocks

[**Resources**](resources/overview/) are the versioned building blocks of your pipelines. They are inputs for and sometimes outputs from the executable units of your pipeline, aka [Jobs](jobs/overview/). Examples of resources include source code repositories, docker images, container clusters, environment variables, etc.

[**Jobs**](jobs/overview/) are the executable units of your pipelines. They take one or more [resources](resources/overview/) as inputs, perform some operation on the inputs, and can output to other resources. Jobs can also act as inputs for other jobs, which serve to daisy-chain a series of jobs into a pipeline.

[**Triggers**](triggers/) are used to manually trigger a workflow by running a job that takes the trigger as an input. Manually triggering a job is also available through the SPOG UI, though a `trigger` defined in a versioned yml file in your source control includes all of the versioning and history that comes with repo-managed files.

---

##Configuration files

Your deployment pipelines are defined through three yml-based configuration files:

**shippable.jobs.yml** is a required file and contains definitions of the [Jobs](jobs/overview/) in your pipeline.

**shippable.resources.yml** is a required file and contains definitions of the [Resources](resources/overview/) in your pipeline.

**shippable.triggers.yml** is an optional file and contains definitions of manual [Triggers](triggers/) for jobs in your pipeline. This file is needed only if you want to manually trigger jobs through commits to your source control repository. Alternatively, you can skip this config and choose to manually trigger jobs through the SPOG view if required.

These configuration files should be committed to a [repository in your source control](#sync). If you have separate deployment pipelines for different environments or applications, you can put config files for each environment or application in separate repositories.

---

<a name="sync"></a>
##Sync repository

A source control repository that contains your pipeline configuration files is called a **Sync Repository**. Each sync repository contains one or more of `shippable.jobs.yml`, `shippable.resources.yml`, and `shippable.triggers.yml` files.

You must seed your pipeline with at least one sync repository through the Shippable UI. Subsequent sync repositories can also be added through the UI following the same process. Instructions are in the [Adding a sync repository](#seedPipeline) section below.

You may have the entire pipeline configuration maintained in one repository, split it across directories within the same repository, or split up the configuration across multiple sync repositories. This decision depends on your organizational preferences, as well as [security and permissions requirements](#permissions). For example, you may have pipeline configuration for source control through your first test environment in one repo, configuration for subsequent test environments in another repo, and configuration for your production environment in yet another repo. In this way, you can manage who can configure and execute different areas of your pipeline based on the permissions set on each repo.

---

<a name="seedPipeline"></a>

##Adding a sync repository

You can add a sync repository by following the steps below:

* First, add a subscription integration for the source control provider where your sync repository is located. Instructions are here - [Source Control Provider Integrations](../integrations/scm/scmOverview/).
* Go to your Organization's page on Shippable. A list of all available Organizations can be accessed by clicking on the  <i class="fa fa-bars" aria-hidden="true"></i>  icon.
* Click on the `Pipelines` tab
* If you have never added a sync repository, your Single Pane of Glass will be empty. Click the `+` button at upper right to add a sync repository.
* Complete the Add Resource fields:
	* The subscription integration dropdown should show the subscription you created in the first step. If not, you will need to go through the flow of adding the integration.
	* The `Select Project` dropdown will show all repositories in the source control you just connected with the integration. Choose your sync repository.
	* Select the branch of the sync repository that contains your pipeline configuration files.
	* Name your sync repository with an easy to remember name.
* Click on `Save` to apply your sync repository configuration.

At this point, Shippable will parse all configuration files in the sync repository and create your pipeline(s). You will see a visualization of the the jobs and resources from your `shippable.jobs.yml` and your `shippable.resources.yml` in the Single Pane of Glass.

Note: if you do not see what you expected, you likely have a configuration error. Click on the rSync resource in the SPOG view to see the console and identify any errors that may exist.

---

<a name="spog"></a>
## Single Pane of Glass (SPOG) View

The Single Pane of Glass (SPOG) view is a real time, interactive, visual representation of all pipelines across your organization. You can zoom in or out of any part of the pipeline with your mouse scrollbar or with pinch-to-zoom on any mouse keypad.

Each job in this view will update colors in real time based on the status of the job. Jobs are green is last run was successful, red if the last run failed, and blue if the job is in processing state.

You can interact with the SPOG view in the folllowing ways:

- Zoom in and out to see additional details for any part of the pipeline
- Right click on a job to run or pause the job. When a job is paused, it will ignore webhooks and not run unless you manually trigger the job.
- Left click on a job to view console output for each version of the job. You can also use the `trace` feature to see the job's inputs, or `properties` to see additional information about the job.

To see "orphaned" resources (resources that are not inputs to a job in the pipeline) or resources that are soft-deleted (deleted from the yml, but not from the pipeline on Shippable), use the arrow dropdown in the upper right to add them to the view.

You can also filter this view by using Flags. Flags need to be included in your job and/or resource configurations to be available as a filter for SPOG.

---

##Filtering the SPOG view

If you have a lot of pipelines configured, the SPOG view can get busy and it can be difficult to find a specific job or resource. You can use the `flags` keyword to filter SPOG for easier readability.

The `flags` keyword can be set in any job or resource:

```
jobs:
  - name: <string>
    type: <job type>
    #other job related inputs
    flags:
      - <filter name>
```

Once you commit this, the filters will be shown in the Filter dropdown in your SPOG view.

Please note that you do not need to add flags to every job or resource in your pipeline. If we detect flags for a job or resource and you filter SPOG with that flag, the UI will display all upstream and downstream jobs and resources in the pipeline.

---

## Connecting CI to your Pipelines
You can trigger your pipeline to run after your CI build is complete. To learn how to do this, check out our [tutorial about connecting CI and Pipelines](../../tutorials/pipelines/connectingCiPipelines/)

---

<a name="permissions"></a>
## Permissions

Permissions for your deployment pipelines are tightly coupled with permissions for your sync repositories. Consequently, pipeline elements are only visible and accessible to perform actions to users with **Owner**, **Collaborator/Write**, or **Read** access to the sync repository that holds the configuration files. Users that do not have access to your sync repository will not have access to that portion of your pipeline, even if they are members of your organization.

---
## Deploying a sample application
To get started with using pipelines and to understand how they work, you should try deploying our sample application. It will take less than 30 minutes and is a great example to showcase how a simple application can be deployed with Pipelines.

Check out the sample application in our [Pipeline Tutorials section](../../tutorials/pipelines/samplePipeline/)
