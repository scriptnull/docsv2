page_title: Switching from Event Triggers to runCI
page_description: Detailed instructions on how to use runCI to accomplish the same scenario as eventTriggers
page_keywords: getting started, pipelines, quick start, documentation, shippable

#Switching from Event Triggers to runCI
Until Feb. 18, 2017, if you had set up a connection from Shippable CI to Shippable Pipelines, it was likely through an Event Trigger account integration. This required you to complete all kinds of manual steps repeatedly for every resource you wanted to update. We have introduced a new and efficient mechanism and will be retiring event triggers functionality on April 30th, 2017. This page describes how you can migrate from Event Trigger to the new [runCI](../../pipelines/jobs/runCI.md) style of connecting CI with Pipelines.

##The Scenario

I have a project enabled for CI.  That project builds and pushes a docker image, and then uses an event trigger to update my image resource, which triggers my pipelines workflow.

My `shippable.yml`
```yml
language: node_js

node_js:
  - 4.2.3

build:

  ci:
    - ./buildAndPush.sh

integrations:
  notifications:
    - integrationName: "trigger_my_image"
      type: webhook
      payload:
        - versionName=$BRANCH.$BUILD_NUMBER

```

My `shippable.resources.yml`
```yml
resources:
  - name: my_image
    type: image
    pointer:
      sourceName: trriplejay/simpleserver
    seed:
      versionName: latest

```

##Setting up runCI
The runCI job represents your CI job and allows CI jobs to directly integrate with your pipelines. First, the runCI job must be created.  You can do this simply by going into your project settings page, and clicking the "hook" button.  

<img src="../../../pipelines/images/jobs/hookPipeline.png" alt="Hook button on project settings page." style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

If you don't see the button, check your pipelines SPOG to see if your runCI job already exists.  Any project that was enabled after this functionality was released on Feb. 18, 2017 will not need to use the hook button.

Once this button is clicked, if you look at your pipelines SPOG, you should see something like this:

<img src="../../../pipelines/images/jobs/runCIInSPOG.png" alt="SPOG with runCI job" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>


##Add runCI to your shippable.jobs.yml

Now that you have a base runCI job created for your project, you can enhance it via the `shippable.jobs.yml` file in your [syncRepo](../../pipelines/resources/syncRepo.md).  

My `shippable.jobs.yml`<br>
```yml
jobs:
  - name: my_app_runCI
    type: runCI
    steps:
      - OUT: my_image

```

This gives you a ton of flexibility regarding what your CI can do in your pipeline.

In this example, we're simply going to add a single `OUT` statement for our image resource.  You might be familiar with this notation if you've used our [runSh](../../pipelines/jobs/runSh.md) or [runCLI](../../pipelines/jobs/runCLI.md) job types.

Once rSync successfully runs for this change, your SPOG should look something like this:<br>

<img src="../../../pipelines/images/jobs/runCIOutSpog.png" alt="SPOG with runCI job" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

You can see that the `OUT` resource is now connected to your runCI job.

##Update your resource from CI.
Now that the image resource is connected to runCI though an `OUT` statement, you will see it referenced several times when the build runs.

<img src="../../images/pipelines/OUTlogExample.png" alt="Processing OUT resource" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/><br>
<img src="../../images/pipelines/OUTlogExample2.png" alt="Processing OUT resource pt2" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

You can see here that the processor will only create new versions of `OUT` resources if a change is detected.  This allows you to control when a resource is updated, based on logic in your `shippable.yml`.

The structure of a runCI job is very similar to runSh and runCLI.  Every build will have access to certain standard environment variables that represent the different resources and jobs involved in the build.  Each build also has a state directory where you can store information that you'd like to persist from one job to the next, or that you'd like to pass through to the next stage in your pipeline.  This directory can be accessed via the `$JOB_STATE` variable.

Additionally, each `OUT` resource in your runCI job will have a corresponding `<resourceName>.env` file inside the state directory.  This is the file that you need to update in your job in order to signify that you want to create a new version of your resource.

Update your `shippable.yml` to look like this:
```yml
language: node_js

node_js:
  - 4.2.3

build:

  ci:
    - ./buildAndPush.sh
    - echo "versionName=$BRANCH.$BUILD_NUMBER" >> $JOB_STATE/my_image.env

#integrations:
#  notifications:
#    - integrationName: "trigger_my_image"
#      type: webhook
#      payload:
#        - versionName=$BRANCH.$BUILD_NUMBER

```

Notice that I've commented out the webhook trigger, and instead, added a single line to the ci section `- echo "versionName=$BRANCH.$BUILD_NUMBER" >> $JOB_STATE/my_image.env`

You can use any identifier you'd like in place of `$BRANCH.$BUILD_NUMBER`.  In the case of an image resource, the `versionName` should represent the image's tag, so you should mark that based on what you've pushed during your build.

If you only docker build and push on commits, and want to ignore pull requests, you may want to use a line like this instead:
```yml
  - 'if [ $IS_PULL_REQUEST == "false" ] ; then echo "versionName=$BRANCH.$BUILD_NUMBER" >> $JOB_STATE/my_image.env; fi'
```
Note that you can use similar logic to control what happens based on any other condition, such as branch name, test results, matrix values, etc.

Committing this change should trigger another CI build, and its results should look something like this:
<img src="../../images/pipelines/OUTlogNewVersion.png" alt="Processing `OUT` with updated version" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

As you can see, a new version was posted for the image resource.  This is what causes subsequent pipeline jobs to be automatically triggered.  If you navigate to your SPOG and click on your image resource, you should see the version that was created.

<img src="../../images/pipelines/spogImageVersions.png" alt="See the version list of your resource from the SPOG view" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

Now you can feel free to delete that old event trigger from your yml, delete the subscription integration from your subscription settings page, and delete the account integration from your account settings page!

##Additional information
* For more information on runCI, [check out the official docs](../../pipelines/jobs/runCI.md)
* For details on environment variables and different scenarios that can be accomplished using INs and OUTs, [see the docs on runSh](../../pipelines/jobs/runSh.md)
* For a more detailed example of how to use the `.env` files, [see our tutorial on updating resource versions](updateResourceVersion.md)
