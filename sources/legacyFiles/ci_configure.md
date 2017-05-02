page_title: Shippable Build Configuration
page_description: How to write your Shippable YML and Set up your Build Configuration
page_keywords: getting started, questions, documentation, shippable, config, yml

# Configure your build

All build configuration happens through shippable.yml which should be present at the root of the repository you want to build using Shippable. The following sections describe the overall structure of the shippable.yml file, as well as detailed descriptions of every section in it.

* * * 

##shippable.yml structure

At a high level, the yml structure for CI is as shown below, along with an explanation of what belongs in each section.
<img src="../images/ci_yml_structure.png" alt="Account Settings Subscription" style="width:700px;"/>

To get started, you can copy the following structure into your shippable.yml and then customize it based on instructions in the sections below.

```
#set your language below
language: python

#set language version. This is only required if you use the default Shippable image for your build
python:
    - 2.7

#specify which services you need. This is only valid if you use the default Shippable image for your build
services:

env:

matrix:

build:
    #commands in this section run on your build machine and not inside your CI container.
    pre_ci:

    pre_ci_boot:
        image_name:
        image_tag:
        pull:
        options:
    #commands in this section run inside your CI container. In addition, any commands such as ls, in this section can be used to skip default processing for CI.
    ci:

    post_ci:
    on_success:
    on_failure:
    cache:
    #commands in this section run on your build machine and not inside your CI container.
    push:



integrations:
#if you are using integrations, you'll need to set it up in two places. Subscription settings & here in this section of the yml.
    notifications:
        - integrationName:
          type:
          recipients:
          branches:
          on_start:
          on_success:
          on_failure:
          on_changed:   

    hub:
        - integrationName:
          type:
          agent_only:
          branches:
```

Details on what can be set in each section of the `shippable.yml` are listed below.

* * * 

## Specifying language and runtime

The following language tags are supported at this time:

```clojure```

```go```

```java```

```node_js```

```php```

```python```

```ruby```

```scala```

```none```

```c/c++```

You can set the language and runtime as shown below. Example is for node.js projects.

```
language: node_js

node_js:
  - "0.10"
```

Please note that you can specify language versions as number or string, i.e. as `0.10` or as `"0.10"`. In most cases the format is entirely interchangeable. However, in cases where the version number ends with a 0, such as `5.10`, it is safer to use a string to avoid the yml parser from transating the version to `5.1`.

