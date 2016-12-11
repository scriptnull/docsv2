page_title: Shippable Account Settings
page_description: Account Settings, Integratons, Account Tokens, Credit Cards
page_keywords: containers, lxc, docker, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, Tokens, account settings

#Seeding your first pipeline

A source control repository that contains your pipeline configuration files is called a **Sync Repository** and can only contain one each of the `shippable.jobs.yml`, `shippable.resources.yml`, and `shippable.triggers.yml` files.  

You must seed your pipeline with at least one sync repository through the Shippable UI. Subsequent sync repositories can also be added through the UI following the same process. Instructions are in the section below.

You may have entire pipeline configurations maintained in one repository or split up the configuration of different sections of your pipeline in multiple sync repositories. This decision depends on your organizational preferences, as well as [security and permissions requirements](../../pipelines/gettingStarted/#permissions). For example, you may have pipeline configuration for source control through your first test environment in one repo, configuration for subsequent test environments in another repo, and configuration for your production environment in yet another repo. In this way, you can manage who can configure and execute different areas of your pipeline based on the permissions set on each repo.

---

##Adding a sync repository

You can add a sync repository by following the steps below:

* First, add a subscription integration for the source control provider where your sync repository is located. Instructions are here - [Source Control Provider Integrations](/integrations/scm/scmOverview/).
* Go to your Organization's page on Shippable. A list of all available Organizations can be accessed by clicking on the  <i class="fa fa-bars" aria-hidden="true"></i>  icon.
* Click on the `Pipelines` tab
* If you have never added a sync repository, you will land on the `Resources` option in the pill menu. If you have added a sync repository in the past, you will need to click on `Resources` in the pill menu.
* Click on `Add Resource`.
* Complete the Add Resource fields:
	* The subscription integration dropdown should show the subscription you created in the first step. If not, you will need to go through the flow of adding the integration.
	* The `Select Project` dropdown will show all repositories in the source control you just connected with the integration. Choose your sync repository.
	* Select the branch of the sync repository that contains your pipeline configuration files.
	* Name your sync repository with an easy to remember name.
* Click on `Save` to apply your sync repository configuration.

You will now see your configured pipelines created on Shippable. Click on `SPOG` in the pill menu to view your pipelines. Note, if you do not see what you expected, you likely have a configuration error. Click on the rSync resource in the SPOG view to see the console and identify any errors that may exist.

---
