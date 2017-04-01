page_title: Machine Images on Shippable
page_description: An explanation of the images used for CI
page_keywords: ci/cd dashboard, subscription settings, CI/CD, shippable CI/CD, documentation, shippable, config, yml, AMI, Docker, version, 1.11, 1.12

# What are Machine images?

Each Subscription on Shippable has an associated Machine Image which is used to spin up virtual machines on AWS to run your builds. Each Machine Image comes pre-installed with a set of tools and packages that are necessary for most builds, along with a set of default Docker images that are used to spin up CI build containers.

The following picture shows the relationship between the build machine and the build container. The build container is spun up on the build machine using either the default Docker image depending the `language` tag in your yml configuration or a custom Docker image if specified in the yml.

<img src="../../images/advancedOptions/shippableOverview.png"
alt="Machine Image for a Subscription" style="width:800px;"/>

We currently offer the following Machine Images. You can click on any one to see what is pre-installed on that image:

| Machine Image | Release date     |
|---------------|-------------------|
| [v5.4.1](v5-4-1/)        | March 30, 2017    |
| [v5.3.2](v5-3-2/)        | March 11, 2017    |
| [Stable](stable/)        | February 19, 2016 (deprecated) |

<br>
**The default Machine Image for your subscription is the latest image available when your Subscription was added to Shippable.**

In most cases, the default Machine image set for your Subscription will be sufficient to run your builds. The main reasons why you might want to consider changing to a more recent image are:

* You need a newer language/service/package version
* You need a newer Docker version

---

## Viewing Subscription Machine Image

To see what Machine Image is being used for your subscription, go to the `Settings` tab of your Subscription and click on `Options` in the left sidebar.

The top item on this page will show you the machine image currently being used:

<img src="../../images/view-machine-image.png"
alt="Machine Image for a Subscription" style="width:800px;"/>

---
<a name="change-machine-image"></a>
## Changing the Subscription Machine image

To select a different Machine Image:

* Go to the `Settings` tab of your Subscription
* Click on `Options` in the left sidebar and select the image you want from the dropdown under the 'Machine Images' section. Please note that this setting will affect all projects and builds in your Subscription.

<img src="../../images/change-machine-image.png"
alt="Machine Image for a Subscription" style="width:800px;"/>

---
