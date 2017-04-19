page_title: Navigating Shippable's Subscriptions CI UI
page_description: Overview of Shippable's Subscriptions CI UI
page_keywords: User Interface, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc


#Subscriptions
A subscription on Shippable corresponds to an individual or organizational subscription on GitHub/Bitbucket.

Your pricing plans are enforced at this level, so you need to determine your minion needs for each subscription, you can also see how many minions are being used at the current time by looking at the **Minion usage** count on the top right of the page.

The Subscriptions page consists of 4 tabs:

1. CI (Continuous Integration) tab
2. Infra tab
3. Pipelines tab
4. Settings tab

---

#CI tab

The `CI` tab of your Subscription page shows you the latest status of all enabled projects.

You can also view in progress builds and build history.

To get to the CI view,

- Login to [Shippable](https://app.shippable.com)
- Click on the `Subscriptions` dropdown and select the subscription you want to
view.
- This will bring you to the CI tab which shows you the latest status for your
projects.

<img src="../../images/subscriptions/dashboardMv.gif" alt="Continuous Integration
Dashboard" style="width:700px;"/>

---

##Dashboard section
The 'Dashboard' section is the default view when you navigate to this page.

By default, you will see the status of the most recent commit builds for all enabled
projects.

The default branch of the repository, as configured in your source control account, is used.

You can customize the branch(es) shown on this page and also configure this view
to include status for pull request builds.

More details on this in the [Dashboard Settings for Projects](/navigatingUI/projects/settings/#Dashboard-Settings).

If a build is queued or in progress, you will see at the top of the 'Dashboard'
view.

Use the 'Filters' dropdown to select specific statuses of the builds to be
displayed.

By default, you will see all statuses of the builds.

<img src="../../images/subscriptions/dashboard.png" alt="Subscription Dashboard"
style="width:700px;"/>

I'll use the example build '86 sample_nodejs' from the picture shown above to
explain the details included in the 'Subscription Dashboard' summary:

- Build status icon: Displays the status of the build - Success, failed, etc.
Hover the mouse on the icon to get a pop up about the build status. For a complete
list refer the [builds status page](builds/overview/#Build-status). In the above
example, it shows 'Success'
- Build number: Displays the latest build number. In the above example, it shows '86
- Project: Displays the name of the project. In the above example, it shows
`sample_nodejs` within your subscription.
- Branch name: Displays the name of the branch for the project. By default, it
is 'master'. To include other branches in this summary, go to the
[Project's Settings page](navigatingUI/projects/settings/#Dashboard-Settings) and
select the branch to be displayed. In the above example, it shows the 'master' branch.
- Build Date: Displays the date the build was run. Hover the mouse on the date
to get additional information such as day and time. In the above example, it shows
'Today at 1:58 PM'. NOTE: The dates are shown in the MM/DD/YYYY format.
- Build Time: Displays the time taken for the build to run. Hover the mouse on
the time to get the exact timestamp in seconds. In the above example, it shows '4 minutes'.
- Triggered by: Displays the account name (from the source control system) that
triggered the build. In the above example, it shows 'Shippable-Demo'.
- Secure Hash Algorithm (SHA): Displays the unique ID of the commit. You can
click on the SHA to directly access the commit on the source control system. In
the above example, it is 41e820fe07.
- Commit message: Displays the commit message. In the above example, it is
'changed...'. Hover the mouse over the commit message to get the full sentence.

NOTE:
- You can trigger manual builds for an enabled project by clicking on `Build` for
a project.

---

##History tab
The **History** tab opens a global runs search modal with filters preset to the current subscription, and you can initially see all runs for all of the enabled projects of your subscription.

Filters are available to select any of your subscriptions/ projects/ branches/ statuses/ run types to further refine your search, this gives you easy and quick access to all of your runs.

- Rebuild a previous run, by clicking on `Rebuild`.
- Delete specific runs, by clicking the `Delete Runs` button. Select the build
to be deleted and click the `Delete` button.
- Use the `More` button on the bottom of the list to load and display more runs
from your history.

<img src="../../images/subscriptions/history.png" alt="Subscription History"
style="width:700px;"/>

---

##Enable Project
The 'Enable Project' section shows all the repositories (from your source control
system) that are yet to be enabled as projects on the Shippable platform.

You can enable projects and trigger manual builds from this page.

You can enable a project for CI by doing the following:

- Click on the 'Enable Project' section on your Subscription page. This brings
you to the page with the list of repositories that are not yet enabled within
your subscription.
- Click on the `Enable` button to enable a particular repository. If you have a
long list of repositories, pull it up by typing the name of the repo to filter
by name and enable it.
- If you have recently added a repository to github/bitbucket and do not see it
in the list, click on the Sync button. Once the sync is complete, you should see
the new repo in the list, for you to enable.

After enabling a project you will be redirected to the project's page on Shippable.

More details on this are in our [Projects section](/navigatingUI/projects/status).

<img src="../../images/subscriptions/enableProjectMv.gif" alt="Subscription
Dashboard" style="width:700px;"/>

---
