page_title: Migrating Pipelines from One repository to another repository
page_description: Detailed instructions on how to migrate resources, jobs and triggers from one repository to another repository
page_keywords: getting started, pipelines, quick start, documentation, shippable, migration


##Scenario
If you have more than one repository for your CI-CD pipeline and want to consolidate both into one repository without losing the history or want to migrate specific resources, jobs or triggers from one repository to another without any downtime.

##How to migrate a job, resource or trigger from repository to another

These are the steps to migrate one or more resources, jobs and/or triggers from one repository to another repository:

1. On Shippable, goto SPOG page and pause the rSync job for repository from which you want to migrate resources, jobs and/or triggers.
<img src="../../images/pipelines/pauseJob.png" alt="See the version list of your resource from the SPOG view" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

2. In another repository's shippable.resources.yml, shippable.jobs.yml and/or shippable.triggers.yml, add the resources, jobs and/or triggers you want to migrate.

3. Run the rSync job of second repository. Once rSync job completes, resources, jobs and/or triggers will be migrated to that repository. Your SPOG will not change as no dependency is changed. Therefore, to verify this you can check the logs of rSync job
<img src="../../images/pipelines/migrationConsoleLog.png" alt="See the version list of your resource from the SPOG view" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

4. Resume the parent repository and delete the migrated resources, jobs and triggers from shippable.resources.yml, shippable.jobs.yml and/or shippable.triggers.yml. Webhook will start rSync job and it should succeed.
<img src="../../images/pipelines/resumeJob.png" alt="See the version list of your resource from the SPOG view" style="width:800px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

5. Now, Your resources, jobs and/or triggers are migrated to second repository. You can add, remove or update them there.

**NOTE:** While migrating a job you should copy it exactly the same in the parent repository. Dependencies removed or added during migration will not be reflected after migrating. You can add or remove dependencies once the resource is successfully migrated to new repository.
