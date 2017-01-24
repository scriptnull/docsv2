page_title: Integrating HipChat on the Shippable platform
page_description: Configure HipChat to send out notifications for Continuous Integration/Continuous Delivery actions
page_keywords: Integration, Integrate, Slack, Notify, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, email, HipChat, IRC

#HipChat

You can send HipChat notifications for any status changes for CI and monitored Docker images.

To send these notifications to HipChat channel(s), you will first need to add an integration to your Account Integrations.


---
<a name="addHipchatToAccount"></a>
##Adding a HipChat integration

There are two actions to set up the HipChat integration through the UI. They are:

- Configure an incoming webhook on Slack
- Add the Slack integration to your subscription

#####Generate a token for API access

1. Sign in to your HipChat account using [this link to generate a token](https://www.hipchat.com/account/api).
     - Provide credentials to your HipChat account, if prompted.
2. Create a token with `Send Message` and `Send Notification` scopes.

#####Add the HipChat integration to your Account
* Go to your **Account Settings** by clicking on the gear menu in the top navbar.
* Click on **Integrations** in the sidebar menu.
* Click on **Add Integration**.
* Enter the following:
	* In the **Master Integration** dropdown, choose **HipChat**  
	* Add a friendly name for your integration
	* In the **Token** field, paste the HipChat token
* Click on **Save**. You should now see the integration in your list of integrations.

---

##CI notifications

To use your HipChat integration for your CI workflows, follow the steps below:

* First, add the [HipChat integration to your Account](#addHipchatToAccount)
* Go to your Subscription's Settings tab. This should be the Subscription containing the project you want to send HipChat notifications for.
* Click on **Integrations** in the sidebar menu.
* Click on **Add Integration**.
* Name your HipChat integration with a friendly name. This can be the same name as the one in your account integration.
* From the dropdown, choose the account integration you just created in the step above.
* Click on **Save**.

To enable HipChat notifications for your project, use the following format in the `shippable.yml` file for that project:

```
integrations:
  notifications:
    - integrationName: hipchat-integration
      type: hipchat
      recipients:
        - "#roomOne"
        - "#roomTwo"
        - "@userOne"
      branches:
        only:
          - master
          - dev
      on_success: never
      on_failure: always
```
While the above is a sample code for your `shippable.yml`, use the descriptions of each field below to modify the `yml` and tailor it to your requirements.

- `integrationName` value is the name of the HipChat integration you added to the 'Subscription' settings. It is important the name matches exactly. If not, the build will fail with error as [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match).
- `type` is `hipchat`.
- `recipients` specifies the rooms and/or users you want to send the notification to. Please note this is a required field for HipChat notifications to work.
     - Even if there is a single recipient, use the format `recipients: "#roomOne"` for rooms and `@userOne` for users.
- [optional] `branches` allows you to choose the branches you want to send notifications for. By default, notifications are sent for all branches. The `only` tag should be used when you want to send notifications to specific branches. You can also use the `except` tag to exclude specific branches. [Wildcards](../../ci/advancedOptions/branches/) are also supported.
- [optional] You can set the following options for the `on_success`, `on_failure` tags:
     - The value `change` for `on_success` or `on_failure` fields means you will receive notifications only when the build status changes to success or failure respectively.
     - `always` means that you will always receive a notification for that build status.
     - `never` means that you will never receive a notification for that build status.
     - By default, `on_success` is set to `change` and `on_failure` is set to `always` when HipChat is configured in the yml and you have not specified these tags.
- [optional] You can set the following options for the `on_start`, `on_pull_request` tags:
     - Setting the value to `always` means that you will always receive a notification for build start/pull request.
     - Setting the value to `never` means that you will never receive a notification for that build start/pull request.
     - By default, `on_start` is set to `never` and `on_pull_request` is set to `always` when HipChat is configured in the yml and you have not specified these tags.

Check our blog [on configuring HipChat for both CI and Pipelines](http://blog.shippable.com/hipchat-integration-is-here).

---

##Pipeline notifications
HipChat notifications for Pipeline jobs are not supported at this time. To request these, open a <a href="https://github.com/Shippable/support/issues/" target="_blank">support issue</a> and we'll get back to you.

---

##Docker Image monitoring notifications

You can add a HipChat account integration to receive notifications for monitored Docker images. To do this:

1. Ensure you have configured [the monitoring of a Docker image](/navigatingUI/accountSettings/images/).
2. Go to 'Account Settings' and click the 'Images' section.
3. Select the 'Image' you want to configure the HipChat notifications.
4. Under the 'Notifications' section, use the dropdown for the 'Channel' field and select `hipchat`.
5. From the 'HipChat Integrations' dropdown select the HipChat integration created from your 'Account Settings'.
     - If there is no HipChat integration, click `Create Integration` and create a HipChat Account Integration. Follow [these steps](#Set-up-HipChat-integration-in-Account-Settings), if you need help.
6. In the 'Recipients' field, add the room and/or users. For example: "#roomOne" and/or "@userOne"
7. Click the `Add Item` button.
8. [Optional] Add additional recipients.
9. Click the `Save` button.

<img src="/ci/images/integrations/notifications/hipchat/monitorImages.png" alt="Configure to be notified through HipChat while monitoring a Docker image" style="width:700px;"/>

---
##Deleting the Integration

To remove a HipChat integration, you'll need to remove this integration from all dependencies configured to use it.

###Deleting from a Project
To remove the HipChat integration from a project, simply remove the section shown below from the `shippable.yml` for that project.

```
integrations:
  notifications:
    - integrationName: hipchat-integration
      type: hipchat
      recipients:
        - "#roomOne"
        - "#roomTwo"
        - "@userOne"
      branches:
        only:
          - master
          - dev
      on_success: never
      on_failure: always
```

###Deleting from a Subscription
To delete a HipChat integration from a subscription, the steps are:

1. Ensure you have logged in to [Shippable](https://app.shippable.com).
2. Select your Subscription from the dropdown burger bar menu on the top left.
3. Click the 'Settings' tab and go to the 'Integrations' section.
4. Review the list of integrations for your subscription.
5. Click the `Delete` button next to the HipChat integration.

<img src="/ci/images/integrations/notifications/hipchat/deleteInt.png" alt="Delete the HipChat integration from your Subscription" style="width:700px;"/>

###Deleting Account Integration
To delete the HipChat Account integration, all the dependencies, such as Subscriptions, using the HipChat account must be deleted first.

Do note that this action is irreversible.

The steps to delete the HipChat Account integration are:

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
2. Select the HipChat integration from the list of integrations. If you have many entries, use the `Filters` dropdown and select `HipChat`. Alternatively, you can use the `Integration Name` field to provide the name of your HipChat integration.
3. Your HipChat integration shows up in the list.
4. Click on the `Delete` button.
5. A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration. The list will include any project, environment or subscription image dependent on this integration.
6. If there are dependencies, individually access the `Settings` tab for each project/environment/subscription image and delete the HipChat integration.
7. Once all dependencies of the HipChat integration have been removed, Step 5 will show the message: `No dependency`.
8. Click the `Yes` button to delete the HipChat Integration.

--------
