page_title: Navigating Shippable's Builds UI
page_description: Overview of Shippable's Builds UI
page_keywords: User Interface, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# Builds

The Builds page shows you information about a specific build(s) in a project.

To get to a specific build page:

- Login to [Shippable](https://app.shippable.com)
- Click on the CI dropdown and select the subscription you want to view.
- This will bring you to Dashboard of the subscription which shows latest status
for all your projects.
- Click on the project name to view the Project page. This will take you to the
project dashboard.
- Click on any build number in the Summary or History tabs to get to the Build page

Additional information about what is shown on this page is given below.

To learn how to configure your build, check out our [Build configuration section](/ci/shippableyml).

<img src="../../images/builds/dashboardMv.gif" alt="Build Information" style="width:700px;"/>

---

## Build information
The build page shows all relevant information like status, branch name, commit message, committer, etc.

A build on Shippable can actually consist of one or more individual build items, depending on your yml configuration.

Configurations resulting in a [matrix build](/ci/advancedOptions/matrixBuilds/) will have more than one build items as part of the build.

The build status widget for the overall build looks like this:

<img src="../../images/builds/status.png" alt="Build status" style="width:700px;"/>

The breadcrumb shows you the Subscription name and project name. The actual widget
provides the following information:

- A circle indicating build status ('Success' in the screenshot above)
- Build number ('79' in the screenshot above)
- Commit message ('removed slack completely' in the screenshot above)
- Branch name ('master' in the screenshot above)
- Language used ('node_js' in the screenshot above)
- Commit SHA from your source control system ('99f4e3cc82' in the screenshot above.
You can go to the commit page on the source control system by clicking on the SHA.)
- Triggered by builder who committed the code change ('Shippable-Demo' in the
screenshot above)
- When the build was started ('Yesterday at 5:03 PM' in the screenshot above)
- How long it took for the build to complete ('7 minutes' in the screenshot above)
- How long it was in the queue waiting for an available minion ('a few seconds'
in the screenshot above)
- Tests Results, if Test coverage is configured ('All Passed' in the screenshot
above)

## Build status

The circle at the left of the widget shows a color coded status with the following
values when you hover the mouse on it:

### Build status definitions:
Complete States

- ![add_icon](/navigatingUI/images/builds/buildSuccess.png) **success**: The build has successfully
completed with no failed tests in a build or in any job in a matrix build. The
statusCode in API for this state is 30.
- ![add_icon](/navigatingUI/images/builds/buildUnstable.png) **unstable**: State when a job ends
successfully (meaning it returned an exit code of 0), but there are one or more
failed tests. If at least one job in a matrix is unstable and all the other jobs
are successful, the build will be marked as unstable. The statusCode in API for
this state is 50.
- ![add_icon](/navigatingUI/images/builds/buildTimeout.png) **timeout**: The build has timed out
prior to executing all the jobs. This occurs  when there is no log output or a
command hangs for over 10 minutes. It also occurs if your build is running for
over 60 minutes (for free accounts) or over 120 minutes (for paid accounts). The
statusCode in API for this state is 60.
- ![add_icon](/navigatingUI/images/builds/buildCancelled.png) **canceled**: State when a build
has been manually canceled, from the Shippable portal. The statusCode in API for
this state is 70.
- ![add_icon](/navigatingUI/images/builds/buildFailed.png) **failed**: State when there is at
least a single failure in a build or a single job in a matrix build that fails.
The errors in the build causing it to fail are listed in the console logs for the
exact step it failed. The statusCode in API for this state is 80.

Incomplete States:

- ![add_icon](/navigatingUI/images/builds/buildWaiting.png) **waiting**: A build that is waiting
for an available minion. The most probable reason you'll see this state is when
you have exceeded the number of concurrent builds your subscription is eligible for.
The statusCode in API for this state is 0.
- ![add_icon](/navigatingUI/images/builds/buildProcessing.png) **processing**: A build that is
still in progress and is executing the steps defined in shippable.yml file. The
statusCode in API for this state is 20.


The white icon inside the circle shows whether a commit or a pull request triggered
the build:

- ![add_icon](/navigatingUI/images/builds/buildCommit.png) for a build triggered by a commit
- ![add_icon](/navigatingUI/images/builds/buildPR.png) for a build triggered by a pull request


### Build number
To the right of the status circle is the build number. Build number starts at 1
for the first build of the project and increase by 1 for every build.

For matrix builds, the overall build is still numbered with a whole number and
individual build items under it are numbered using decimals like 11.1, 11.2, etc.

---
