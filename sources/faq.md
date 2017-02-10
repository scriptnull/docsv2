page_title: Shippable FAQ
page_description: Commonly asked questions that will help with troubleshooting
page_keywords: concepts, documentation, shippable, CI/CD, pipelines, upgrade containers, add new pipeline, multi containers, microservices, Continuous Integration, Continuous Deployment, Delivery, testing, automation, frequently asked questions

# FAQ

This document lists the FAQs that we've received through our [Support repository](https://github.com/Shippable/support/issues) and speaking with our customers, on running Continuous Integration. If you have specific errors that you would like to troubleshoot, then refer troubleshooting sections under [Continuous Integration](/ci/troubleshoot/) and [Pipelines](/pipelines/troubleshoot/).

---
##Setup
### How can I upgrade or downgrade my plan?

The Continuous Delivery plan gives you a minimum of 1 free minion & 1 free pipeline. Upgrading or downgrading your CI  simply means increasing or decreasing the number of build minions in your subscription. Similarly upgrading or downgrading your Pipelines means increasing or decreasing the number of pipelines and containers in your subscription.

You can do this by going to the 'Billing' Tab on your Subscription Dashboard and clicking the 'Update plan' button. Use the slider to indicate the number of containers you want for CI and/or the number of pipelines you want for CD. Click on the 'Save changes' button when you are done.

Plan upgrades are effective immediately and your bill will be pro-rated for the current month. Plan downgrades are effective immediately, however we do not issue refunds for minions that were already paid for during the current month.

