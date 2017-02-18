page_title: Unified Pipeline Resources - syncRepo
page_description: List of supported resources
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# syncRepo
The `syncRepo` resource is at the heart of your deployment pipelines. This resource is a pointer to the source control repository containing the files that define your pipelines:  `shippable.resources.yml` and `shippable.jobs.yml`.

When you add a `syncRepo`, Shippable will read the jobs and resources ymls and create your pipeline. We also add a webhook on the source repository that notifies Shippable each time anything in the repository is changed. This webhook notification will automatically sync any changes you make to the jobs and resources ymls and reflect them in your pipeline.

**You need to add at least one syncRepo resource from the Shippable UI.** Additional syncRepo resources can be added through the UI or by specifying them in the shippable.resources.yml file in the first syncRepo that was added.

This is the only resource type that can be added from the UI. You should not add the same repository as a sync repo more than once. This can lead to unexpected behavior.

## Adding a syncRepo from the UI

* Go to your Subscription's page and click on `Pipelines`
* Click on the `+` button in the upper right corner
* You will first need to select a subscription integration. This should point to the source control system where the repository containing your pipeline definitions is located. To learn how to create subcription integrations for source control, read the [SCM section of integrations overview page](../../integrations/overview.md#scm).
* Once you add the integration, you will see a list of repositories in your subscription.
* Select the repository and branch that contains your shippable.jobs.yml and shippable.resources.yml files
* Name your sync repository and click on `Save`. This should seed your pipeline.

Once you have added a syncRepo, you will see a visualization of the the jobs and resources from your `shippable.jobs.yml` and your `shippable.resources.yml`.

## Adding a syncRepo through the yml
You will always need to add at least one syncRepo through the UI. Subsequent syncRepos can be either added through the UI or you can include them in the `shippable.resources.yml`of the first syncRepo:

```
resources:
  - name: <string>                           	#required
    type: syncRepo                            	#required
    integration: <string>                    	#required, source control integration
    pointer:
      sourceName: org/repo                 	#required
      branch: <string>                          #optional, default master
```

* `name` should be an easy to remember text string. This will appear in the visualization of this resource in the SPOG view. It is also used to refer to this resource in the jobs yml. If you have spaces in your name, you'll need to surround the value with quotes, however, as a best practice we recommend not including spaces in your names.

* `type` is always set to 'syncRepo'.

* `integration` should be the name of the integration that connects to the Source Control provider where the repository is located. To learn how to create integrations for a specific Source Control Provider, please select from the list below and read the **Adding an Integration** section on that page:

	- [GitHub](../../integrations/scm/github/)
	- [Bitbucket](../../integrations/scm/bitbucket/)
	- [Github Enterprise](../../integrations/scm/githubEnterprise/)
	- [Bitbucket Server (formerly Stash)](../../integrations/scm/bitbucketServer/)
	- [Gitlab/GitlabServer](../../integrations/scm/gitlab/)


* `pointer` section provides information about the repository and branch where the configuration files are located.
	* `sourceName` is the fully qualified name of the repository in the format **org/repo**
	* `branch` specifies the branch. If not specified, it defaults to `master`.
