page_title: Shippable CI/CD Dashboard
page_description: Explanation of the CI/CD Dashboard
page_keywords: ci/cd dashboard, subscription settings, CI/CD, shippable CI/CD, documentation, shippable, config, yml

# Projects
A project on Shippable CI corresponds to a repository in your source control that you have enabled for CI. To learn how to enable a repository, check out the ["Enable a project"](ci_subscriptions/#enabling-a-project) section.

To get to the page for a particular project,

- Login to [Shippable](https://app.shippable.com)
- Click on the 'Burger bar' dropdown on the top left and select the subscription you want to view.
- This will bring you to Dashboard of the subscription which shows latest status for all your projects.
- Click on the project name to view the Project page. This will take you to the project dashboard.

In the example below, 'sample_nodejs' project is selected. 

<img src="../images/ci_projects_mv_ci_dashboard.gif" alt="Project Dashboard" style="width:700px;"/>

* * * 

## Dashboard
The Project Dashboard shows you the latest status of all branches of the project. You can also view in progress builds and build history. You can get here by clicking on a project from the Shippable Home Page or Subscriptions page.

An example of the Project 'sampleNod' is shown below:

<img src="../images/project_home_page.png" alt="Project Page" style="width:700px;"/>


The **Dashboard View** is the default view when you navigate to this dashboard. It shows you the latest status for all branches that are built using Shippable. Latest status is defined as the build status for the latest commit build for that branch.

If a build is queued or in progress, you will see at the top of the 'Dashboard' view.

NOTE: To view time stamp for a build, hover the mouse over time details

The **History View** shows the build history across all branches in the project. You can filter this view by commit vs pull request builds, build status, and branch name.


### Manually building a branch
You can trigger manual builds for branch by clicking on `Build` for the branch in the **Dashboard View** of the project dashboard. If you cannot see the branch you want to build, check the `Show all branches` checkbox which will show all branches irrespective of whether they configured to be shown in `Dashboard Settings`.

To rebuild a previous build, go to the **History view** and click on `Rebuild` for any build listed there.

### Deleting builds

You can **Delete builds** by clicking on the `Delete builds` button, checking the builds you want to delete, and clicking on `Delete`.

* * * 

## Settings
You can perform project level actions by clicking on the `Settings` tab on the Project page as shown below:

<img src="../images/Project_settings.png" alt="Project Settings" style="width:700px;"/>

### Options
#### Syncing your project
We sync your account with your source control provider once every 4 hours. However, there are times when you want to force a sync in order to see recently changes that were made in your source control. You can force a sync by clicking on the `Sync` button in the Sync section of the Project settings page.
This action simply makes sure the permissions and repository changes from your source control are reflected in your Shippable project. Here are few scenarios where you will want to sync your project:

- A new branch has been created on your GitHub/Bitbucket repository and you don't find it on Shippable
- The Project's name or language has been changed and is not reflected on Shippable


#### Pausing your project
You can 'Pause' a project to stop triggering builds for this project. This means Shippable will stop listening to all code commits and pull requests on your source control repository and no action will be taken.

You can 'Resume' a paused project, which will re-establish the web hooks to your source control repository. This will enable Shippable to trigger builds for code commits and pull requests.

#### Clearing cache
You can clear cache for your project by clicking on the `Clear cache` button. This will clean up all cached Docker images from your build hosts and the next build will pull the build image again.

#### Resetting the project
Resetting a project recreates all webhooks and deployment keys for your project. This should only be done if your project is in an inconsistent state and you need to restore it. Please note that you will need to re-encrypt all environment variables for your project after resetting it.

#### Deleting the project
You can delete your project by clicking on the `Delete` button and then clicking on `Confirm`. Please note that deleting a project will delete all build history and delete all webhooks.

Deleting a project has no effect on the repository in your source control.


### Runs Config

#### Dashboard Settings
By default, we show a project's status on the Subscription and Landing pages with the following logic:

- Show status of latest commit build of default branch configured in source control repo
- If no default branch, show latest commit build for master branch
- If no master branch, show status of the latest build across branches.

Builds triggered for pull requests are not considered while determining status of a branch.

You can override this default behavior in the Dashboard Settings section by customizing the following:

- **Branches to be displayed**: choose the branch(es) that you want displayed on the Subscription and Landing pages.
- **Run types to be displayed**: Specify whether you want the computation for project status to include builds triggered by pull requests in addition to ones triggered by a commit..

For example, the configuration below will show status of the latest build for branches master and revert-68-master, irrespective of whether it was triggered by a commit or a pull request..

<img src="../images/projects_settings_dashboard.png" alt="Project Settings Dashboard configuration" style="width:700px;"/>

#### Configure Webhook Events for Triggering Builds
In this section, you can enable or disable triggering of builds for your project, based on specific webhook events on your source control system.

- **Pull Requests**: Default value is Enabled. Every pull request initiated on the project in your source control system, triggers a build on Shippable. Click 'Disable' to stop the triggering of builds for this project.
- **Commits**: Default value is Enabled. Every commit initiated on the project in your source control system, triggers a build on Shippable. Click 'Disable' to stop the triggering of builds for this project.
- **Tags**: Default value is Disabled. To trigger builds for all git tag push events on the project in your source control system, click 'Enable'. Currently this feature is supported only for GitHub, Gitlab, Bitbucket and BitbucketServer.
- **Releases**: Default value is Disabled. To trigger builds for all GitHub release events on the project in your source control system, click 'Enable'. Currently this feature is supported only for GitHub.

NOTE: The above webhook events, when enabled, trigger builds on Shippable irrespective of the user who initiated the event on the source control system.


<img src="../images/configure_build_triggers.png" alt="Configure Webhook events to trigger builds" style="width:700px;"/>

Walk through the different scenarios and [configure webhooks to trigger Continuous Integration builds](http://blog.shippable.com/configure-web-hooks-to-trigger-continuous-integration).  

#### Run Parallel Jobs
Disabling parallel jobs allows you to restrict job processing based on branch name.  When this setting is disabled, any waiting job will not begin until all processing jobs have completed.

You can further customize this by selecting the specific branches that you don't want running in parallel.

<img src="../images/run_parallel_jobs.png" alt="Run Parallel Jobs" style="width:700px;"/>

In this example, if jobs are triggered simultaneously for branches `master` and `prtest`, only one job will be allowed to run at a time, even if the subscription has two idle nodes.  However, if this project were to trigger a job on a branch that was not selected, it would start as soon as it finds  an available node. The serial jobs will run in the order that they were queued.


#### Custom Timeout
By default, builds that are running for over 60 minutes (for free accounts) or over 120 minutes (for paid accounts) will time out. You can configure the timeout setting between 1 and 60/120 minutes for your project.

- Click 'Edit'
- Input a number between 1 and 60 (for free accounts) or 1 and 120 minutes (for paid accounts)
- Click 'Save'
- Once you set a custom timeout, it'll be in effect immediately
- Click on 'Clear' to revert back to the default setting of 120 minutes

For more details refer to our blog - [Changing the default time for a Continuous Integration projet](http://blog.shippable.com/changing-the-default-timeout-for-a-continuous-integration-project).

### Encrypt
#### Encrypting your environment variables
Shippable allows you to encrypt your environment variable definitions and keep your configurations private in your shippable.yml by using the `secure` tag.

To encrypt a variable, enter the environment variable and its values in the text box as shown below and click on `Encrypt`-

```
name="abc"
```
To encrypt multiple variables, you can use the following syntax-

```
var1="abc" var2="xyz"
```
You can now use these encrypted variables in your shippable.yml with a secure tag . For example,

```
env:
  secure: <encrypted output>
```

### Badges

#### Status badges
You can get embeddable build status badges for your project. These provide a visual indication of project status.

Select the branch you want the status for and also whether you want the 'Image URL' or 'Markdown' options. Copy the embedded code.

You can use this code anywhere you want to display the badge. For example, many customers  display this badge in the README.md of their project so anyone who goes to the project page on GitHub/Bitbucket can see it and know the project's status.  

To learn more about practical examples, check out our blog - [Configuring a build badge for a node.js project status](http://blog.shippable.com/configuring-a-visual-indicator-for-a-node.js-project-status).

#### Code coverage badges
Similar to the build status badge, you can get embedded code coverage badges for your project, to provide visualization of the code coverage status for a repository.

Select the branch you want the status for and pick between the 'Image URL' or 'Markdown' options. Copy the embedded code.

You can use this code to display your code coverage report after code coverage runs. The badge displays appropriate colors depending on the code coverage configuration in the `shippable.yml`. Refer the  [documentation](http://docs.shippable.com/ci_configure/#test_code_coverage) on how to configure code coverage reports. In addition, refer to a step by step blog on [setting up code coverage for tests in Continuous Integration](http://blog.shippable.com/setting-up-code-coverage-visualization-for-tests-in-ci).

### Integrations
To add an integration to your project, go to the Subscription's 'Settings'. [Click here](ci_subscriptions/#integrations) for instructions. 

---
