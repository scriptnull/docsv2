page_title: Node Machine Images on Shippable
page_description: How to configure the node Machine Image for your Continuous Integration projects
page_keywords: ci/cd dashboard, subscription settings, CI/CD, shippable CI/CD, documentation, shippable, config, yml, AMI, Docker, version, 1.11, 1.12

# Machine images
Machine images are used to spin up a VM for your build. **This should not be confused with the actual build container where your CI workflow is executed.** The CI container is spun up on the build VM using either the default image depending on your yml configuration or a custom image if specified in the yml.

The green arrow in the picture below shows where the build machine image is used.

<img src="../../images/advancedOptions/shippableOverview.png"
alt="Machine Image for a Subscription" style="width:1000px;"/>

The `Stable` machine image has been extensively tested and has served the needs of almost all customers for a very long time. However, this image is now deprecated and is not recommended for use on newer projects. If you are currently using this image, please plan to migrate to a more recent image.

The `Unstable` machine image was introduced to enable scenarios that need newer versions of Docker for customers that need it. It has been tested for the common scenario. This image is deprecated and is not recommended for use on newer projects. If you are currently using this image, please plan to migrate to a more recent image.

From release 5.3.1 onwards, Shippable will introduce new machine images on each release with the latest tooling and official Docker images. The images are named after each release version (`v5.3.1`, `v5.4.1`, and so on). They are tested for all scenarios and are recommended for general use.

---
## Selecting an image

Your subscription is configured to use the latest default image the first time your run a build on Shippable. You can switch between the available images at any point in time.

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

* **v5.3.1 (Current default, released March 3, 2017)**
    * Shippable Official Docker Images (tag: `v5.3.1`)
    * Docker Server Version: 1.13.0
    * Storage Driver: aufs
    * Root Dir: /data/aufs
    * Backing Filesystem: extfs
    * Dirperm1 Supported: true
    * Cgroup Driver: cgroupfs
    * Kernel Version: 3.19.0-51-generic
    * Operating System: Ubuntu 14.04.5 LTS

---
## Setting an image

To select your Machine Image, go to the 'Settings' tab of your 'Subscription'. Click on the 'Options' section and select from the dropdown under the 'Machine Images' section.

<img src="../../images/advancedOptions/machineImage.png"
alt="Machine Image for a Subscription" style="width:1000px;"/>

---