Check our blog on [upgrading your CI/CD subscription](http://blog.shippable.com/how-to-upgrade-your-ci-cd-subscription) that includes additional details on when to upgrade using your organizational details.

---

## My builds stopped working. How do I know if Shippable is down?

You can get the current status of the Shippable UI and API by going to our [Service Status page](http://status.shippable.com). Our status page also shows the status of some of our third party integrations like Amazon EC2, Quay.io, Digital Ocean, as well the response time of our API.

The service status page can be viewed from our product UI by going to the heart ekg icon in your top navbar.

For additional details, check our blog on [Shippable status page](http://blog.shippable.com/features_amazon_ecr_regions_status_page_github_tags_and_more#status_page).

---

### What does the term Minion mean in Shippable?
Minions are Docker based containers that run your CI builds.

When your build is triggered, we determine which Docker image to use in order to spin up your build minion. By default, the minion will container popular versions of the language specified in your yml, as well as popular tools and services used with that language. All this happens under the hood and 'just works'.

If you're a Docker enthusiast and want to spin up your build minion based on your custom Docker image or build an image from Dockerfile, you can do so by following instructions here.

Your build minions are transient and spin up when a build is triggered and are destroyed when a build completes.

Each minion has 2 cores and 4GB RAM. If you use your own infrastructure to run your builds with [Bring Your Own Node (BYON)](/ci/advancedOptions/byon/) option, you can spin up bigger containers for your builds since we do not restrict resources for containers running on customers' infrastructure.

---
### Do I need to create an Account on Shippable?
You do not need to explicitly create an account on Shippable to start using it. However, since we allow you to connect multiple source control providers and clouds to Shippable, the term 'account' is used to encompass all of these identities. So for example, 'sync' at an account level means syncing your information across all source control providers and connected third party services. Read the [accounts section](/navigatingUI/accountSettings/accounts/) for more details.

---
### Can you explain what a Subscription on Shippable means?
A subscription on Shippable corresponds to an organization or personal account on GitHub or Bitbucket. So if you sign in to Shippable with GitHub credentials and your username is abcfoo and you're a member of orgs org1foo and org2foo, you will have 3 subscriptions on Shippable.

Our billing plans are at a subscription level, so you can upgrade or downgrade each of your subscriptions independently. Also, we mirror permissions from your source control provider, so if someone has access to organizational repositories on GitHub/Bitbucket, they will also have access to view and run builds on Shippable. These permissions are synced automatically and you do not have to do anything to make this work. Read the [subscriptions section](/navigatingUI/subscriptions/ci/) for more details.

---
### What is a Project?
A project on Shippable corresponds to a repository on your source control provider. As with subscriptions, project permissions are also synced with your source control provider.

Once a project is enabled, we build all commits and pull requests for that project, irrespective of who commits and opens the pull request. Refer the [projects section](/navigatingUI/projects/status/) for additional details.

---
### What is the difference between a Build Container (cexec) and Shippable Agent (mexec) on the Shippable platform?
A Build Container (also called cexec) is a Docker Container that is spun up on the host Node machine that executes the Continuous Integration related tasks. These include installing the required dependencies, cloning information from the source control system repository, executing unit tests and running test/code coverage reports, all of which have to be specified in the `shippable.yml` file.  

Shippable Agent (mexec) on the other hand is also a Docker Container that is spun up on the host Node machine. The main function of the Shippable Agent is to interact with the Shippable platform and the Build Container and, performs actions outside the build container. Within the `shippable.yml` file, the `pre_ci`, `pre_ci_boot` and the `push` sections are executed on the Shippable Agent.

Read the blog [Key concepts to get most out of Shippable's Continuous Integration](/ci/overview/) for a deeper understanding of the functions and interaction between the Build Container and Shippable Agent.

---
### Why can't I see some of my repositories in my Shippable account?

This happens due to one of the following reasons:

- You haven't enabled private repositories in your Shippable account. Go to Account Settings (gear icon on the top right hand navigation bar), in the 'Accounts' tab and under 'Git Identities' section, click 'Enable' under 'GitHub' 'Click to enable private access'.
- Your account hasn't yet been synced with the latest permissions from GitHub. To force sync your account, go to your Account Settings and click on the `Force Sync` icon next to your Account Id.
-  You're a Bitbucket user and you have mercurial repositories. We do not support mercurial at this time, so you will need to convert them to git or use another platform for CI/CD.

---
### Why do I get an error when I try to enable a project that is listed on my dashboard?

This usually happens if you are a collaborator on a project and the owner of the project has not given Shippable access to the project. You can verify this by confirming that the owner of the project can see the project on their Shippable dashboard.

---
### How can I validate my shippable YML?

You can use either of the tools below to validate if your YML is valid:

* [YAML Lint](http://www.yamllint.com/)
* [YAML Online Parser](http://yaml-online-parser.appspot.com/)

---
### Why can't I see my BitBucket repos in my Shippable account?

Shippable only supports git based repositories, so if you have mercurial
repositories in your BitBucket account, you will not see them in the
Shippable repository list. If you cannot see git based repos, please
open an issue on our [GitHub Support
repo](<https://github.com/Shippable/support>).

---
### Why can't Shippable see my org on GitHub?

GitHub's default policy when a new org is created is 'access
restricted'. In order for Shippable to be able to see the org, you must
manually grant access to Shippable. This can be resolved by going to the
third-party access section for the org, and clicking 'Remove
restrictions' Under the 'Third-party application access policy' section.

---

### How do I link my GitHub and Bitbucket accounts?

Please read our documentation on [linking GitHub and Bitbucket accounts](/ci/advancedOptions/linkGitHubBitbucket/). In addition, refer our [blog](http://blog.shippable.com/how-to-link-github-and-bitbucket-accounts) on this topic.

---

### Why am I not able to see Bitbucket org repos after deleting and recreating my account on Shippable?

Deleting the shippable account will also delete the permissions
associated with the account. If you recreate your account, bitbucket
will not allow us to pull all the permissions you have, unless the owner
of that organization logs in back to shippable and then click on the
sync repos button to see the repos.

---

### Why is my project showing up as "empty" after I enable it? It is certainly not empty in github!

A project is empty in Shippable if there are zero builds associated with it. A new project that you have just enabled shows up as an empty project. To avoid cluttering the project page with projects that are never built, the projects page doesn't show projects that have no builds unless you explicitly use the check box to let us know you want to see all projects. An exception to this is if you have just enabled a project; we do check this box during the enable process, so you are able to see your new project. We are continuously iterating on the user experience, so please write to us at support@shippable.com if you have any feedback on the feature.

---

### I converted a public repository (enabled as a project on Shippable) to a private repo. Should I change any setting in Shippable?

When public or private repo on GitHub/Bitbucket (and enabled as a project on Shippable) is converted to a private or public repo, all you need to do is reset the enabled project on Shippable. To do so:

- Click on your project from the Shippable dashboard
- Click the `Settings` tab
- Scroll all the way down and click the `Reset` button under the 'Reset' section
- Click `Confirm`.

The reset action will do the following things:

1. Reset the webhook for Shippable
2. Generate a new deploy key and update the repository

If you are using encrypted variables for this project, they'll need to be re-encrypted. Integrations and other settings will not be affected.

---

### Is mariadb supported on Shippable?

Shippable [supports](/ci/supported/) lots of different services, tools and third party services. If you have a service or a tool that is currently unsupported, you can still use it to run CI within Shippable in either of the two ways listed:

1. Use Shippable's default images based on the language you use & install 'mariadb' as a dependency in the `build: ci` step.
2. If you have an existing Docker image with 'mariadb' and other dependencies installed, then you can [override Shippable's default image](/ci/shippableyml/#pre_ci_boot) and use it for CI. You can also [build your own Docker image](/ci/shippableyml/#pre_ci) with all the dependencies including 'mariadb' and use it for CI.

---

## Continuous Integration (CI)
### I have enabled my repository and committed code, but my build doesn't start. What could be wrong?

A couple of reasons why this could happen:

(1) Missing YML in the branch you are building. Create a `shippable.yml` file in the root of your repository. Here is a [reference/example](/ci/shippableyml/) of a `shippable.yml` file

(2) Shippable YML is invalid. Please validate your YML using either of the links below:

* [YAML Lint](http://www.yamllint.com/)
* [YAML Online Parser](http://yaml-online-parser.appspot.com/)

---

### How do I set desired timezones inside the minions?

By default, our minions are configured with ETC/UTC timezone which is
set in /etc/timezone file for ubuntu minions. However, we allow you to
set a specific time zone for the minion in before\_script section of
your yml file . For example, you can do the following in your yml:

```
build:
  ci:
    - echo 'Europe/Paris' | sudo tee /etc/timezone
    - sudo dpkg-reconfigure --frontend noninteractive tzdata
```

This will change your minion timezone to paris time. Refer the article
[list of tz database time zones](http://en.wikipedia.org/wiki/List_of_tz_database_time_zones) to select the timezone for your location.

---

### How do I skip webhook builds?

Any changes to your source code will trigger a build automatically on Shippable. If you do not want to run build for a particular commit,
then add ```ci skip``` or ```skip ci``` to your commit message.

Our webhook processor will look for the string ```ci skip``` or ```skip ci``` in the commit message and if it exists, then that particular
webhook build will not be executed.

**PR Builds:** To skip a PR build, the ```ci skip``` or ```skip ci``` needs to be part of your PR title, since that's what GitHub sends us as part of the webhook.

**PR Build with multiple commits:** If the original commit did not include the skip flags and subsequent commits do, the PR build will _not_ skip a build.

---

### In my total build time, provisioning a node takes the longest time. How can I reduce the node provisioning time?
When you trigger a build, we spin up a build machine and run your build. This provisioning takes approximately 2-3 minutes.

If you want your builds to start immediately and avoid the node provisioning time, you can use our feature that lets you run builds on your own infrastructure. You can buy a machine from AWS or Digital Ocean or Linode and attach it to your Shippable subscription. We will run all your builds on your attached machines, and since your machines are always up, we will not need to provision nodes and this will save ~3 mins per build. You can read more about [Bringing Your Own Node - BYON](/ci/advancedOptions/byon/). Once you BYON, [read the instructions](navigatingUI/subscriptions/settings/#nodes) for attaching your build infrastructure to Shippable.

---

### I am pushing to heroku as part of my build. Why is this suddenly failing?

We have made a change as to where your keys are stored on your minion.

You probably have a command in your yml that looks something like this:

```
- test -f ~/.ssh/id_rsa.heroku || ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.heroku && heroku keys:add ~/.ssh/id_rsa.heroku
```

You will need to replace the `~/.ssh/id_rsa` to `/tmp/ssh/00_sub` since that is the new location for keys. Your command will now look like this:
```
- test -f ~/.ssh/id_rsa.heroku || ssh-keygen -y -f /tmp/ssh/00_sub > ~/.ssh/id_rsa.heroku && heroku keys:add ~/.ssh/id_rsa.heroku
```
Your push to Heroku should succeed with this change.

---

### I cannot start a manual build for my Bitbucket project. Why is it not working?

Check your branch name to see if it contains a ```/```. The Bitbucket API currently does not support branch names with a ```/``` in them. We opened a support issue with them - [Get commit/{revision} API does not accept branch names with "/"](https://bitbucket.org/site/master/issues/9969/get-commit-revision-api-does-not-accept) over 1 year ago and there is still no resolution for this. This is not a Shippable bug and while we understand that this affects everyone who uses Gitflow+Shippable+Bitbucket, we cannot fix this at our end.

---

### I am using PHP 7.0 and am unable to install extensions as part of my yml config.

We use pickle to install extensions for PHP. This works for all versions of PHP except 7.0.

Pickle requires `php$ver-dev` environment (which has development modules) to compile extensions. At this time, there is no official php7-dev environment, so pickle is unable to find dependent modules and cannot compile extensions like intl, redis, and gettext.

We will watch for updates and as soon as `php7-dev` is available, the next image update will address this.

---

###How do pull request builds work?
The usual workflow for a pull request is:

* Developer commits to his branch/fork: This will trigger a build on Shippable if the branch/fork is enabled for CI
* Developer opens a pull request with the changes. This will trigger a pull request build if the branch that is being merged to is enabled on Shippable. We will execute your CI workflow on temporarily merged code and let you know if everything works as expected. A couple of things to know about pull request builds:
  - The YML is always picked from the destination(base) branch.
  - If the pull request comes from a private fork of the project and the subscription key is not added as a deploy key for the fork, the pull request build will fail at the `git_sync` CI step. This is due to the way Bitbucket handles permissions on private forks. To fix this:
     - Copy the subscription deploy key from Shippable Subscription > Settings > Deployment Keys
     - Next, add it as a deploy key for the private fork: Bitbucket Project Settings > Deploy Key > Add.
* The pull request is merged. This will trigger a build if the destination (base) branch is enabled on Shippable.

---

### How do I specify a region while setting up Amazon EC2 Container Registry (ECR) Integration?
When you set up the [Amazon ECR integration](/integrations/imageRegistries/ecr/), the default region is set to  `us-east-1`. You can override the default region by configuring the `shippable.yml` file as shown below.

```
integrations:
  hub:
    - integrationName: your_integration_name
      type: ecr
      region: us-west-2
```

---

### Can I use multiple languages in a build?

Yes. The [language tag in the yml](ci/shippableyml/#language) mainly tells us which default build image to use and any default handling for that language.

If you want to use multiple languages, you should:

1. Specify your primary [language](/ci/shippableyml/#language) as usual. For example: `language: node_js`. This will cause your build to run on the default Shippable image for that language.
1. In the `ci` section of your yml, first install the dependencies for the other language(s) in the build.

You can also build your own Docker image with all the dependencies you need for both languages and then [override the default build image](ci/shippableyml/#pre_ci_boot) to use your Docker image.

Let's look at an example of using Node.js & Ruby in a build.

Specify your [language](/ci/shippableyml/#language) in the `shippable.yml`. For example: `language: node_js`. This will cause your build to run on the [default Shippable image](http://docs.shippable.com/ci/languages/node/) for that language.

All [official Shippable images](https://hub.docker.com/u/drydock/) have rvm installed, with a default version of Ruby. However, the rvm location is not added to the $PATH environment variable, so you will need to `source` rvm in your YML. This will give you access to both `ruby` and `rvm`. Your `shippable.yml` should look like this:

```
language: node_js

#desired Node.js version(s)
node_js:
  - "0.12"

build:
  ci:
    - source /usr/local/rvm/scripts/rvm
```

---

### Can I run unit tests in multiple languages in the same repository?

Yes, you can run unit tests in multiple languages in the same repo. Let's look at an example of running Javascript and Python unit tests in the same build on Shippable.

The easiest way to do this would be to specify `node_js` as your language in the `shippable.yml` file, since python already comes installed on our node images by default. Your `shippable.yml` should look like this:


```
language: node_js

# specify node version(s)
node_js:
  - "0.12"

build:
  ci:
    # check the python version; it will be 2.7 by default
    - python --version
    # check the node version; it will be whatever you specified under node_js, above
    - node --version
    - npm install
    - pip install -r requirements.txt
    # now run your tests
```


This `yml` configuration should cover a lot of scenarios. If you want a more tailor-made set-up, you can always create a custom image, install what you want in that image, and then use that for your build. Feel free to use our [drydock images](https://github.com/dry-dock) as a starting place; these are our build images. For more info on how to use custom images with a build, check out our [docs](ci/shippableyml/#pre-ci-boot).


---

### How do I identify the node on which the job ran?

There are two ways to identify the node on which the job ran:

1. In the UI, the job console has a section called `Job node info`, under which is `uname -a` that shows you the hostname of the node
1. For API users, each job has a "nodeId" property that can be used to identify the node

While the job is running, you can view the job being run on a particular node by going into your Subscription Settings>Nodes section

---