Setting the runtime only works if you are using our default build images or an image pulled from our [drydock repository on Docker Hub](https://hub.docker.com/u/drydock/).

Specific examples for each language are in our [Language guide](ci_languages.md)

Specifying ```language: none``` in your yml skips any default language specific processing and will require you to specify a custom image for your build. Details are in the [Building unsupported languages](#unsupported_languages) section.

* * * 

## Preparing your environment
The `pre_ci` section lets you prepare your environment before your CI container is booted up. This could include building the docker image you want to use for CI, installing dependencies that your container needs, etc.

* * * 

<a name="build_images"></a>
## Setting your build image

To run your build, we spin up Docker containers depending on the language specified in your yml. Our build images are available on Docker Hub under the [dry-dock repository](https://hub.docker.com/r/drydock) and the corresponding Dockerfiles are available in our [GitHub repository dry-dock](https://github.com/dry-dock).

Our standard build images are named as follows:

* The first 3 letters of image name indicate the platform. `u14` denotes ubuntu 14.04, `u12` denotes ubuntu 12.04
* The next 3 letters indicate the language. `nod` for node.js, `sca` for scala, `pyt` for python, `gol` for golang, `rub` for ruby, `clo` for clojure, `jav` for java, `php` for php and `cpp` for c
* The last 3 letters, if present, indicate any additional services that are pre-installed. `pls` indicates that Cassandra, Elasticsearch, Memcached, MongoDB, MySQL, Neo4j, RabbitMQ, Redis, Selenium, SQLLite are already installed on the build minion. `all` indicates that in addition to all services available in pls images, CouchDB, Kestrel, RethinkDB, Riak are also installed.

Exact details on what is included in each image is available in the github repo for the image, as well as the image description on Docker Hub.

### Default build image
By default, we will spin up a build container based on the either the base version or the `all` version of an image for the language specified in your yml. For example, if you specify ```language: python``` in your yml, we will spin up a build minion based on u14pyt:prod. If your build also starts any services using the `services` tag in the yml, we will spin up a build minion based on  u14pytall:prod images.

### Overriding the default build image

You can override which Docker image is used for your CI by specifying a different image in your yml -

```
build:
  pre_ci_boot:
    image_name: manishas/myImage
    image_tag: latest
    pull: true
    options: "-e HOME=/root"
```

The image you specify in this section should be available to Shippable when the build reaches this step. To learn how to build your CI image from a Dockerfile or pull from a registry, check out the sections below.

<a name="add_project_integrations"></a>
###Adding a hub integration to a project

Add a hub integration to your project to do any of the following:

- Pull an image from a private Docker repository
- Build a Docker image which has a `FROM` that pulls an image from a private Docker repository
- Push to a Docker repository

To [add a Hub integration](ci_subscriptions/#integrations) to a project do the following:

-  Go to the 'Settings' tab of your subscription.
-  Under the 'Integrations' section, click the 'Add Integration' button.
-  In the dropdown of the 'Account Integrations' field, select the desired integration for a Docker registry (such as Docker Hub, Amazon ECR, Google Container Registry, Quay.io, Private registry and Docker Trusted Registry). 
-  If you have created the required Docker registry integration before, it'll show up as an option in the dropdown. Select the required integration
-  Configure the integration in the `shippable.yml` file for the project. 
-  Ensure the integration name in the UI and in the `shippable.yml` are exactly the same. 

Check our [Docker Registries Integrations](int_docker_registries/) page for specifics on different Docker Registries such as Docker Hub, Amazon ECR, Google Container Registry, Quay.io, Private registry and Docker Trusted Registry.

###Building your CI image

If you want to build your Docker image as part of your workflow for each CI run, you will need to do the followng in your shippable.yml-

```
build:
  pre_ci:
    - docker build -t myImage:tip .

  pre_ci_boot:
    image_name: myImage
    image_tag: tip
    pull: false
    options: "-e HOME=/root"
```
For your specific case:

* `image_name` value is the name of the image that was built in the `pre_ci` step.
*  `image_tag` is the tag for the image that was built in the `pre_ci` step.  
* set `pull` to `false` if you want to use the image you built during the `pre_ci` step instead of pulling from a docker registry.
* In the `env` section, you can enter any environment variables you want to be set inside your CI container.
* In the `options` tag, enter any docker options you want to use in the `docker run` command. You also need to include the HOME environment variable as shown if it is not already set in your Dockerfile.

The example yml above will ensure that manishas/myImage:tip is used to start the CI container with the option `--privileged=true` and with the environment variable FOO=BAR already set within the container.

**Please note that if your Dockerfile has a `FROM` which pulls a private image from a registry, you will also need to add a hub integration to your project (see section above) and have the following in your yml.**

```
integrations:
    hub:
      - integrationName: "my integration name"
        type: docker

```

The `type` field should be `docker` for Docker Hub, `gcr` for Google container registry, `quay.io` for Quay.io, `ecr` for Amazon EC2 Container registry, and `private docker registry` for a self hosted private registry.

###Pulling your CI image from a Docker registry

You can pull any image you have access to from a Docker registry and use that to spin up your CI build container.

To pull a image from a registry, you will need to do the following-

1. [Skip if you are pulling an image from a public repo] Add a hub integration to your project ([Instructions here](#add_project_integrations)).
2. Add the following in your shippable.yml:

```
pre_ci_boot:
    image_name: manishas/myImage
    image_tag: latest
    pull: true
    options: "-e HOME=/root"

integrations:
    hub:
      - integrationName: manishasDockerHub
        type: docker
        branches:
          only:
            - master

```

For your specific case:

* `image_name` value is in the format (docker-registry-username)/(docker-registry-image-repo). For GCR and ECR, you will to specify image_name in the right format:
    *  GCR: gcr.io/(docker-registry-username)/(docker-registry-image-repo)
    *  ECR: aws_account_id.dkr.ecr.us-east-1.amazonaws.com/repo-name
* `image_tag` is the tag for the image that you want to pull.  
* set `pull` to `true` if you want to pull this image from a docker registry.
* In the `env` section, you can enter any environment variables you want to be set inside your CI container.
* In the `options` tag, enter any docker options you want to use in the `docker run` command. You also need to include the HOME environment variable as shown if it is not already set in your image.
* For `integrationName` tag, enter the name of the account integration you have added to your project. This account should have permissions to pull the the build image specified in the `image_name` setting.
* In the `type` tag, enter the type of registry. Options are `docker` for Docker Hub, `gcr` for Google container registry, `quay.io` for Quay.io, `ecr` for Amazon EC2 Container registry, `private docker registry` for a self hosted private registry and `Docker Trusted Registry` for Docker Trusted Registry.
* [optional]Using the `branches` section, specify the branches this account integration is applicable to. You can skip this if you want your integration to be applicable for all branches.

The example yml above will pull the image manishas/myImage:tip using the integration manishas_dockerhub, and run the container with option `--privileged=true` and set env `FOO=BAR` inside the container.  

* * * 

## The `ci` section

The `ci` section of your yml is where the bulk of your build commands should be included. All commands in this section are executed sequentially inside your build container in the order they appear in your yml.

NOTE: If the `ci` section is blank, then default commands are executed, depending on the language. For more information, [refer the 'Default Commands' section for each language](ci_languages.md).

In general, follow the guidelines below to write the `ci` section:

* First, install or update any required dependencies or packages. Commands like `npm install` or `sudo apt-get update` should be at the top of this section.
* Next, create any databases or folders you need. For example, you could create a mysql database with a `- mysql -e 'create database myapp_test;'` or create folders for test results with the command `- mkdir -p shippable/testresults`
* Next, include commands for your builds and tests. This could be something like `- nosetests python/sample.py --with-xunit --xunit-file=shippable/testresults/nosetests.xml` for a python project.

Depending on the whether your `ci` section is successful or not, the `on_success` or `on_failure` sections will be executed. You can include post build actions depending on your build result in these sections.

An example is shown below:
```
build:
  ci:
    - mkdir -p shippable/testresults
    - mkdir -p shippable/codecoverage
    - npm install
    - grunt
    - npm test
    - mysql -e 'create database if not exists test;'
```

* * * 

## Pushing an image to a registry

After CI is complete, you might want to push your build image to a Docker registry and tag it appropriately. Or you might want to build a new 'production' image without any of your CI artifacts and push that to your Docker registry account.

You can do this in the `post_ci` or `push` sections of your shippable.yml. The main difference is that the `post_ci`section runs inside your CI build container, so **if you want to push to Google Container Registry or Amazon ECR AND you're using a custom image for your build, your custom image would need to have the appropriate cli installed.** More on what is required [here](#gcr_ecr_push). The `push` section runs on the build machine, i.e. outside the build container, so you do not need these CLIs installed on your custom image.

To push your CI build container image to a registry: 

1. Add a Hub integration to your project ([Instructions here](#add_project_integrations))
2. Add the following in your shippable.yml:

**Docker Hub**

```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI step
    - docker commit $SHIPPABLE_CONTAINER_NAME manishas/sample-node:tag
    - docker push manishas/sample-node:tag

integrations:
  hub:
    - integrationName: your_integration_name
      type: docker
      branches:
        only:
          - master

```

The `branches` tag is optional and if it is skipped, the integration is used for all branches.

**GCR**

```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI step
    - docker commit $SHIPPABLE_CONTAINER_NAME gcr.io/manishas/sample-node:tag
    - docker push gcr.io/manishas/sample-node:tag

integrations:
  hub:
    - integrationName: your_integration_name
      type: gcr
      branches:
        only:
          - master

```

The `branches` tag is optional and if it is skipped, the integration is used for all branches.

**Amazon ECR**

```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI step
    - docker commit $SHIPPABLE_CONTAINER_NAME 1234567890.dkr.ecr.us-west-2.amazonaws.com/sample-node:tag
    - docker push 1234567890.dkr.ecr.us-west-2.amazonaws.com/sample-node:tag

integrations:
  hub:
    - integrationName: your_integration_name
      type: ecr
      region: us-west-2
      branches:
        only:
          - master

```

The `branches` tag is optional and if it is skipped, the integration is used for all branches.
The `region` field is set to `us-east-1` by default. You can override it in the yml as shown above.

**Quay**

```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI step
    - docker commit $SHIPPABLE_CONTAINER_NAME manishas/sample-node:tag
    - docker push manishas/sample-node:tag

integrations:
  hub:
    - integrationName: your_integration_name
      type: quay.io
      branches:
        only:
          - master

```

The `branches` tag is optional and if it is skipped, the integration is used for all branches.

**Private Docker Registry**

```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI step
    - docker commit $SHIPPABLE_CONTAINER_NAME your_registry_URL/sample-node:tag
    - docker push your_registry_URL/sample-node:tag

integrations:
  hub:
    - integrationName: your_integration_name
      type: "private docker registry"
      branches
        only:
          - master

```

The `branches` tag is optional and if it is skipped, the integration is used for all branches.

**Docker Trusted Registry**

```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI step
    - docker commit $SHIPPABLE_CONTAINER_NAME ship-dtr.in/sdemo/sample_app:$BRANCH.$BUILD_NUMBER
    - docker push ship-dtr.in/sdemo/sample_app:$BRANCH.$BUILD_NUMBER

integrations:
  hub:
    - integrationName: "ship-ddc-dtr"
      type: "Docker Trusted Registry"
      branches:
        only:
          - master

```

In the above example: `ship-dtr.in` is the Docker Trusted Registry URL, `sdemo` is the username, `sample_app` is the image and `$BUILD.$BUILD_NUMBER` is the tag for the image.

The `branches` tag is optional and if it is skipped, the integration is used for all branches.


### Building and pushing a new production image after CI

If you want to run your CI on one image and after successful CI, build a new 'production ready' image and push to a docker registry, you can do so in the `post_ci` section: 

```
build:
  post_ci:
    - docker build -t manishas/sample-node-prod .
    - docker push manishas/sample-node-prod

integrations:
  hub:
    - integrationName: your_integration_name
      type: docker
      branches:
        only:
          - master

```
### Pushing to multiple registries

An image can be pushed to multiple registries by tagging it and specifying the required hub integration in the `hub` section.

Here, the image `manishas/sample-node-prod` is built and tagged as `gcr.io/manishas/sample-node-prod` and then pushed to the Google Cloud Registry using an integration of type `gcr`.
```
build:
  post_ci:
    - docker build -t manishas/sample-node-prod .
    - docker push manishas/sample-node-prod
    - docker build --rm -t=gcr.io/manishas/sample-node-prod .
    - docker push gcr.io/manishas/sample-node-prod

integrations:
  hub:
    - integrationName: docker_integration_name_one
      type: docker
    - integrationName: docker_integration_name_two
      type: docker
    - integrationName: gcr_integration_name
      type: gcr

```
**Please note that if you specify multiple hub integrations for a registry, the last one will be used to push the image.**

In the example above, there are two `docker` integrations specified along with one `gcr` integration. In this case, only `docker_integration_name_two` and `gcr_integration_name` will be used to push the image to the respective registries.

### Pushing multiple tags
In most cases, you will push your image to a Docker registry with one tag. However, there are times when you need to add multiple tags. For example, you might want to tag a container with `tip` as well as the build number.

Here is an example of how to set this up in your yml:

```
build:
  post_ci:
    - docker tag -f manishas/sample-node:latest manishas/sample-node:tip
    - docker tag -f manishas/sample-node:latest manishas/sample-node:$BUILD_NUMBER
    - docker push manishas/sample-node:tip
    - docker push manishas/sample-node:$BUILD_NUMBER
```
In the above example, replace the repo/image name with your image name and the tags with the ones you need for your image.

<a name="gcr_ecr_push"></a>
### GCR/ECR and custom images
All standard images in our [drydock repository on Docker Hub](https://hub.docker.com/u/drydock/) have the required CLIs preinstalled for integration with GCR and ECR, so you can run a docker build or push in any section of your yml with no effort. **This section is relevant only if you're using a custom image for your build and pushing to GCR or ECR.**

If you are using a custom image, the story is a little different. If you specify a `gcr` or `ecr` integration in your yml, we will try to login to the registry on your behalf from inside your CI build container. This means that for custom images, you would need the gcloud SDK or aws cli installed inside your custom image if you want this to succeed, else you will get a `gcloud not found` or `aws: command not found` errors.

You can get around this requirement by setting `agent_only: true` for your hub integration.

```
integrations:
  hub:
    - integrationName: gcr_int_name
      type: gcr
      agent_only: true
```

This will ensure that we will not attempt to login to the registry from inside your CI build container. However, this also means that you will not be able to pull from or push to GCR/ECR in the `ci`, `post_ci`, `on_success` and `on_failure` sections.

You can always choose to push your image to GCR/ECR in the `push` section which runs at the end of the build on the build machine, i.e. outside your CI container-

```
build:
  push:
    - docker push manishas/sample-node:$BRANCH.$BUILD_NUMBER
```

If you do want to use docker commands to interact with GCR or ECR in your `ci`, `post_ci`, `on_success` or `on_failure` sections, check out the sections below explaining how to install gcloud SDK or aws CLI as required.


####gcloud SDK
This is required if you want to pull from or push to GCR in your `ci`, `post_ci`, `on_success` or `on_failure` sections. Include the following in your Dockerfile to install the gcloud SDK:

```
echo "================= Adding gclould binaries ============"
CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)"
echo "deb http://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | sudo tee /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update && sudo apt-get install google-cloud-sdk
```

####AWS CLI

This is required if you want to pull from or push to Amazon's ECR in your `ci`, `post_ci`, `on_success` or `on_failure` sections.. Include the following in your Dockerfile to install the aws cli:

```
sudo pip install awscli
```

<a name="matrix_builds"></a>

* * * 

## Running multiple builds per commit

In most cases, you want to trigger one build for each commit/pull request to your repository. However, there are times when you might want to trigger multiple builds for a single code change. For example, you might want to test against multiple versions of Ruby, multiple aspect ratios for your Selenium tests, or multiple environment variables.

This scenario is handled by our matrix build feature, so the following yml configs will trigger multiple builds -

- specifying more than one language version
- specifying more than one variable in the ```env``` section
- specifying multiple gemfiles for ruby



```yaml
rvm:
  - 1.9.2
  - 1.9.3
  - rbx
  - jruby

gemfile:
  - gemfiles/Gemfile.rails-2.3.x
  - gemfiles/Gemfile.rails-3.0.x
env:
  - ISOLATED=true
  - ISOLATED=false
```

The above example will fire 16 different builds for each push. Whoa! Need more minions?

### including/excluding versions

You can also exclude a specific version by configuring your yml with an `exclude` tag:

```
matrix:
  exclude:
    - rvm: 1.9.2
```

To include only specific combinations of a matrix build, use the `include` as shown below:
with include tag.

```
matrix:
  include:
    - rvm: 2.0.0
      gemfile: gemfiles/Gemfile.rails-3.0.x
      env: ISOLATED=false
```

### allowing failures

Allowed failures are items in your build matrix that are allowed to fail without causing the entire build to be shown as failed. You can define allowed failures in the build matrix as follows:

```yaml
matrix:
  allow_failures:
    - rvm: 1.9.3
```

Please note that you can specify language versions as number or string, i.e. as `0.10` or as `"0.10"`. In most cases the format is entirely interchangeable. However, in cases where the version number ends with a 0, such as `5.10`, it is safer to use a string to avoid the yml parser from transating the version to `5.1`.

As an example, check out this tutorial on [testing a node.js app against multiple versions of node](http://blog.shippable.com/how-to-test-your-node.js-app-against-multiple-versions-of-node). 
* * * 

## Using environment variables

### Standard variables

The following environment variables are available for every build. You can use these in your scripts if required:


| Env variable        | Description           |
| ------------- |-------------|
|BASE_BRANCH		 | Name of the target branch into which the pull request changes will be merged.|
|BRANCH		 | Name of branch being built.|
|BUILD_NUMBER		 | Build number for current build.|
|BUILD_URL		 | Direct URL link to the build output.|
|CACHE_CONTAINER    | false |
|CACHE_DIR    |  If cache is true in the build section of `shippable.yml`, then **true**.  Otherwise **false**.  |
|CI		 | true|
|COMMIT  |Commit id that is being built and tested. |
|COMMITTER  |Name of the last committer. |
|COMMIT_MESSAGE  |Commit message specified in the commit. |
|COMPARE_URL		 |A link to GitHub/Bitbucket's comparision view for the push. |
|CONTINUOUS_INTEGRATION	 |true |
|DEBIAN_FRONTEND		 |noninteractive |
|HEAD_BRANCH		 | This is only set for pull requests and is the name of the branch the pull request was opened from.|
|IS_PULL_REQUEST     |Set to **true** if the job is a pull request. If not, this will be set to **false**. |
|IS_FORK    |Set to **true** if the job belongs to a forked project. If not, this will be set to **false**. |
|JOB_ID		 | ID of job in Shippable.|
|JOB_NUMBER, SHIPPABLE_JOB_NUMBER, SHIPPABLE_JOB_ID | All three variables are the same & represent the number of the job in Shippable.|
|LANG		 |en_US.UTF-8 |
|LAST_AUTHOR |Name of the last author. |
|LAST_SUCCESSFUL_BUILD_TIMESTAMP		 |Timestamp of the last successful build in seconds. This will be set to **false** for the first build or for the build with no prior successful builds. |
|LC_ALL 		 |en_US.UTF-8 |
|LC_CTYPE 		 | en_US.UTF-8|
|MERB_ENV 		 |test |
|ORG_NAME     | Name of the organization/user that owns the repository currently being built (eg. This will be set to `Shippable` if the full name is `Shippable/support`).|
|PATH		 | $HOME/bin:$PATH:$HOME/usr/local/bin|
|PROJECT_ID | ID of the Shippable Project. |
|PULL_REQUEST		 |Pull request number if the job is a pull request. If not, this will be set to **false**. |
|PULL_REQUEST_BASE_BRANCH | Name of the branch that the pull request will be merged into. It should be the same as BASE_BRANCH.|
|PULL_REQUEST_REPO_FULL_NAME | Full name of the repository from where the pull request originated.|
|RACK_ENV 		 | test|
|RAILS_ENV		 |test |
|REPO_FULL_NAME     | Full name of the repository currently being built (eg. `Shippable/support`).|
|REPO_NAME 		 | Name of the repository currently being built (eg. This will be set to `support` if the full name is `Shippable/support`).|
|REPOSITORY_URL 		 |URL of your GitHub or Bitbucket repository. |
|SERVICE_SKIP 		 |false |
|SHIPPABLE		 | true|
|SHIPPABLE_ARCHIVE 		 | true|
|SHIPPABLE_BUILD_DIR | Directory where the repository is cloned. |
|SHIPPABLE_BUILD_ID 		 |ID of build in Shippable. |
|SHIPPABLE_BUILD_NUMBER | Build number for current build. |
|SHIPPABLE_BUNDLER_ARGS  | The value of bundler_args in the build section of `shippable.yml`. |
|SHIPPABLE_COMMIT_RANGE  | Parent commit… current commit being built.  |
|SHIPPABLE_DATA_DIR | $HOME/data|
|SHIPPABLE_GEMFILE | The gemfile specified for the job in the `shippable.yml`. |
|SHIPPABLE_JDK_VERSION | The jdk for the job in the `shippable.yml`. |
|SHIPPABLE_MYSQL_BINARY		 |"/usr/bin/mysqld_safe" |
|SHIPPABLE_MYSQL_CMD		 |"\$SHIPPABLE_MYSQL_BINARY" |
|SHIPPABLE_OS_NAME | linux|
|SHIPPABLE_POSTGRES_VERSION		 | "9.2"|
|SHIPPABLE_POSTGRES_BINARY		 |"/usr/lib/postgresql/\$SHIPPABLE_POSTGRES_VERSION/bin/postgres" |
|SHIPPABLE_POSTGRES_CMD 		 | "sudo -u postgres \$SHIPPABLE_POSTGRES_BINARY -c "config_file=/etc/postgresql/\$SHIPPABLE_POSTGRES_VERSION/main/postgresql.conf""|
|SHIPPABLE_REPO_DIR | The directory where builds run. |
|SHIPPABLE_REPO_SLUG | Full name of the repository being built (e.g. `Shippable/support`). |
|SHIPPABLE_SELENIUM_PORT | 4444|
|SHIPPABLE_SELENIUM_BINARY |Location of selenium binary. It is set only if selenium is in the addons or services in the `shippable.yml`. |
|SHIPPABLE_SUBMODULE_ENABLED | Whether or not submodules in the repository will be updated. |
|SHIPPABLE_VE_DIR		 | "\$HOME/build_ve/python/2.7"|
|SUBSCRIPTION_ID | ID of the Subscription. |
|IS_GIT_TAG | Set to **true** if the build is triggered by a git tag push webhook. If not, this will be set to **false**. This env variable is currently supported for GitHub only.|
|GIT_TAG_NAME | The git tag name if the build is triggered by a git tag push webhook or a release webhook. This env variable is currently supported for GitHub only.|
|IS_RELEASE | Set to **true** if the build is triggered by a release webhook. If not, this will be set to **false**. This env variable is currently supported for GitHub only.|
|IS_PRERELEASE | Set to **true** if the release is marked pre-release when it was published. If not, this will be set to **false**. This env variable is currently supported for GitHub only.|
|RELEASED_AT | The timestamp when the release was published. This env variable is currently supported for GitHub only.|
|RELEASE_NAME | The name of the release webhook. This env variable is currently supported for GitHub only.|

#### Language based:
Based on the language used, the following environment variables should be used.

| Env variable        | Description           |
| ------------- |-------------|
| SHIPPABLE_LEIN_VERSION		 | Clojure version specified for the job.|
| SHIPPABLE_GO_VERSION | GO version specified for the job. |
| SHIPPABLE_GOPATH | PATH set to $HOME |
| SHIPPABLE_NODE_VERSION | NodeJS version specified for the job. |
| SHIPPABLE_PHP_VERSION | PHP version specified for the job. |
| SHIPPABLE_PYTHON_VERSION | Python version specified for the job. |
| SHIPPABLE_RUBY | Ruby version version specified for the job. |
| SHIPPABLE_SCALA_VERSION | Scala version specified for the job. |

#### Travis compatible Variables:
Given below are the variables, supported by Shippable.

| Env variable        | Description           |
| ------------- |-------------|
| TRAVIS		 | true|
| TRAVIS_OS_NAME | linux|
| TRAVIS_BUILD_DIR | |
| TRAVIS_REPO_SLUG | |
| TRAVIS_COMMIT_RANGE | |
| TRAVIS_BUILD_NUMBER | |
| TRAVIS_JOB_NUMBER | |
| TRAVIS_BUILD_ID | |
| TRAVIS_JOB_ID | |
| TRAVIS_BRANCH | |
| TRAVIS_COMMIT | |
| TRAVIS_PULL_REQUEST | |
| TRAVIS_NODE_VERSION | |
| TRAVIS_PHP_VERSION | |
| TRAVIS_PYTHON_VERSION | |
| TRAVIS_RUBY_VERSION | |
| TRAVIS_SCALA_VERSION | |


### Custom Variables
You can also set your own environment variables in the yml. Each statement under the ```env``` tag will trigger a separate build with that env variable, so specifying multiple environment variables will give you a build matrix for every commit.

```yaml
# environment variable
env:
 - FOO=foo BAR=bar
 - FOO=bar BAR=foo
```

Env variables can create an exponential number of builds when combined with `jdk` & `rvm , node_js etc.` i.e. it is multiplicative. For an example, please check out the Build Matrix section above. To avoid a build matrix and kick off a single build with all environments, you can use the global tag as detailed in the 'Combining variables in a single build' section below.


### Secure variables

Shippable allows you to encrypt environment variables and keep your configurations private using `secure` tag.

To do this,

1. Encrypt your environment variables ([Instructions here](ci_projects.md#encrypting-your-environment-variables))
2. Copy the encrypted output string and add it to your yml file as shown below:

```
env:
  secure: <encrypted output>
```

### Multiple variables per build

You can combine multiple environment variables in the same build using the `global` tag. This will prevent a build matrix for being triggered and all your variables will be defined for one build.

```yaml
env:
  global:
    - FOO="bar"
    - secure: <encrypted output>
```


####Injecting Global Env Variables
You can also inject global environment variables into the new build by specifying key-value pairs in the request body while [triggering a new build](api.md#trigger-a-new-run) . These key-value pairs have to be set in the `globalEnv` property.

```
{
  "globalEnv": {
    "FOO": "bar",
    "FIZZ": "buzz"
  }
}
```

To encrypt multiple environment variables separately, configure your yml
file as shown below:

```
env:
  global:
    #encrypted output of first env variable
    - secure: <encrypted output>
    #encrypted output of second env variable
    - secure: <encrypted output>
  matrix:
    #encrypted output of third env variable
    - secure: <encrypted output>
```

> **Note**
>
> Due to the security risk of exposing your secure variables, we do not
> decrypt secure variables for pull request from the forks of public
> projects. Secure variable decryption is limited to the pull request
> triggered from the branches on the same repository. And the decrypted
> secured variables are also not displayed in the script tab for
> security reasons.

* * * 

## Caching

You can turn on caching for your builds by including `cache: true` in the `build` section of your shippable.yml. This will cache contents of the build directory $SHIPPABLE_BUILD_DIR.

```
build:
  cache: true
```

You can also choose to cache specific folders instead of the entire build directory by using the `cache_dir_list` tag. The cache_dir_list is an array of **absolute path** of the folders that needs to be cached. Please note that you still need the `cache: true` in your yml:

```
build:
  cache: true
  cache_dir_list:
    - absolute path of dir1
    - absolute path of dir2
    - absolute path of dir3
```

For example, to cache node modules and the .git folder, you would specify the following:

```
build:
  cache: true
  cache_dir_list:
    - $SHIPPABLE_BUILD_DIR/node_modules
    - $SHIPPABLE_BUILD_DIR/.git
```

Cache is updated for every build and is available to subsequent builds.

### Clearing cache
You can clear cache in one of two ways:

* Including [reset minion] or [reset_minion] in your commit message.
* Clicking on the `Clear cache` in your Project settings UI.

In both cases, your cached image will be deleted. If cache is still set to true in your yml, the build will generate a new cache which will be used for subsequent builds. This method is the best way to update your cache if required.

### Removing unwanted files when caching is enabled
When caching is enabled, the entire build directory including artifacts (when `cache: true` is used) or the entire folder contents (if configured for specific folders) is cached. This may include unwanted files. Unless these unwanted files are removed, your tests could fail during the CI build, as Shippable updates the cached directory with the new commits. Remove the unwanted files in one of the two ways:

* Use the `git clean` command: In your `shippable.yml` add the `git clean` in the `ci` section using the following format:
    - `git clean -f` removes all untracked files not ignored by `.gitignore`.
    - `git clean -f -X` removes all untracked files included in `.gitignore` only.
    - `git clean -f -x` removes all untracked files.

* Add commands such as `rm` to remove the unwanted files in your `shippable.yml` in the `ci` section.  

* * * 

## Retrying a command

Sometimes, commands like `npm install` fail due to the intermittent network issues and this affects your build result. To avoid this, you can use `shippable_retry` in the yml to try the command up to 3 times if it returns a non-zero code.

`shippable_retry` functionality is available for all default installation commands. You can
also use it for any custom installation from external resources. For example:

```
build:
  ci:
    - shippable_retry sudo apt-get update
    - shippable_retry sudo apt-get install something
```

For more examples, refer our blog ["Automatically retry scripts to avoid network hiccups during CI process"](http://blog.shippable.com/automatically-retry-scripts-to-avoid-network-hiccups-during-ci-process). 
* * * 

## Using git submodules

Shippable supports git submodules. For big projects, you can break your projects down into manageable chunks and use git submodules to make everything work. We automatically initialize the `.gitmodules` file in the root of the repo.

> **Note**
>
> If you are using private repos, you will need to add the deploy keys so that our minion
> ssh keys are allowed to pull from the repo.

If your submodules are in your own public repos then the following will work:

```python
# for public modules use
https://github.com/someuser/somelibrary.git

# for private modules use
git@github.com:someuser/somelibrary.git
```

If you want to turn off submodules completely:

```yaml
# for public modules use
git:
 submodules: false
```

* * * 

## Including/excluding branches

By default, Shippable builds all branches for enabled repositories. If a branch does not have a shippable.yml at its root, we will create a build and show an error in the build console.

You can choose to build specific branches by using the `branches` sections in your yml. The specific branch that is being included or excluded needs to have this configuration, and not just the master branch. When we get a webhook for an enabled repository, we read the shippable.yml from the branch that has changed and trigger a build using that yml. So unless the yml in the branch to be included/excluded has the right settings, we are not aware of it and will trigger a build as expected.  

Wildcard entries for branches and git-flow branches, are supported.

Here are some examples of the include/exclude config -

```
# this config will build all branches and exclude the following: test1, experiment2, all branches beginning with "dev" and all git-flow branches in the "feature" branch
branches:
  except:
    - test1
    - experiment2
    - dev*
    - feature/*
```
```
# this config will only build the following branches: stage,  prod, all branches beginning with "beta" and all git-flow branches in the "release" branch
branches:
  only:
    - stage
    - prod
    - beta*
    - release/*
```

* * * 

<a name="test_code_coverage"></a>
## Test and Code Coverage Reports

Shippable can show you test and code coverage results in a consumable format where you can drill down further and find out which tests failed or which sections of your code were not covered by your tests.

Your tests results data needs to be in junit format and your code coverage results need to be in cobertura format in order to see these visualizations.

### Test Results

To set up test result visualization for a repository, do the following:

- Run tests as part of your CI workflow using shippable.yml
- Make sure test results are in junit format.
- Output test results to shippable/testresults folder.

For example, here is a sample configuration for a Python project -

```yaml
build:
  ci:
    - nosetests python/sample.py --with-xunit --xunit-file=shippable/testresults/nosetests.xml
```

Examples for other languages can be found in our [Code Samples](ci_languages/).

Once you have set this up, you can view your test results in the `Test` tab on your build page.


### Code Coverage

To set up code coverage result visualization for a repository, do the following:

- Run your code coverage command(s) as part of your CI workflow using shippable.yml
- Make sure code coverage output is in cobertura xml format.
- Output code coverage output to shippable/codecoverage folder.

For example, here is a sample configuration for a Python project -

```yaml  
build:
  ci:  
    - coverage run --branch python/sample.py
    - coverage xml -o shippable/codecoverage/coverage.xml python/sample.py
```

Examples for other languages can be found in our [Code Samples](ci_languages/).

Once you have set this up, you can view your code coverage results in the `Code coverage` tab on your build page.

Check out our blogs on [creating visualizations of your CI test results](http://blog.shippable.com/automatically-retry-scripts-to-avoid-network-hiccups-during-ci-process) and [setting up code coverage for tests](http://blog.shippable.com/setting-up-code-coverage-visualization-for-tests-in-ci) for additional details.
* * * 

## Downloading console logs
You can download your console logs by going to the build's page on Shippable, clicking on the `Download` dropdown that's on the right of the console pane, and selecting your logs.

* * * 

## Build artifacts
Shippable does not store artifacts for your builds. You will need to handle uploading artifacts as part of your configuration in your shippable.yml.

For example, the sample snippet below copies the entire build directory to an AWS S3 bucket which is in the region us-east-1:

```
env:
  global:
    #secure variable contains values for AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY
    - secure: HKwYujx/qmsyQQdHvR2myu8HLUDtcLeDyYV149YJuxIV4J7Hk3SxeY8X3D6aTlR8mvMnd/ZFY+tGNUh4G0xtLLjjZcPsBgvFlB

build:

  on_success:
    - aws s3 sync $SHIPPABLE_BUILD_DIR "s3://bucket_name" --region "us-east-1"


```

Similarly, you can copy contents of any folder with `$SHIPPABLE_BUILD_DIR/folder_name` depending on where the artifacts are for your build.
If you need help defining secure variables, you can check out [our instructions](ci_projects/#encrypting-your-environment-variables)

* * * 

## Notifications

Shippable supports email, Slack, HipChat and IRC notifications and these can be configured in your yml file.

By default, we send email notifications to the last committer when a build fails, or the status changes from failed to passed..

You can change the notification settings by configuring the integrations section of your yml. Details for each supported provider are below.


### Email notifications

By default, we send email notifications to the last committer and project owner when a build fails, or the status changes from failed to passed. We get the email address from your source control management system (GitHub/Bitbucket).

Adding email notifications through UI (Subscription settings) is not required. 

To customize email notifications, use the yml structure below:

```yaml
integrations:
  notifications:
    - integrationName: email
      type: email
      recipients:
        - exampleone@org.com
        - exampletwo@org.com
      branches:
        only:
          - master
          - dev
      on_success: always
      on_failure: always
```

* `integrationName` is always `email` since you do not configure emails in account integrations or 'Subscription' settings.
* `type` is `email`
* `recipients` specifies the email addresses you want to send build status notifications to. This overrides the default setting of 'last committer' and 'project owner(s)' email address that we get from your source control management system (GitHub/Bitbucket). NOTE: We do not use the email address specified in your 'Account Settings' for notifications. 
    - To specify 'last committer' and 'project owner(s)' as part of this list, you can use `--last_committer` and `--owners`.
    - If there is a single recipient, you can use the format `recipients: example@org.com`
* [optional] `branches` allows you to choose the branches you want to send notifications for. By default, notifications are sent for all branches. The `only` tag should be used when you want to send notifications to specific branches. You can also use the `except` tag to exclude specific branches.
* [optional] You can set the following options for the `on_success`, `on_failure` tags :
    - `change` for `on_success` or `on_failure` means you will receive notifications only when the build status changes to success or failure respectively.
    - `always` means that you will always receive a notification for that build status
    - `never` means that you will never receive a notification for that build status
   By default, `on_success` is set to `change` and `on_failure` is set to `always`.
* [optional] You can set the following options for the `on_start`, `on_pull_request` tags :
    - `always` means that you will always receive a notification for build start/pull request
    - `never` means that you will never receive a notification for that build start/pull request
  By default, `on_start` is set to `never` and `on_pull_request` is set to `always`.    

If you do not want to get notified for any reason, you can turn off email notifications with the following in your yml:

```yaml
notifications:
  - integrationName: email
    type: email
    on_success: never
    on_failure: never
    on_pull_request: never
```

### Slack notifications

To send Slack notifications, you will need to do the following:

1. **Add Slack Integration in the UI**: 
     - Go to the 'Settings' tab of your subscription.
     - Under the 'Integrations' section, click the 'Add Integration'  button.
     - Give a name to the Slack integration for your subscription, for example: `my_slack_integration`
     - In the dropdown of the 'Account Integrations' field, select 'Slack'. 
     - If you have created the Slack integration before, it'll show up as an option in the dropdown. Select the Slack integration, click the `Save` button and go directly to step 2.
     - If Slack does not show up in the list of dropdowns, click the '+ Add  integration' option.
    - Add the Slack integration. [Access instructions here for help](int_notifications/#slack-notifications).

2. **Add the following in your shippable.yml**:

```yaml
integrations:
  notifications:
    - integrationName: my_slack_integration
      type: slack
      recipients:
        - "#channelOne"
        - "#channelTwo"
      branches:
        only:
          - master
          - dev
      on_success: never
      on_failure: always
```
* `integrationName` value is the name of the Slack integration you added to the 'Subscription' settings in Step 1. **IMPORTANT** The names in the 'Subscription' 'Integration' settings in Step 1 and the `shippable.yml` have to be exactly the same. If they are not, the build will fail with an error as [described here](ci_troubleshoot/#integration-name-specified-in-yml-does-not-match).
* `type` is slack
* `recipients` specifies the channels you want to send the notification to. Please note that this is a required field for slack notifications to work. 
    - If there is a single recipient, use the format `recipients: "#channelOne"`
* [optional] `branches` allows you to choose the branches you want to send notifications for. By default, notifications are sent for all branches. The `only` tag should be used when you want to send notifications to specific branches. You can also use the `except` tag to exclude specific branches.
* [optional] You can set the following options for the `on_success`, `on_failure` tags :
    - `change` for `on_success` or `on_failure` means you will receive notifications only when the build status changes to success or failure respectively.
    - `always` means that you will always receive a notification for that build status
    - `never` means that you will never receive a notification for that build status
  By default, `on_success` is set to `change` and `on_failure` is set to `always` if Slack is configured in the yml but you do not specify these tags.
* [optional] You can set the following options for the `on_start`, `on_pull_request` tags :
    - `always` means that you will always receive a notification for build start/pull request
    - `never` means that you will never receive a notification for that build start/pull request
  By default, `on_start` is set to `never` and `on_pull_request` is set to `always` if Slack is configured in the yml but you do not specify these tags.     


Check our blog ["Notifying CI failure/success status on Email and Slack"](http://blog.shippable.com/notifying-ci-failure/success-status-on-email-slack) for multiple scenarios.

### HipChat notifications

To send HipChat notifications, you will need to do the following:

1. **Add HipChat Integration in the UI**: 
     - Go to the 'Settings' tab of your subscription.
     - Under the 'Integrations' section, click the 'Add Integration' button. 
     - Give a name to the Slack integration for your subscription, for example: `my_hipchat_integration`
     - In the dropdown of the 'Account Integrations' field, select 'HipChat'. 
     - If you have created the HipChat integration before, it'll show up as an option in the dropdown. Select the HipChat integration, click the `Save` button and go directly to step 2.
    - If HipChat does not show up in the list of dropdowns, click the '+Add interation' option. 
    - Add the HipChat integration. [Access instructions here for help](int_notifications/#hipchat-notifications).

2. **Add the following in your shippable.yml**:


```yaml
integrations:
  notifications:
    - integrationName: my_hipchat_integration
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
* `integrationName` value is the name of the HipChat integration you added to the 'Subscription' settings in Step 1. **IMPORTANT** The names in the 'Subscription' 'Integration' settings in Step 1 and the `shippable.yml` have to be exactly the same. If they are not, the build will fail with an error as [described here](ci_troubleshoot/#integration-name-specified-in-yml-does-not-match).
* `type` is hipchat
* `recipients` specifies the rooms and/or users you want to send the notification to.
    - If there is a single recipient and it is a room, you can use the format `recipients: "#channelOne"`
    - If there is a single recipient and it is a user, you can use the format `recipients: "@userOne"`
* [optional] `branches` allows you to choose the branches you want to send notifications for. By default, notifications are sent for all branches. The `only` tag should be used when you want to send notifications to specific branches. You can also use the `except` tag to exclude specific branches.
* [optional] You can set the following options for the `on_success`, `on_failure` tags :
    - `change` for `on_success` or `on_failure` means you will receive notifications only when the build status changes to success or failure respectively.
    - `always` means that you will always receive a notification for that build status
    - `never` means that you will never receive a notification for that build status
  By default, `on_success` is set to `change` and `on_failure` is set to `always` if HipChat is configured in the yml but you do not specify these tags.
* [optional] You can set the following options for the `on_start`, `on_pull_request` tags :
    - `always` means that you will always receive a notification for build start/pull request
    - `never` means that you will never receive a notification for that build start/pull request
  By default, `on_start` is set to `never` and `on_pull_request` is set to `always` if HipChat is configured in the yml but you do not specify these tags.

Check our blog on [configuring HipChat for both CI & piplelines](http://blog.shippable.com/hipchat-integration-is-here).

### IRC notifications

You can send notifications to public IRC rooms using Shippable. Private IRC support is coming up soon.

Adding IRC notifications through UI (Subscription settings) is not required. 
Use the following yml structure to send IRC notifications:

```yaml
integrations:
  notifications:
    - integrationName: irc
      type: irc
      recipients:
        - "chat.freenode.net#channel1"
        - "chat.freenode.net#channel2"
      branches:
        only:
          - master
          - test
      on_success: never
      on_failure: always
```

* `integrationName` for public channels is `irc`.
* `type` is `irc`
* `recipients` specifies the rooms you want to send the notification to.
    - If there is a single recipient, you can use the format `recipients: "chat.freenode.net#channel2"`
* [optional] `branches` allows you to choose the branches you want to send notifications for. By default, notifications are sent for all branches. The `only` tag should be used when you want to send notifications to specific branches. You can also use the `except` tag to exclude specific branches.
* [optional] You can set the following options for the `on_success`, `on_failure` tags :
    - `change` for `on_success` or `on_failure` means you will receive notifications only when the build status changes to success or failure respectively.
    - `always` means that you will always receive a notification for that build status
    - `never` means that you will never receive a notification for that build status
  By default, `on_success` is set to `change` and `on_failure` is set to `always` if IRC is configured in the yml but you do not specify these tags.
* [optional] You can set the following options for the `on_start`, `on_pull_request` tags :
    - `always` means that you will always receive a notification for build start/pull request
    - `never` means that you will never receive a notification for that build start/pull request
  By default, `on_start` is set to `never` and `on_pull_request` is set to `always` if IRC is configured in the yml but you do not specify these tags.     


* * * 

## Event Triggers
Shippable supports triggers on user-specified webhook URLs or other enabled projects.

By default, configured triggers are hit only if a commit build succeeds. You can configure the integration in the yml according to your scenarios.

This can be done by following the steps below:

1. **Configure `Event Trigger` Integration**: Go to the 'Settings' tab of your project & under the Integrations section, click the 'Select notification integration' dropdown. If you have created the `Event Trigger` integration before, it'll show up as an option in the dropdown. Select the Event Trigger integration and go directly to step 2.

    - If this is the first time, click the 'Create Integration' option.
    - In the 'New Notification Integration' section, select 'Event Trigger' from the 'Master Integration' dropdown.
    - Give a name to your Event Trigger integration.
    - From the dropdown under **What would you like to trigger?**, you can select either **`Project`** or **`Generic Webhook`**.
    - On selecting **`Project`**, you can choose a project to trigger a run for from the `Project` dropdown. In the `Authorization` field, you need to specify your Shippable [API token](acc_overview/#api-tokens).
    - On selecting **`Generic Webhook`** you can specify a webhook URL you want to trigger in the `WebhookURL` field. In the `Authorization` field, you need to specify the **HTTP Authorization Header** required to hit the specified webhook URL. If no authorization is required, this field can be left blank.
    - Click 'Save Integration'.
    - If you want to view the list of all integrations configured for your subscription, [access instructions here](ci_projects.md#enabling-integrations).

2. **Add the following in your shippable.yml**:

```yaml
integrations:
  notifications:
    - integrationName: my_webhook_integration
      type: webhook
      payload:
        - isPRBuild=$IS_PULL_REQUEST
        - branchName=$BRANCH
        - message=Shippable Run $BUILD_NUMBER ($BUILD_URL) succeeded for $COMPARE_URL
        - FOO=BAR
        - FIZZ=BUZZ
      branches:
        only:
          - master
          - dev
      on_success: always
      on_failure: never
      on_pull_request: never
      on_start: never
```
* `integrationName` value is the name of the account integration you added to 'Subscription' settings.
* `type` is webhook
* [optional] `payload` You can specify `key=value` pairs where the `value` is a string. The string can contain Shippable [Environment Variables](ci_configure/#using-environment-variables). These variables will be populated by corresponding values from a run for this project.
* If the `webhook` integration is set up **to trigger an enabled project**: the `payload` will be injected into the next run as a set of global environment variables ([Injecting Global Env Variables](api/#trigger-a-new-run)).
* If the `webhook` integration is set up **to trigger an external webhook**: the `payload` will be converted to a JSON with `key: value` attributes which will be sent as the body when the specified webhook URL is triggered.
* [optional] `branches` allows you to choose the branches you want to trigger the configured webhook for. By default, the configured webhook is triggered for all branches. The `only` tag should be used when you want to trigger webhooks for specific branches. You can also use the `except` tag to exclude specific branches.
* [optional] You can set the following options for the `on_success`, `on_failure` tags :
    - `change` for `on_success` or `on_failure` means you will receive notifications only when the build status changes to success or failure respectively
    - `always` means that you will always receive a notification for that build status
    - `never` means that you will never receive a notification for that build status
    - **By default, `on_success` is set to `always` and `on_failure` is set to `never`** if the integration is configured in the yml but you do not specify these tags.
* [optional] You can set the following options for the `on_start`, `on_pull_request` tags :
    - `always` means that you will always receive a notification for build start/pull request
    - `never` means that you will never receive a notification for that build start/pull request
    - **By default, `on_start` is set to `never` and `on_pull_request` is also set to `never`** if `webhook` is configured in the yml but you do not specify these tags.

Check out our blogs  on practial examples of [Triggering a custom webhook after CI](http://blog.shippable.com/triggering-a-custom-webhook-after-continuous-integration) and [Triggering a parameterized build after CI](http://blog.shippable.com/triggering-a-parameterized-build-after-continuous-integration)to learn more about Event Triggers.

* * * 

## Configuring deployments to PaaS/IaaS
Having configured CI for your builds, here are the steps to deploy your code to various PaaS/IaaS providers.

### AWS Elastic Beanstalk
Our updated platform enables you to deploy your code to AWS Elastic Beanstalk (EB)
in two ways - Source Code Deployment and Docker Deployment. Let's look at both and the way Shippable helps you in having a successful deployment to EB.

####**Source code deployment to AWS Elastic Beanstalk**

Shippable performs the following steps for you, to deploy your source code on EB:

- Installs EB Command Line Interface (CLI)
- Authenticates into EB console using the right credentials
- Issues the Deploy command on EB from the right directory

To enable Shippable to perform these steps, you will need to configure the following two steps for a successful deployment to EB.

1. **Add Amazon AWS integration to the project**: 
     - Go to the 'Settings' tab of your subscription.
     - Under the 'Integrations' section, click the 'Add Integration' button.
     - In the dropdown of the 'Account Integrations' field, select 'AWS'. 
     - If you have created the AWS integration before, it'll show up as an option in the dropdown. Select AWS. This enables Shippable to authenticate into AWS. Given below is a screen shot of the Subscription Settings page, where 'AWS' is being added to the subscription 'Shippable-Demo'. 
     - If you don't see an AWS option in the dropdown, instructions on setting up Amazon EB account integration in Shippable can be found [here](int_paas_iaas_providers.md).
     <img src="../images/subscription_settings_aws_integration.png" alt="Account Settings Subscription" style="width:700px;"/>

2. **Configure shippable.yml**: Update the `shippable.yml` with the new collection for deployments. A sample form for doing a source code deployment to Elastic Beanstalk is shown:

```yaml
integrations:
  deploy:
    - integrationName: "aws"
      type: aws
      target: eb_paas
      platform: "Node.js"
      application_name: sample-node-eb-paas-app
      env_name: sample-node-eb-paas-env
      region: us-east-1
```
Replace in the above form, with your values as follows:

- **integrationName**: replace 'aws' with your Account Integration in Shippable, set up for AWS (keep the double quotes)
- **platform**: replace 'Node.js' with your platform (available options can be found [here](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/tutorials.html))
- **application_name**: replace with your Elastic Beanstalk application name
- **env_name**: replace with your Elastic Beanstalk environment name
- **region**: replace with your EB region name

With this configured, upon a successful CI run you will see the above steps executed within your CI run console and you should be able to verify within EB that a deployment was triggered.

For reference, here is a [sample Node.js application](https://github.com/shippableSamples/sample_node_eb_paas) that successfully performs source code deployment to Elastic Beanstalk. In addition, the blog ["How to deploy your application to AWS Elastic Beanstak"](http://blog.shippable.com/how-to-deploy-your-application-to-aws-elastic-beanstalk-using-shippable-part-1) walks you through an end to end workflow - from setting up your repo to deploying your source code on AWS Elastic Beanstalk.

####**Docker deployment to AWS Elastic Beanstalk**

Shippable performs the following steps for you to deploy Docker on EB, after all the steps in `shippable.yml` that run inside the container are complete:

- Logs into EB
- Updates your Dockerrun.aws.json file with the IMAGE_NAME & TAG
- Uploads the artifacts to S3
- Updates the application version
- Issues the command to update the EB environment

Upon completion of the above, EB updates your environment based on the uploaded application version, independent of Shippable.

To enable Shippable perform the above steps, you will need to configure the following three steps for a successful deployment to EB.

1. **Add Amazon AWS integration to the project**: 
     - Go to the 'Settings' tab of your subscription.
     - Under the 'Integrations' section, click the 'Add Integration' button.
     - In the dropdown of the 'Account Integrations' field, select 'AWS'. 
     - If you have created the AWS integration before, it'll show up as an option in the dropdown. Select AWS. This enables Shippable to authenticate into AWS. Given below is a screen shot of the Subscription Settings page, where 'AWS' is being added to the subscription 'Shippable-Demo'. 
     - If you don't see an AWS option in the dropdown, instructions on setting up Amazon EB account integration in Shippable can be found [here](int_paas_iaas_providers.md).
     
2. **Ensure you have a dockerrun.aws.json file** in the root of your source code directory. The relevant section to Shippable is given below and must follow this format:
```yaml
  "Image": {
    "Name": "<IMAGE_NAME>:<TAG>",
    "Update": "true"
  }  
```

3. **Configure shippable.yml**: Update the `shippable.yml` with the deploy integration collection. A sample form for doing a Docker deployment to Elastic Beanstalk is shown:

```yaml
integrations:
  deploy:
    - integrationName: "aws"
      type: aws
      target: eb_docker
      application_name: "sample-node-eb-docker-app"
      env_name: "docker-env"
      bucket_name: "elasticbeanstalk-us-east-1-480971114143"
      region: "us-east-1"
      image_name: "harryi3t/sample_node_eb_docker"
      image_tag: "$BRANCH.$BUILD_NUMBER"
```
Replace in the above form, with your values as follows:

- **integrationName**: replace 'aws-test' with your Account Integration in Shippable, set up for AWS (keep the double quotes)
- **application_name**: replace with your EB application name
- **env_name**: replace with your EB environment name
- **bucket_name**: replace with your EB bucket name
- **region**: replace with your EB region name
- **image_name**: replace with your Docker image name
- **image_tag**: replace with your Docker image tag

With this configured, upon a successful CI run you will see the above steps executed within your CI run console and you should be able to verify within EB that a deployment was triggered.

For reference, here is a [sample Node.js application](https://github.com/shippableSamples/sample_node_eb_docker) that successfully performs Docker deployment to Elastic Beanstalk


**NOTE**:
While the above scenario includes pulling a public Docker image, you may have other scenarios such as wanting to pull an image from a private registry in your workflow. This is totally possible. Authenticating and pulling private third party images occurs outside of Shippable actions. You will have to be configure these settings within Elastic Beanstalk as Shippable is not responsible for these actions.

Click for instructions [to pull an image from a private repository hosted by an online registry](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker.html) and/or for [multicontainer docker environments](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker_ecs.html)

* * * 


## Services

Shippable offers a host of pre-installed services to make it easy to run your builds. In addition to these you can install other services also by using the `install` tag of `shippable.yml`.

All the services are turned off by default and can be turned on by using the `services:` tag.

**Please note that the `services` tag only works if you are using the default image for your builds, or if you're pulling an official image from our [drydock repository on Docker Hub](https://hub.docker.com/u/drydock/).**

### MongoDB

```yaml
# Mongo binds to 127.0.0.1 by default
services:
 - mongodb
```

Sample PHP code using
[mongodb](https://github.com/shippableSamples/sample_php_mongo) .

### MySQL

```yaml
# MySQL binds to 127.0.0.1 by default.
# Create a user and DB as part of the ci section before you use it

services:
  - mysql
build:
  ci:
    - mysql -e "GRANT ALL ON *.* TO shippable@localhost IDENTIFIED BY ''; FLUSH PRIVILEGES;"
    - mysql -e 'create database myapp_test;'
```

Sample javascript code using
[mysql](https://github.com/shippableSamples/sample_node_mysql).


###Postgres

```yaml
# Postgres binds to 127.0.0.1 by default.
# Create a user and DB as part of the ci section before using it

services:
  - postgres

build:
  ci:
    - psql -c 'create role shippable with superuser;' -U postgres
    - psql -c 'create database myapp_test;' -U postgres
```

### SQLite3

SQLite is a software library that implements a self-contained,
serverless, zero-configuration, transactional SQL database engine. So
you can use SQLite, if you do not want to test your code behaviour with
other databases.

Sample python code using
[SQLite](https://github.com/shippableSamples/sample_python_sqllite).

### Elastic Search

```yaml
# elastic search is on default port 9200
services:
  - elasticsearch
```

Sample python code using [Elastic Search](https://github.com/shippableSamples/sample_python_elasticsearch).

### Memcached

```yaml
# memcached runs on default port 11211
services:
  - memcached
```

Sample python code using
[Memcached](https://github.com/shippableSamples/sample_python_memcache) .

### Redis

```yaml
# redis runs on default port 6379
services:
  - redis
```

Sample python code using
[Redis](https://github.com/shippableSamples/sample_python_redis).

### Neo4j

```yaml
#neo4j runs on default port 7474
services:
  - neo4j
```

Sample javascript code using
[Neo4j](https://github.com/shippableSamples/sample_node_neo4j) .

### CouchDB

```yaml
# couchdb binds to the default localhost 127.0.0.1 and runs on default port 5984. It is not started on boot.
services:
  - couchdb
```

Sample ruby code using
[CouchDB](https://github.com/shippableSamples/sample-ruby-couchdb) .

### RethinkDB

```yaml
# rethinkdb binds to the default localhost 127.0.0.1 and is not started on boot.
services:
  - rethinkdb
```

Sample javascript code using
[RethinkDB](https://github.com/shippableSamples/sample-node-rethinkdb).

### RabbitMQ

```yaml
# rabbitmq binds to 127.0.0.1 and is not started on boot. Default vhost "/", username "guest" and password "guest" can be used.
services:
  - rabbitmq
```

Sample python code using
[RabbitMQ](https://github.com/shippableSamples/sample_python_rabbitmq) .


### Custom startup commands for services

You can give custom commands to startup services by defining SHIPPABLE_*_CMD environment variable in your yml.
Replace * by the name of service.

For example, defining SHIPPABLE_POSTGRES_CMD in yml will override the default startup command for postgres:
```
env:
  global:
    - SHIPPABLE_POSTGRES_VERSION="9.4"
    - SHIPPABLE_POSTGRES_BINARY="/usr/lib/postgresql/$SHIPPABLE_POSTGRES_VERSION/bin/postgres"
    - SHIPPABLE_POSTGRES_CMD="sudo -u postgres $SHIPPABLE_POSTGRES_BINARY -c \"config_file=/etc/postgresql/$SHIPPABLE_POSTGRES_VERSION/main/postgresql.conf\" -c \"fsync=off\" -c \"synchronous_commit=off\""

```

 List of names of environment variables for corresponding services:

| Service        | Env variable           |
| ------------- |-------------|
| MongoDB | SHIPPABLE_MONGODB_CMD|
| MySQL | SHIPPABLE_MYSQL_CMD |
| Postgres | SHIPPABLE_POSTGRES_CMD |
| Elastic Search | SHIPPABLE_ES_CMD |
| Memcached | SHIPPABLE_MEMCACHED_CMD |
| Redis | SHIPPABLE_REDIS_CMD |
| Neo4j | SHIPPABLE_NEO4J_CMD |
| CouchDB | SHIPPABLE_COUCHDB_CMD |
| RethinkDB | SHIPPABLE_RETHINKDB_CMD |
| RabbitMQ | SHIPPABLE_RABBITMQ_CMD |
| Selenium | SHIPPABLE_SELENIUM_CMD |

* * * 


## Addons

### Firefox

We support different firefox versions like "18.0", "19.0", "20.0",
"21.0", "22.0", "23.0", "24.0", "25.0", "26.0", "27.0", "28.0", "29.0".
To select a specific firefox version, add the following to your
shippable.yml file.

```yaml
addons:
  firefox: "21.0"
```

### Custom Host Name

You can also set up custom hostnames using the **hosts** addons. To set
up the hostnames in /etc/hosts file, add the following to your
shippable.yml file.

```yaml
addons:
  hosts:
    - google.com
    - asdf.com
```

### Selenium

Selenium is not started on boot. You will have to enable it using
**services** tag and start xvfb (X Virtual Framebuffer) on display port
99.0, so that all your test suites will run on the server without a
display. Configure your yml file as shown below to start selenium server
on firefox and run node tests.

```yaml
language: node_js
node_js:
  - "0.10"
addons:
  firefox: "23.0"

services:
  - selenium

build:
  ci:
    - sudo npm install
    - "export DISPLAY=:99.0"
    - xvfb-run --server-args="-ac" npm test
```

Selenium **2.40** is started by default. You can also select a different
version of selenium using **addons** tag. The following versions are
supported:

- 2.39
- 2.40
- 2.41
- 2.42
- 2.43
- 2.44

Choose the required version and add it to your shippable.yml file as
shown below

```yaml
addons:
  selenium: "2.43"
```

This will download the required version. You will have to include
**services** tag in your yml file to start the selenium server using the
downloaded version. Configure your yml file as shown below to start
selenium server **2.43** on firefox.

```yaml
language: node_js
node_js:
  - "0.10"
#specify required selenium and firefox version
addons:
  selenium: "2.43"
  firefox: "27.0"

build:
  ci:
    - sudo npm install
    - "export DISPLAY=:99.0"
    - xvfb-run --server-args="-ac" npm test
```

Sample javascript code using
[Selenium](https://github.com/shippableSamples/sample_node_selenium) .

* * * 


## Pull requests

### GitHub
Shippable integrates with github to build your pull requests and show status inline on your GitHub page for the PR.

Whenever a pull request is opened for a project that is enabled on Shippable, we will run a build for the respective pull request and send you a build status notification. You can also see this status on your GitHub page as shown below:

<img src="../images/ci_pr_status.png" alt="GitHub PR Status" style="width:600px;"/>

You can then merge the PR confidently if the build passes, or fix any issues that cause a failed build. Each time your pull request is updated, we will kick off a new build and update status.

After you accept the pull request, Shippable will run one more build for the merged repo and will send email notifications for the merged repo.  

### Bitbucket
Shippable also integrates with Bitbucket to build pull requests.

<img src="../images/bitbucket_pr_status.png" alt="Bitbucket PR Status" style="width:700px;"/>

The pull request build can be enabled or disabled by editing the webhook in Bitbucket.
To enable it, go to Bitbucket Project Settings > Webhooks > Edit > Choose from a full list of triggers and check Pull Request Created and Updated. To disable, uncheck the two options.

<img src="../images/ci_bitbucket_webhook_triggers.png" alt="Bitbucket Webhook Triggers" style="width:350px;">

A few things to note here:
- The YML is always picked from the destination(base) branch.
- If the pull request comes from a private fork of the project and the subscription key is not added as a deploy key for the fork, the pull request build will fail at the `git_sync` CI step. This is due to the way Bitbucket handles permissions on private forks. To fix this - first, copy the subscription deploy key from Shippable Subscription > Settings > Deployment Keys. Next, add it as a deploy key for the private fork: Bitbucket Project Settings > Deploy Key > Add.


* * * * * 

## Build timeout

Your builds will time out in the following scenarios:

-   If there has not been any log output or a command hangs for 10 minutes
-   If the build is still running for more than 60 minutes (for free accounts) or for 120 minutes (for paid accounts). This is a default setting that you can change for your project and configure it between 1 - 60 minutes/120 minutes for free/paid accounts respectively. Read [instructions](ci_projects.md#custom-timeout) on setting a desired value in the Project settings. In addition, we also have a blog on [Changing the default timeout for a CI project](http://blog.shippable.com/changing-the-default-timeout-for-a-continuous-integration-project).

Please [let us know](https://github.com/shippable/support/issues) if you believe a build is timing out when it shouldn't do so and we will take a look.


* * * * *

## Skipping a build

Any changes to your source code will trigger a build automatically on
Shippable. So if you do not want to run build for a particular commit,
then add **[ci skip]** or **[skip ci]** to your commit message.

Our webhook processor will look for the string **[ci skip]** or **[skip
ci]** in the commit message and if it exists, then that particular
webhook build will not be executed.

* * * 

<a name="unsupported_languages"></a>
## Building unsupported languages
We support many popular languages with official build images for each one as listed on the [What is supported?](http://docs.shippable.com/gs_supported/) page.

However, you can run builds for any language you want using Shippable. You will need to provide a custom image for your build as explained in the [Overriding default build image](http://docs.shippable.com/ci_configure/#overriding-the-default-build-image) section and use the `language: none` tag in your shippable.yml. Setting the language to none means that we skip any default processing for each section in your yml. You will need to configure everything you need in the yml and your builds should work as expected.

* * * 

## Service Status

You can get the current status of the Shippable UI and API by going to our [Service Status page](http://status.shippable.com). Our status page also shows the status of some of our third party integrations like Amazon EC2, Quay.io, Digital Ocean, as well the response time of our API.

The service status page can be viewed from our product UI by going to the heart ekg icon in your top navbar.

For additional details, check our blog on [Shippable status page](http://blog.shippable.com/features_amazon_ecr_regions_status_page_github_tags_and_more#status_page).

---
