page_title: BYON (Bring Your Own Nodes) while running your Continuous Integration projects
page_description: An overview of the BYON feature on Shippable
page_keywords: containers, lxc, Docker, Continuous Integration, Continuous Deployment, CI/CD, testing, automation

# Running builds on your own machines (BYON)

By default, all your builds run in build containers hosted on Shippable's infrastructure.

However, some organizations have very specific security requirements that do not allow them to run builds on hosted infrastructure. Others need bigger machines since their builds are resource intensive and take too long on Shippable's infrastructure.

To address these types of scenarios, we offer a novel approach called Bring Your Own Nodes (BYON).

**BYON lets you run builds on your own infrastructure**, so you can attach your machines to your Shippable subscription and all your builds are routed to those machines.

This is a powerful hybrid approach that gives you the benefit of using a SaaS service for CI orchestration, while still giving you full control over the infrastructure and security of your build machines.

---

##Advantages of BYON  

**Security**: Your build machines can be inside your VPC and/or behind your firewall, which  gives you the ability to configure access, IAM, etc.

We even have a way of configuring these machines so that you do not have to grant Shippable SSH access!

This means your code never leaves your firewall and no external entity can access your machines.

**Complete control over your build machines**, including SSH access, ability to choose your cloud provider and size of build machines.

**Faster build times**: You can leave your build machines running all the time, which eliminates the occasional 2-3 mins per build that is added when new machines are spun up on Shippable's hosted infrastructure.

**Bigger containers**: We do not constrain container size in terms of cores and RAM while spinning up build containers on your infrastructure.

This means your container uses all available capacity of your build machine, which is especially great if your builds are resource intensive.

The result? Faster builds!

**Docker caching**: If you use Docker for your build workflows like pulling Docker images from a registry or building Docker images, your build machines will already have these images and this will speed up your builds.

---


## Minimum requirements
The minimum requirements for a build machine that can be attached to Shippable are:

* 1.8GB RAM
* 30GB SSD
* Ubuntu 14.04 OS
* 64-bit architecture

**Please note that you will need to attach one build machine per parallel build.**

We have tested this extensively using machines on Amazon EC2 and Digital Ocean.

We expect machines from other providers to work without issues as long as they satisfy the above requirements.

If you run into any issues, contact us through [support](https://github.com/shippable/support/issues) or email our [support alias](mailto:support@shippable.com).

---

## Configuring BYON

BYON is configured through your Subscription Settings UI.

To start, click on the `Settings` tab of your Subscription page and then click on `Nodes` in the sidebar menu.

Choose the radio button for `My node`

<img src="../../images/advancedOptions/byon.png" alt="bring your own node" style="width:1000px;"/>

Next, follow instructions for adding a build node below.

Please note that you need to add one build node per parallel build, up to a maximum of the number of parallel builds allowed in your plan.

### Adding a build node
* To add a build machine, click on the `Add node` button. You will be redirected to the Add Node page.
* Enter a name for the node and its IP address.
* You can choose to initialize the build host through Shippable or run the initialization scripts yourself. Initialization through Shippable requires you to grant SSH access, so if you do not want to grant that for any reason, select the radio button for `I want to run the scripts myself`
* To initialize the node through Shippable,
    * Enter the SSH port for your build host. This is usually port 22, but is configurable.
    * Choose whether you want to enable swap space on your machine. This is recommended.
    * Copy the command shown and run it on your build host. This will create a
    shippable user on your host and allow us to run initialization scripts on
    your machine.
    * Check the checkbox to confirm that you have run the command on your machine
    and click on `Initialize`
    * You will be redirected to a page showing you the console log as your machine
    is initialized.
    * When your node is ready, the status indicator on will turn green.
* To run the initialization scripts yourself,
    * Choose whether you want to enable swap space on your machine. This is recommended.
    * Click on `Generate initialization scripts` to generate the script.
    * Click on `Download scripts` to download. Copy it to your build machine and
    run them.
    * Check the `I have run this script on my node successfully` and then click
    on the `Save` button.
    * Your node status will automatically show green at this point. We have no way F
    of verifying that the node was in fact successfully initialized so you will
    need to make sure this was the case.

Once you add your first build node, all subsequent builds for that subscription
will run on your machines.

### Editing a build node
You can click on the `Edit` button for a build node to edit the node name. Nothing
else can be edited for a node.

### Resetting a build node
You can reset a node by clicking on the `Reset` button. This action will initialize/install everything from scratch.

You can only reset nodes provisioned on Shippable's infrastructure.

If you have added your own build nodes, you will need to re-download the initialization script and run it on your node.

### Deleting a build node
Click on `Delete` to delete your build node. This action is final and cannot be undone.

---
