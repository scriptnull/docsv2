page_title: Navigating Shippable's Projects Status UI
page_description: Overview of Shippable's Projects Status UI
page_keywords: User Interface, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

#Projects Status tab
A project on Shippable CI corresponds to a repository in your source control that you have enabled for CI.

To learn how to enable a repository, check out the ["Enable Project"](/navigatingUI/subscriptions/ci/#enable-project) section.

To get to the Projects Status page for a particular project,

- Login to [Shippable](https://app.shippable.com)
- Click on the 'Burger bar' dropdown on the top left and select the subscription
you want to view.
- This will bring you to Dashboard of the subscription which shows latest status
for all your projects.
- Click on the project name to view the Project page. This will take you to the
project dashboard.

In the example below, 'sample_nodejs' project is selected.

<img src="../../images/projects/dashboardMv.gif" alt="Project Dashboard" style="width:700px;"/>

---

##Project Dashboard section
The Project Dashboard shows you the latest status of all branches of the project.

You can also view in progress builds and build history.

You can get here by clicking on a project from the Shippable 'Home Page' or 'Subscriptions' page.

The **Dashboard View** is the default view when you navigate to this dashboard.

It shows you the latest status for all branches that are built using Shippable.

Latest status is defined as the build status for the latest commit build for that branch.

If a build is queued or in progress, you will see at the top of the 'Dashboard' view.

Use the 'Filters' dropdown to select specific statuses of the builds to be displayed.

By default, you will see all statuses of the builds.

<img src="../../images/projects/home.png" alt="Project Page" style="width:700px;"/>

I'll use the example build '79 for the 'sample_nodejs' project from the picture shown above to explain the details included in the 'Project Dashboard' summary:

- Build status icon: Displays the status of the build - Success, failed, etc.
Hover the mouse on the icon to get a pop up about the build status. For a complete
list refer the [builds status page](builds/overview/#build-status). In the above
example, it shows 'Success'
- Build number: Displays the latest build number. In the above example, it shows '79'
- Branch name: Displays the name of the branch for the project. By default, it is
'master'. To include other branches in this summary, go to the
[Project's Settings page](/NavigatingUI/projects/settings/#dashboard-settings)
and select the branch to be displayed. In the above example, it shows the 'master'
and the 'sample_nodejs_branch' branch.
- Build Date: Displays the date the build was run. Hover the mouse on the date to
get additional information such as day and time. In the above example, it shows
'Yesterday at 5:10PM'. NOTE: The dates are shown in the MM/DD/YYYY format.
- Build Time: Displays the time taken for the build to run. Hover the mouse on
the time to get the exact timestamp in seconds. In the above example, it shows
'7 minutes'.
- Triggered by: Displays the account name (from the source control system) that
triggered the build. In the above example, it shows 'Shippable-Demo'.
- Secure Hash Algorithm (SHA): Displays the unique ID of the commit. You can click
on the SHA to directly access the commit on the source control system. In the above
example, it is 99f4e3cc82.
- Commit message: Displays the commit message. In the above example, it is
'removed slack completely'. Hover the mouse over the commit message to get the
full sentence.

### Manually building a branch
You can trigger manual builds for branch by clicking on `Build` button for the branch in the **Dashboard View** of the project dashboard.

If you cannot see the branch you want to build, check the `Show all branches` checkbox which will show
all branches irrespective of whether they configured to be shown in `Dashboard Settings`.

---

##History section
The **History View** shows the build history across all branches in the project.

You can filter this view by build type (commit vs pull request builds), build statuses, and project names.

The default view is set to all build types, all build statuses and all projects.

- Rerun a previous build, by clicking on `Rerun` for any build listed here.
- Delete specific builds/runs, by clicking the `Delete Runs` button. Select the
build to be deleted and click the `Delete` button.
- Use the `More` button on the bottom of the page to load and display more builds
from your history.

<img src="../../images/projects/history.png" alt="Project History" style="width:700px;"/>

---
