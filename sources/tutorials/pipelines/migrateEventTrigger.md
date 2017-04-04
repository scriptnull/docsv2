page_title: Switching from Event Triggers to runCI
page_description: Detailed instructions on how to use runCI to accomplish the same scenario as eventTriggers
page_keywords: getting started, pipelines, quick start, documentation, shippable

#Switching from Event Triggers to runCI

Until Feb. 18, 2017, if you had set up a connection from Shippable CI to Shippable Pipelines, it was likely through an Event Trigger account integration. This required you to complete all kinds of manual steps repeatedly for every resource you wanted to update. We have introduced a new and efficient mechanism to achieve the same scenario with a new job type called `runCI`.

The main goal of this the new `runCI` job is to make CI a first class citizen in Pipelines. This introduces a lot of flexibility in how and where you can use CI jobs, as well as improves SPOG performance due to a more efficient implementation.

**We will retire Event Trigger functionality for triggering Resources on April 30th, 2017. If you do not migrate to runCI before this date, your pipeline will not trigger after CI completes.** This page describes how you can migrate from Event Trigger to [runCI](../../pipelines/jobs/runCI.md) to connect CI with your Pipelines.

##The Scenario

You have a project enabled for CI which builds and pushes a Docker image to a Docker registry. You use an Event Trigger integration to trigger an update for your image resource, which triggers the rest of your pipelines workflow.  

<img src="../../images/pipelines/event-trigger-ci-pipelines.png" alt="Hook button on project settings page." style="width:600px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

##Migration steps

Please follow the following steps for **all enabled projects with an Event Trigger integration.**

### 1. Set up runCI

The runCI job represents your CI job and allows CI jobs to directly integrate with your pipelines. For all projects enabled after February 18, 2017, a runCI job is automatically created and named by appending `_runCI` to your repository name. You can skip this step if you see this runCI job in your SPOG already.

If you do not see a runCI job for your repository in SPOG, you need to create it by following the steps below:

- Go to the `Settings` tab for your project. Find the `Hook pipeline` section and click on `Hook`.

<img src="../../../pipelines/images/jobs/hookPipeline.png" alt="Hook button on project settings page." style="width:600px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

- If you don't see the button, recheck your pipelines SPOG to see if your runCI job already exists.  

- Your pipelines SPOG should now show the runCI job:

<img src="../../../pipelines/images/jobs/runCIInSPOG.png" alt="SPOG with runCI job" style="width:500px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>


### 2. Add runCI to your shippable.jobs.yml

- Next, you need to add the runCI job to your pipelines yml. Add the following to your `shippable.jobs.yml` file in your [syncRepo](../../pipelines/resources/syncRepo.md).  

```

jobs:
  - name: my_app_runCI  ## Replace my_app_runCI with your runCI job name
    type: runCI
    steps:
      - OUT: my_image ## Replace my_image with the resource name that was being triggered by Event Trigger

```

This gives you a lot of flexibility regarding what your CI can do in your pipeline.

- Commit the changes to your sync repository. When the rSync job finishes, your SPOG should show this:

<img src="../../../pipelines/images/jobs/runCIOutSpog.png" alt="SPOG with runCI job" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

You can see that the `OUT` resource is now connected to your runCI job.

### 3. Update your resource from CI.

Next, you need to edit your `shippable.yml` to update the resource at the end of each CI build:

- Update your `shippable.yml` as shown below to update the resource:

```
build:
  ci:
    # your ci commands

    - echo "versionName=$BRANCH.$BUILD_NUMBER" >> $JOB_STATE/my_image.env  #This command updates an image resource called my_image.

```

- Make the following changes to the command:
    - `versionName=$BRANCH.$BUILD_NUMBER` should be what you have in the `payload` tag in your Event Trigger.
    - Replace `my_image` with your resource name
    - The echo command should be after the commands that push your image to a registry

- Delete the Event Trigger integration from the yml
- You can also update the resource conditionally as shown below:

```
  #resource is only updated for commit builds, but not for pull requests
  - 'if [ $IS_PULL_REQUEST == "false" ] ; then echo "versionName=$BRANCH.$BUILD_NUMBER" >> $JOB_STATE/my_image.env; fi'
```

```
  #resource is only updated for master branch, but not for other branches
  - 'if [ $BRANCH == "master" ] ; then echo "versionName=$BRANCH.$BUILD_NUMBER" >> $JOB_STATE/my_image.env; fi'
```

- Commit and push this change. This will trigger a CI build and your resource should be updated as part of this build. This will trigger the rest of your pipeline.

<img src="../../images/pipelines/OUTlogNewVersion.png" alt="Processing `OUT` with updated version" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>


### 4. Repeat for each project using Event Trigger

Repeat steps 1-3 for every project using an Event Trigger integration to trigger resources.

### 5. Delete all Event Trigger integrations

To do this:

- Go to your Subscription Settings->Integrations page and remove all the integrations you just replaced with runCI jobs
- Go to your Account Settings and remove all these old integrations from there as well

### 5. Improve SPOG performance

The new `runCI` implementation is much more efficient and leads to better SPOG performance when compared to Event Triggers. You will automatically see this improvement
after April 30, 2017. However, if you are experiencing slow SPOG load, you can do the following to tell Shippable that you have finished your migration to runCI:

- Go to your Subscription settings. In the `Pipeline Event Triggers` section, check the box for hiding event triggers:

<img src="../../images/pipelines/hide-event-triggers.png" alt="Processing OUT resource" style="width:600px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/><br>

Please do this ONLY if you have completed steps 1-5 above.

##Additional information

* For more information on runCI, [check out the official docs](../../pipelines/jobs/runCI.md)
* For details on environment variables and different scenarios that can be accomplished using INs and OUTs, [see the docs on runSh](../../pipelines/jobs/runSh.md)
* For a more detailed example of how to use the `.env` files, [see our tutorial on updating resource versions](updateResourceVersion.md)
