page_title: Bitbucket Server integration with Shippable
page_description: How to integrate Bitbucket Server with Shippable
page_keywords: source control management system, continuous integration, continuous delivery, pipelines, git, GitHub, GitHub Enterprise, Bitbucket, GitLab

# Bitbucket Server

We support Bitbucket Server in 2 flavors:

* Using Shippable Hosted, you can sign in to Shippable with your GitHub or Bitbucket credentials, and then add an integration to your account that enables you to build repositories hosted on your Bitbucket Server instance. This is described in the sections below.

* You can also buy Shippable Server and run it on-premises or behind your firewall in your private cloud. In this setup, we support authenticating against Bitbucket Server directly. If you are interested in this model, please send us an email to our [Sales team](mailto:sales@shippable.com)

---

##Adding a Bitbucket Server integration

To add an integration for Bitbucket Server, you will first need to sign in with GitHub or Bitbucket credentials.

Adding an integration is achieved as follows:

##### Add an account integration

* Go to your **Account Settings** by clicking on the gear menu in the top navbar.
* Click on **Integrations** in the sidebar menu.
* Click on **Add Integration**.
* Enter the following:
	* In the **Master Integration** dropdown, choose **Bitbucket Server**
	* Add a friendly name for your integration
	* Enter the URL for your Bitbucket Server instance. **Ensure there are no trailing slashes here**. Read the [FAQ](../../ci/faq/#i-cannot-start-a-manual-build-for-my-bitbucket-project-why-is-it-not-working) on this topic for more details.
	* Get your **Bitbucket Server private token**
		* Go to your Bitbucket Server account settings and in the left menu, select
    `Shippable Token`
    	* Copy the private token provided
		* Paste it in the **Token** textbox in Shippable
* Click on **Save**. You should now see the integration in your list of integrations.

#####Set up CI for your repositories
If you don't want to use your integration to enable repositories for CI, skip this section and go to the next.

* In your Account Settings, click on `Sync`.
* Go to your Home dashboard. Click on the Subscription dropdown on the top left of your screen.
* You should see your Organizations from Bitbucket Server in your list of subscriptions.
* Click on any Bitbucket Server organization and you should be able to enable projects for [CI](../../ci/overview/).

#####Use your integration in your Pipeline configuration

* Go to your Subscription's Settings tab. This should be the Subscription where you want to set up your pipelines.
* Click on **Integrations** in the sidebar menu.
* Click on **Add Integration**.
* Name your Bitbucket Server integration with a friendly name. This can be the same name as the one in your account integration.
* From the dropdown, choose the account integration you just created in the step above.
* Click on **Save**.

The integration is now added to your subscription. You can now use this account integration while configuring your CI or pipeline workflows. In your configurations, **you should use the friendly name for the integration from Subscription Settings**.

For example, if the integration is called `bbs-foo` in your subscription settings, you can use it in `shippable.resources.yml` as follows:

```
- name: bbs-repository                     	 #required
  type: gitRepo                             	#required
  integration: bbs-foo                     	#this is your github integration name
  pointer:
    sourceName: org/repo                  		#required
    branch: master                          	#optional
```

---


##Bitbucket Server tutorial

[Using Shippable with Bitbucket Server](/tutorials/ci/scm-bitbucket-server-ci/)

---
