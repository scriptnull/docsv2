page_title: Notification provider integration
page_description: How to watch images using Shippable's Lighthouse Feature
page_keywords: lighthouse, shippable ci, documentation, shippable, watch docker images

# Notifications

We can notify you about your builds and deployment workflows through Slack, IRC, and Email. HipChat support is next on our list and will be available in Q1 of 2016.

To set up notifications via Slack, HipChat or private IRC channels, you will need to configure account integrations with your credentials or keys to these services.

---

## Slack notifications
You will need to configure this integration to send notifications to Slack channel(s). Notifications can be sent for build status updates, monitoring your Docker image updates, and deployment updates.

**Configuring an incoming webhook on Slack**

1. Sign in to your Slack account and [go to this link to create an incoming webbook](https://my.slack.com/services/new/incoming-webhook/).
2. It does not matter which channel you choose while creating the webhook. We will override it when you configure the integration on Shippable.

**Set up Slack integration on Shippable**

1. From your Shippable dashboard, click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` tab. Click on 'Add Integration'
2. **Integration type:** In the dropdown, select `Slack`
3. **Integration Name:** Use a distinctive name that's easy to recall. Example: `manishas-slack`
4. In the **URL** textbox, paste the webhook URL you created in the Slack UI above.
5. Click `Save`

You have now configured Slack and can use this integration during your CI and Pipelines settings.

Refer our blog on a stepwise guide to [notify failure/success status for your CI project, using Slack](http://blog.shippable.com/notifying-ci-failure/success-status-on-email-slack). 

---

## IRC notifications
You will need to configure this integration to send notifications to private IRC room(s). Public IRC channels can be configured without the need for an account integration.

1. From your Shippable dashboard, click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` tab. Click on 'Add Integration'
2. **Integration type:** In the dropdown, select `IRC`
3. **Integration Name:** Use a distinctive name that's easy to recall. Example: `manishas-irc-shippable`
4. Enter your channel key and if applicable, username and password.
5. Click `Save`

You have now configured your private IRC integration and can use this to send notifications during your CI and Pipelines settings.

---

## Email notifications

Build Emails are always sent through our Shippable account and are configured through shippable.yml. At this time, you cannot customize the domain emails are sent from. To request this enhancement, please contact [customer support](mailto:support@shippable.com)

To get console logs, coverage reports as attachments in build report to your email, add `sendLogsInEmail: true` in email integration in shippable.yml.

```yaml
notifications:
    - integrationName: email
      type: email
      recipients:
        - deepika@shippable.com
      sendLogsInEmail: true
```

You can, however, add an Email account integration to receive notifications for monitored Docker images. To do this,

1. From your Shippable dashboard, click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` tab. Click on 'Add Integration'
2. **Integration type:** In the dropdown, select `Email`
3. **Integration Name:** Use a distinctive name that's easy to recall. Example: `manishas-email-lhs`
5. Enter your email address: `janedoe@shippable.com`
6. Click on `Save`


Check our tutorial to [notify failure/success status for your CI project, using Email](http://blog.shippable.com/notifying-ci-failure/success-status-on-email-slack). 

---

## HipChat notifications
You will need to configure this integration to send notifications to HipChat room(s) and user(s). Notifications can be sent for build status updates, monitoring your Docker image updates, and deployment updates.

**Generate a TOKEN for API access**

1. Sign in to your HipChat account using [this link to generate a token](https://www.hipchat.com/account/api).
2. Create a token with `Send Message` and `Send Notification` scopes.

**Set up HipChat integration on Shippable**

1. From your Shippable dashboard, click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` tab. Click on 'Add Integration'
2. **Integration type:** In the dropdown, select `HipChat`
3. **Integration Name:** Use a distinctive name that's easy to recall. Example: `manishas-hipchat`
4. In the **token** textbox, paste the token you created in the Hipchat UI above.
5. Click `Save`

You have now configured HipChat and can use this integration during your CI and Deploy workflows.

In addition, you can also review our blog on [getting started with HipChat notifications](http://blog.shippable.com/hipchat-integration-is-here).

---
