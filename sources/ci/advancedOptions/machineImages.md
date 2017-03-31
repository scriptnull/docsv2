page_title: Node Machine Images on Shippable
page_description: How to configure the node Machine Image for your Continuous Integration projects
page_keywords: ci/cd dashboard, subscription settings, CI/CD, shippable CI/CD, documentation, shippable, config, yml, AMI, Docker, version, 1.11, 1.12

# Machine images

When a build is triggered for any of your enabled projects, we spin up a virtual machine on AWS to run your build. We use our standard AMIs (Machine Images) to spin up these VMs. **This should not be confused with the actual build container where your CI workflow is executed.** The CI container is spun up on the build VM using either the default image depending on your yml configuration or a custom image if specified in the yml.

The green arrow in the picture below shows the relationship between the base VM and the CI container.

<img src="../../images/advancedOptions/shippableOverview.png"
alt="Machine Image for a Subscription" style="width:1000px;"/>

We currently offer the following Machine Images:

The `Stable` machine image has been extensively tested and has served the needs of almost all customers for a very long time. However, this image is now deprecated and is not recommended for use on newer projects. If you are currently using this image, please plan to migrate to a more recent image.

The `Unstable` machine image was introduced to enable scenarios that need newer versions of Docker for customers that need it. It has been tested for the common scenario. This image is deprecated and is not recommended for use on newer projects. If you are currently using this image, please plan to migrate to a more recent image.

From release 5.3.2 onwards, Shippable will introduce new machine images on each release with the latest tooling and official Docker images. The images are named after each release version (`v5.3.2`, `v5.4.1`, and so on). They are tested for all scenarios and are recommended for general use.

---
## Available machine images

By default, your subscription is configured to use the latest default image available available at the time you run your first build on Shippable. You can switch between the available images at any point in time. The most common reason for changing the machine image for your subscription is if you need a newer Docker or OS version.

* **v5.4.1 (Released March 30, 2017)**
    * Shippable Official Docker Images (tag: `v5.4.1`)
    * Docker Server Version: 1.13.0
    * Storage Driver: aufs
    * Root Dir: /data/aufs
    * Backing Filesystem: extfs
    * Dirperm1 Supported: true
    * Cgroup Driver: cgroupfs
    * Kernel Version: 3.19.0-51-generic
    * Operating System: Ubuntu 14.04.5 LTS

* **v5.3.2 (Current default, released March 11, 2017)**
    * Shippable Official Docker Images (tag: `v5.3.2`)
    * Docker Server Version: 1.13.0
    * Storage Driver: aufs
    * Root Dir: /data/aufs
    * Backing Filesystem: extfs
    * Dirperm1 Supported: true
    * Cgroup Driver: cgroupfs
    * Kernel Version: 3.19.0-51-generic
    * Operating System: Ubuntu 14.04.5 LTS

* **Stable (deprecated)**
    * Shippable Official Docker Images (tag: `prod`)
    * Docker Server Version: 1.9.1
    * Storage Driver: aufs
    * Root Dir: /data/aufs
    * Backing Filesystem: extfs
    * Dirperm1 Supported: true
    * Execution Driver: native-0.2
    * Logging Driver: json-file
    * Kernel Version: 3.19.0-51-generic
    * Operating System: Ubuntu 14.04.3 LTS

* **Unstable (deprecated)**
    * Shippable Official Docker Images (tag: `prod`)
    * Docker Server Version: 1.11.1
    * Storage Driver: aufs
    * Root Dir: /data/aufs
    * Backing Filesystem: extfs
    * Dirperm1 Supported: true
    * Cgroup Driver: cgroupfs
    * Kernel Version: 3.19.0-51-generic
    * Operating System: Ubuntu 14.04.3 LTS



---

## Changing the Machine image for a subscription

To select a different Machine Image, go to the 'Settings' tab of your 'Subscription'. Click on 'Options' in the left sidebar and select the image you want from the dropdown under the 'Machine Images' section. Please note that this setting will affect all projects and builds in your Subscription. 

<img src="../../images/advancedOptions/machineImage.png"
alt="Machine Image for a Subscription" style="width:1000px;"/>

---
