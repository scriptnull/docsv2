page_title: Navigating Shippable's Home Page UI
page_description: Overview of Shippable's Home Page UI
page_keywords: User Interface, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc


#Home Page
Login to [Shippable](http://www.shippable.com) and you'll see the Home Page of the Shippable portal.

The Home Page gives you a summary of information across your subscriptions, projects and builds.

From anywhere within the portal, you can get to the Home Page either by clicking the 'Home' image on the top right hand navigation or the 'Home' image on the top left burger menu.

<img src="../images/home.png" alt="Shippable Home Page" style="width:700px;"/>

---

##Home Dashboard
The 'Dashboard' section on the Home Page shows you the builds being processed under the 'Inflight Runs' section and a summary of the latest builds of all the projects, across your subscriptions that your account has access.

By default, only the latest builds from Master branches are displayed.

To change this default setting and include branches in the summary display, go to the
[Project's Settings page](projects/settings/#dashboard-settings) and select the
branch to be displayed.

I'll use the example build '85 Shippable-Demo/sample_nodejs' from the picture shown above to explain the details included in the 'Home Dashboard' summary:

- Build status icon: Displays the status of the build - Success, failed, etc.
Hover the mouse on the icon to get a pop up about the build status. For a complete
list refer the [builds status page](builds/overview/#build-status). In the above
example, it shows 'Success'
- Build number: Displays the latest build number. In the above example, it shows '85'
- Project: Displays the name of the project and the Subscription it belongs to.
In the above example, it shows 'Shippable-Demo/sample_nodejs' where 'Shippable-Demo'
is the subscription and 'sample_nodejs' is the name of the project.
- Branch name: Displays the name of the branch for the project. By default, it
is 'master'. To include other branches in this summary, go to the
[Project's Settings page](projects/settings/#dashboard-settings) and select the
branch to be displayed. In the above example, it shows the 'master' branch.
- Build Date: Displays the date the build was run. Hover the mouse on the date to
get additional information such as day and time. In the above example, it shows
'Last Wednesday at 11:03 AM'. Hovering the mouse over it shows July 13, 2016 11:03 AM.
NOTE: The dates are shown in the MM/DD/YYYY format.
- Triggered by: Displays the account name (from the source control system) that
triggered the build. In the above example, it shows 'Shippable-Demo'.
- Secure Hash Algorithm (SHA): Displays the unique ID of the commit. You can click
on the SHA to directly access the commit on the source control system. In the
above example, it is 41e820fe07.
- Commit message: Displays the commit message. In the above example, it is
'changed on_success to always for slack'.

---

##Metrics
*Coming soon!*

---
##Top right navigation
<img src="../images/homeRightMenu.png" alt="Shippable Top Right Navigation Menu" style="width:200px;"/>

The top right navigation menu includes the following:

- Home Page icon: Clicking this from anywhere within the Shippable portal will
get you to the Home Page.
- Documentation icon: Takes you to Shippable's [Documentation](http://docs.shippable.com)
- Service Status icon: Takes you to Shippable's [Status page](http://status.shippable.com/).
Here the service performance of Shippable's API, Website, Source Control providers
and 3rd party integrations are monitored. This page is also used to notify service
updates, outages and scheduled maintenance. You can subscribe to email/Atom/RSS
updates for this page.
- Support icon: Link to Shippable's [support repository](https://github.com/Shippable/support/issues)
on GitHub. Use this link to view all support issues, log new ones and get help
from Shippable's support team.
- Account Settings icon: Takes you to [account settings](/navigatingUI/accountSettings/accounts/).
- Log out icon: Use this link to log out of Shippable portal.

---
