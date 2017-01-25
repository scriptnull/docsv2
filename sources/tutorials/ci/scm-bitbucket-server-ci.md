page_title: Using Shippable with Bitbucket Server
page_description: This section explains how to use Shippable with Bitbucket Server
page_keywords: getting started, questions, documentation, shippable, bitbucket-server, source control, bitbucket

# Using Shippable with Bitbucket Server

Several software development teams use Bitbucket Server to host their repositories. Shippable supports running Continuous Integration (CI) builds for Bitbucket Server. This tutorial walks you through using Shippable with Bitbucket Server successfully.

The tutorial is split up into 4 sections:

1. Install the Shippable add-on for Bitbucket Server
2. Enable Shippable access to Bitbucket Server for a user
3. Sync subscriptions
4. Running CI

To set this up, follow the steps mentioned here **UpdateLink** to add Bitbucket Server as
an Account Integration and sync your Subscriptions. Once complete, you can enable a project/repository and setup a `shippable.yml` file to get your builds running.

---

## Install the Shippable add-on for Bitbucket Server
This step is a one-time action that needs administrator access on the Bitbucket Server. The steps to add the Shippable add-on for the first time set up are:

- Navigate to the [Atlassian Marketplace](https://marketplace.atlassian.com/plugins/shippable/cloud/overview) and download the Shippable add-on (.jar file) to your computer.
- Log in with administrator access to your Bitbucket Server.
- Navigate to the Administrator page (gear icon on the upper right navigation bar).
<img src="../images/ht_bbs_admin_settings.png" alt="Bitbucket Server Admin Settings" style="width:500px;"/>

- Scroll down and click the 'Manage Add-ons' section.
<img src="../images/ht_bbs_manage_addons.png" alt="Bitbucket Server Admin Settings" style="width:500px;"/>

- Select 'Upload add-on'.
<img src="../images/ht_bbs_upload_addon.png" alt="Bitbucket Server Admin Settings" style="width:700px;"/>

- Choose the shippable-integration-1.0.0.jar file that was downloaded to your computer in the first step and upload it. You'll see a similar image shown below.
<img src="../images/ht_bbs_shippable_jar.png" alt="Bitbucket Server Admin Settings" style="width:700px;"/>

## Enable Shippable access to Bitbucket Server for a user
Let's enable Shippable access to the repositories on Bitbucket Server. Currently, every user running builds on Shippable for repositories hosted on Bitbucket Server, needs to login to Shippable using their GitHub or Bitbucket credentials. NOTE: The following steps need a user account on Bitbucket Server. Admininistration access is not required.

- Log into Shippable using either GitHub or Bitbucket Cloud credentials
- Navigate to the Account Settings (gear in the upper right navigation bar)
- Select the 'Integrations' tab and click 'Add Integration' on the right
- Scroll the dropdown choices under 'Master Integration' and select 'Bitbucket Server'
- Enter the requested information:
    - **Name**: A text-based name for the integration (best practice is to avoid spaces)
    - **Username**: Your Bitbucket Server username
    - **URL**: Your Bitbucket Server url
    - **Token**: Your Bitbucket Server Shippable token
- To find your Bitbucket Server Shippable token:
    - Navigate to 'Manage Account' on the Bitbucket Server portal
    - Select 'Shippable Token' from the left navigation menu
    - Copy the Shippable Auth Token
    - Paste it in the 'Token' field in the Account Integration step on Shippable portal.

<img src="../images/ht_bbs_mv_shippable_int.gif" alt="Bitbucket Server Admin Settings" style="width:700px;"/>

## Sync Subscriptions
- Navigate to the Account Settings (gear in the upper right navigation bar)
- Select the 'Accounts' tab and click the 'Sync' button.

<img src="../images/account_settings_sync.png" alt="Bitbucket Server Admin Settings" style="width:700px;"/>

- Navigate to your list of subscriptions and you should now see your Bitbucket Server projects listed and available to enable.

## Running Continuous Integration

Builds are automatically triggered every time you commit code for an enabled project on Shippable. The Shippable addon for Bitbucket Server is used to setup the required web-hooks and deploy keys.
