page_title: Navigating Shippable's Account Settings - Images UI
page_description: Overview of Shippable's Account Settings Images section UI
page_keywords: User Interface, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

#Images
Shippable lets you monitor any Docker image from any registry and be notified if the image changes.

This is very powerful since you can now set up Shippable to automatically build all dependent projects when an upstream image is updated.

This feature is available to all Shippable users for free.

You can monitor any number of images at no cost.

Check out this feature by doing the following steps:
-  Sign in to [Shippable](https://app.shippable.com)
- Click on the 'Account Settings' (gear icon on the top right hand navigation bar)
- Click the 'Images' section.

##Adding an image
To monitor an image and receive notification when the image is updated in the registry,
follow the steps below.

- Create an account integration for an Image registry (such as Docker Hub, Docker
Trusted Registry,  Amazon ECR, Google GCR, Quay.io or any private registry) where
the image is to be monitored.
[Instructions here](/navigatingUI/accountSettings/integrations/#adding-an-account-integration)
- Create an account integration for the type of notification you want to receive
when the image changes. You can set up [Slack](/integrations/notifications/slack/#docker-image-monitoring-notifications), [HipChat](/integrations/notifications/hipchat/#docker-image-monitoring-notifications), [IRC](/integrations/notifications/irc/#configuring-irc-notifications-to-monitor-docker-images), or [Email](/integrations/notifications/email/#configuring-email-notifications-to-monitor-docker-images)
notification.
- Go to your 'Account Settings' (gear icon on the top right hand navigation bar)
and click the 'Images' section.
- Click the `Add image` button and complete the following:
     - Image Name: `your_repo_name/image_name` (the image from your Image Registry).
     - Hub Integration: The Image Registry integration that has permissions to
     access this image. You can create a new one if you don't have one already
     configured in your Account Settings.
     - Click on the `Save Image` button.
     - Next, enter the type of notification you want to receive each time the
     image changes. This can be email, Slack, HipChat or IRC.
     - If your selection is Slack or HipChat or IRC, enter the account integration
     details.
        - If your selection is email, fill out the email address(es) of recipient(s).
        [Go here](../continuous_integration/notifications/email/) for further help
        in configuring email.
     - Click on Ok
     - You will start seeing the status of the image. The number of tags and the
     latest synced time shows that we could successfully sync the image and will
     be able to send notifications when the image changes.

Images are 'watched' every 20 mins, so your notification will be sent 20 mins after the image is updated, in the worst case.

<img src="../../images/accountSettings/addImage.png" alt="Adding an Image to
monitor" style="width:700px;"/>

---

##Tags
Once configured, Shippable periodically polls the registry and the tags are updated when there is any update to the image in the source registry.

You will see the latest tag against the image when the image is synced.

---

##Updating an image
To update an image or change the notification settings:

- Go to your 'Account Settings' (gear icon on the top right hand navigation bar).
- Click the 'Images' section.
- You will see the list of images being watched.
- Click on the image_name to update the notification setting.

<img src="../../images/accountSettings/updateImage.png" alt="Updating an Image"
style="width:700px;"/>

---

##Deleting an image
To delete an image from being monitored:

- Go to your 'Account Settings' (gear icon on the top right hand navigation bar).
- Click the 'Images' section.
- You will see the list of images being watched.
- Click on the Delete button for the image(s) you want to stop monitoring.


Read the ["How to monitor your Docker Images"](/tutorials/howToMonitorDockerImages/)
section for further details.

---
