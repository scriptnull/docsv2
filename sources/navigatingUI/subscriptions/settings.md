page_title: Navigating Shippable's Subscriptions Settings UI
page_description: Overview of Shippable's Subscriptions Settings UI
page_keywords: User Interface, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

#Subscription Settings
You can perform subscription level actions by clicking on the 'Settings' tab on the 'Subscription' page.

##Options
###Machine Images

Machine images are used to spin up a VM for your build.

The `Stable` machine image has been extensively tested and has served the needs of almost all customers for a very long time. However, this image is being retired and should not be used on newer projects. If you are currently using this image, please plan to migrate to a more recent image.

The `Unstable` machine image was introduced to enable scenarios that required a newer release of Docker. This image is also being retired and should not be used on newer projects. If you are currently using this image, please plan to migrate to a more recent image.

From release 5.3.2 onwards, Shippable will introduce new machine images every month with the latest tooling and official Docker images. The images are named after each release version (`v5.3.2`, `v5.4.1`, and so on). They are tested for all scenarios and are recommended for general use.

You are free to switch you subscription between machine images at any point in time.

For a complete list of what is installed on each machine image, read our [CI Machine images section.](/ci/advancedOptions/machineImages/)

**Machine image should not be confused with the actual build container image where your CI workflow is executed.** The CI container is spun up on the build VM and the actual build is run inside the CI container. This is described in the [CI overview](/ci/overview/#ci-workflow).


###Deployment key

The Deployment key section shows the SSH public key associated with your Shippable Account.

You will need this key to deploy to cloud providers that supports git based deployments like Heroku and Red Hat Openshift.

This key is also used to encrypt any environment variables that you want to use during your build.

Our How To guides provide instructions on how to enable continuous deployment to different providers.

###Technical Contact

Do ensure this field is populated with a valid email address for the subscription.

This email will be used to notify information on service upgrades, service performance and other related topics. It will not be used for marketing purposes.

###Billing Contact

Do ensure this field is populated with a valid email address for the subscription.

This email will be used to send billing invoices for this subscription.

###Reset Subscription

Resetting a subscription recreates all webhooks and deployment keys for your
subscription.

This should only be done if your subscription is in an inconsistent state and you need to restore it.

Please note that you will need to re-encrypt all environment variables for your subscription after resetting it.

<img src="../../images/subscriptions/reset.png" alt="Resetting for a Subscription"
style="width:700px;"/>

---

##Integrations

Integrations are added at a subscription level and they are available to be configured for a project by any user, with access to the subscription level.

For example, if an integration is added to an organization subscription, all users with access to the organization's subscription can configure the integration for their projects.

You can view all the configured integrations for your account by going to the 'Account Settings' and clicking the 'Integrations' section.

It will display a list of all integrations that are configured and available for your projects.

###Adding integrations

To add an integration for a project, you'll need to add it in the UI for the project's subscription and include it in the `shippable.yml` file for that project.

Add the integration to a subscription, in the UI, through **Subscription Settings**:

1. Go to the 'Settings' tab of your subscription and click the 'Integrations' section on the left side menu.

2. Click 'Add Integration' and select the list of integrations available in the dropdown of the 'Account Integrations' field.
<img src="../../images/subscriptions/addIntegration.png" alt="Add Integration to a
Subscription" style="width:700px;"/>

3. Ensure the 'Integration Name' you input is exactly the same as the one you
configure in the `shippable.yml` file
<img src="../../images/subscriptions/addIntegration2.png" alt="Add Integration to a
Subscription" style="width:700px;"/>


NOTE: If you don't see an integration in the dropdown, you will need to create
one. To do so:

- Click on the '+ Add integration' from the dropdown choices of the 'Account Integrations'
field as shown above in Step 3.
- Create the integration. [Detailed instructions here](../accountSettings/integrations/#Adding-an-account-integration).
- Once you create this integration, add it to  your subscription by following
Steps 2 and 3.

**IMPORTANT**: After adding the 'Integrations' in the UI, you'll need to configure the same in the `shippable.yml` file for the project.

More details on [yml config here](/ci/shippableyml/).

The same is true for any Docker registry integrations.

---

##Encrypt
Shippable allows you to encrypt your environment variable definitions and keep your configurations private in your yml config files by using the secure tag.

To encrypt a variable, enter the environment variable and its values in the text box as shown below and click on Encrypt-

```
name="abc"
```

You can now use these encrypted values with a secure tag . For example, here is the shippable.yml syntax:

```
#shippable.yml syntax
env:
  - secure: <encrypted output>
```

Here is the shippable.resources.yml syntax:

```
#shippable.resources. yml syntax
resources:
  - name: paran_name                              
    type: params                                
    version:
      params:                                 
        secure: <encrypted value>           
```

###Encrypting multiple variables
To encrypt multiple variables for the shippable.yml for a CI project, you can use the following syntax-

```
var1="abc" var2="xyz"
```
Copy the encrypted value and use it in your yml.

To encrypt multiple variables for a shippable.resources.yml for Pipelines, you can use the following syntax-

```
var1=abc var2=xyz
```
Copy the encrypted value and use it in your yml.

Please note that the difference in syntax for multiple vars in CI and Pipelines is a bug and we will address it soon.

---

## Billing
A subscription on Shippable corresponds to an individual or organizational subscription
on GitHub/Bitbucket.

Your pricing plans are enforced at a subscription level, so you need to determine your build minion and pipeline needs for each subscription.

To get to the Subscription Billing page:

- Login to [Shippable](http://www.shippable.com).
- Click on the `Subscriptions` dropdown and select the subscription you want to view.
- Click on the Billing tab to view and manage your plan.

By default, you are subscribed to the 'Continuous Delivery' free plan.

It includes:

- One free CI build container (2 core, 3.75GB RAM) that supports unlimited public
and private repositories for unlimited users from your source control provider.
You can run as many builds as you want.
- One free Pipeline, that allows you to set up deployment for one Cell.

<img src="../../images/subscriptions/billing.png" alt="Subscription Plan" style="width:700px;"/>

###Upgrade your plan

You should buy more build minions if:

- Your builds need bigger minions than the one in your free plan. All paid minions
come with 2 cores, 3.75GB RAM
- Your team is growing and your builds are frequently queued as a result, which
means your developers wait longer to get build results. Buying more minions will
enable parallel execution of builds and reduce queuing time.

You should buy more pipelines if your application has more than 1 unit of deployment.
These can be tiers, services, or microservice.

Read more about [when and how to upgrade your subscription](http://blog.shippable.com/how-to-upgrade-your-ci-cd-subscription).

To buy more build minions or pipelines, simply slide the slider to the number of minions and/or pipelines you need.

Choose a credit card, or Enter a new credit card and click on Buy.

We will charge your credit card immediately and send you an invoice. You can also
view past invoices on this page.

###Downgrade your plan

You can downgrade your plan at any time by moving the slider to the number of minions and/or pipelines you need.

Please note that any minion count changes due to your plan downgrade will be effective immediately and you will not receive a partial or prorated refund if you make this change in the middle of a billing cycle.

Your new price will be reflected in your next invoice.

---

## Nodes

This page lets you configure the Bring Your Own Nodes (BYON) feature.

By default, all your builds run inside build containers hosted on Shippable's infrastructure.

However, you can choose to run your builds on your own infrastructure, i.e. you can '**Bring Your Own Node (BYON)**'. This option is great for organizations whose security policies do not allow them to run builds on third party hosted infrastructure, and also for those who want faster builds with no node spin up time and docker caching.

BYON gives you all the flexibility and simplicity of using a SaaS service for CI orchestration, while still giving you full control over the infrastructure and security of your build machines.

To understand the advantages of BYON and learn how to configure this feature, check out the [Running builds on your machines section](../../ci/advancedOptions/byon/).

---
