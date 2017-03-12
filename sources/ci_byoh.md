page_title: BYOH, BYON
page_description: Code examples, FAQs, language & platform support
page_keywords: containers, lxc, Docker, Continuous Integration, Continuous Deployment, CI/CD, testing, automation

# Running builds on your own machines (BYON)
For customers who want complete control over their build hosts, we offer a novel approach called **Bring Your Own Node (BYON)**.

BYON lets you run builds on your own infrastructure, so you can attach your machines to your Shippable subscription and all your builds are routed to those machines. This is a powerful hybrid approach that gives you the benefit of using a SaaS service for CI orchestration, while still giving you full control over the infrastructure and security of your build machines.

---

##Advantages of BYON

**Security**: Your build machines can be inside your VPC and/or behind your firewall, which  gives you the ability to configure access, IAM, etc. We even have a way of configuring these machines so that you do not have to grant Shippable SSH access! This means your code never leaves your firewall and no external entity can access your machines.

**Complete control over your build machines**, including SSH access, ability to choose your cloud provider and size of build machines.

**Faster build times**: You can leave your build machines running all the time, which eliminates the occasional 2-3 mins per build that is added when new machines are spun up on Shippable's hosted infrastructure.

**Bigger containers**: We do not constrain container size in terms of cores and RAM while spinning up build containers on your infrastructure. This means your container uses all available capacity of your build machine, which is especially great if your builds are resource intensive. The result? Faster builds!

**Docker caching**: If you use Docker for your build workflows like pulling Docker images from a registry or building Docker images, your build machines will already have these images and this will speed up your builds.

---

## Pricing
BYON is a feature for Continuous Integration and is not priced separately. The number of minions in your pricing plan determines how many parallel builds we run at a time.

---

## Minimum requirements
The minimum requirements for a build machine that can be attached to Shippable are:

* 1.8GB RAM
* 30GB SSD
* Ubuntu 14.04 OS
* 64-bit architecture

**Please note that you will need to attach one build machine per parallel build.**

We have tested this extensively using machines on Amazon EC2 and Digital Ocean. We expect machines from other providers to work without issues as long as they satisfy the above requirements. If you run into any issues, drop an email to our [support alias](mailto:support@shippable.com)

---

## Configuring BYON
To set up your BYON for your subscription, go to [Subscription settings section explaining how to configure your subscription to use your own build infrastructure](ci_subscriptions.md#ci_byon) to learn more.

---
