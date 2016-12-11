page_title: Build Timeouts
page_description: Information about your builds timing out while running Continuous Integration
page_keywords: getting started, questions, documentation, shippable, config, yml


# Build timeouts

To avoid situations where builds hang due to test or build scripts, we will time out your builds in specific scenarios.   

By default, your builds will time out if:

-   there is no log output or a command hangs for 10 minutes, or
-   the build runs for more than 60 minutes for a project in a free subscription
-   the build runs for more than 120 minutes for a project on a paid subscription

Timed out builds are shown with a purple colored status on your build dashboard:



---

##Custom Timeouts
You can customize the build timeout setting for any enabled project. This setting can be configured to be between 1 - 60 minutes for free subscriptions and 1 - 120 minutes for paid subscriptions.

To customize the timeouts for a project:

- Go to 'Project' settings
- Click the 'Runs Config' section in the sidebar menu
- Scroll down to the 'Custom Timeouts' section
- Click the `Edit` button
- Enter a value between 1 and 60 (for free plans) or 120 (for paid plans)
- Click the `Save` button.

NOTE: For any values entered less than 1 or greater than 60 (for free plans) or 120 (for paid plans), you'll see the error:

```
(Id: 4005) Custom timeout is greater than the maximum limit of 3600000 milliseconds
verbose: projects|putById|callerId:XXXXX|projectId:YYYYYY|_validateTimeout
```

For further reading, we have a blog on [Changing the default timeout for a CI project](http://blog.shippable.com/changing-the-default-timeout-for-a-continuous-integration-project).

Customizing build timeouts is also covered in the [Navigating the UI/Project Settings](/navigating_ui/projects/settings/#custom-timeout) documentation section.

Please [let us know](https://github.com/shippable/support/issues) if you believe a build is timing out when it shouldn't do so and we will take a look.

---
