page_title: Unified Pipeline Jobs - runCI
page_description: List of supported jobs
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# runCI
Each `runCI` job represents a project from Shippable CI.  It's a wrapper around your project that allows for native integration into your continuous deployment pipelines.

## Creating a runCI job
`runCI` jobs are automatically created when a project is enabled in the CI section of Shippable.  If your project is already enabled for CI, then you can create a corresponding runCI job by clicking the "Hook" button on the project settings page.

<img src="../../images/jobs/hookPipeline.png" alt="Hook button on project settings page." style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

Once enabled or hooked, the job is visible in your SPOG, just as though you had added it via `shippable.jobs.yml`.

<img src="../../images/jobs/runCIInSPOG.png" alt="runCI in SPOG" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

You can see that it is connected to a [ciRepo resource](../resources/ciRepo.md).

Your runCI job will show you the latest status of the default branch of your CI project.  To display status for additional branches, [add those branches](../../navigatingUI/projects/settings/#dashboard-settings) in the "Runs Config" section of your project settings page.

## Utilizing a runCI job

Once your runCI is configured, you can treat your project's `shippable.yml` file like you would treat a scripted `TASK` section of a runCLI or runSH job. You can use the runCI job as an `IN` on one of your other jobs to trigger and pass state when CI completes.  For general information on how that works, as well as some of the most common use-cases, [check out the runSh docs](runSh.md).

## Enhancing a runCI job
Once the basic runCI job is created for a project, it can be enhanced through your `shippable.jobs.yml` file in your [syncRepo](../resources/syncRepo.md), just like any other pipeline job.  This allows you to easily set up triggers and OUT resources that will kick off various pipeline workflows when your CI tests pass, based on customizable logic in the project's `shippable.yml` file.

You can do this simply by adding a job to your `shippable.jobs.yml` that references the existing runCI job that appears in your SPOG:

```yml
jobs:
  - name: my_app_runCI      # required
    type: runCI             # required
    steps:                  # required
      - OUT: <resourceName> # at least one IN or OUT required

```

* `name` needs to match what you already can see in SPOG.  It should be in the format `<projectName>_runCI`
* `type` should always be `runCI`
* There should be at least one `<resourceName>` as an `IN` or `OUT` to the job.  It can be any type of resource that you'd like to utilize during CI, including `image`, `file`, `params`, `replicas`, `loadBalancer`, etc. See our [resources page](../resources/overview/) for the full list of available resources.

Once your rSync job completes, you will see your SPOG runCI update to match the configuration in your `shippable.jobs.yml`.

<img src="../../images/jobs/runCIOutSpog.png" alt="SPOG updated to have the image as an OUT of the runCI" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

Now you can configure your `shippable.yml` to update your resources for fully integrated pipeline workflows.  For instructions on how to update resources [check out our tutorial](../../tutorials/pipelines/updateResourceVersion.md).

If your runCI job is triggered by pipeline resources(ie. not a manual or webhook triggered build), then you can check which resource triggered the current build by checking `JOB_TRIGGERED_BY_NAME` and `JOB_TRIGGERED_BY_ID` environment variables

<img src="../../images/jobs/runCITriggeredBy.png" alt="runCI triggered by resource" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

Note that runCI jobs won't process the `TASK` section.  The `TASK` section is automatically configured to be the CI job itself.  Because of this, you also cannot use `NOTIFY` sections in the runCI job.  Instead, notifications should be set up directly in your CI project's `shippable.yml`, which you can read about [here](../../ci/shippableyml/#notifications).
