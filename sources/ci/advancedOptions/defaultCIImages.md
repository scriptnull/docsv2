page_title: Default images used for Continuous Integration (CI)
page_description: An explanation of how we select default images for your Continuous Integration projects
page_keywords: ci/cd dashboard, subscription settings, CI/CD, shippable CI/CD, documentation, shippable, config, yml, AMI, Docker, version, 1.11, 1.12

# Default images used for CI

All builds on Shippable run inside a Docker container which comes pre-installed with most packages and dependencies you will need for your build. We have a set of standard Docker images that are used to spin up these CI containers.

**This should not be confused with the virtual machine which hosts your CI container.** The CI container is spun up on the build VM using either the default image depending on your yml configuration or a custom image if specified in the yml.

All official CI images created by the Shippable team are publicly available in [our drydock repository on Docker Hub](https://hub.docker.com/u/drydock/). CI images are named with the following convention:

* Image name is structured with the following logic:
    * first letter representing the OS (i.e. u for Ubuntu)
    * second and third letter representing OS version (16 for Ubuntu 16.04 and 14 for Ubuntu 14.04)
    * next 3 letters indicate language (nod for Node.js, jav for Java, clo for Clojure, etc)
    * last 3 letters indicate if services are installed (`all` indicates that all supported services are installed)

* Image tag determines what language versions, packages, and services are pre-installed on the image

For example, u16javall indicates an Ubuntu 16.04 image for Java with all supported services pre-installed.

**The default CI image is tightly coupled with the [Machine Image set for your subscription](machineImages/). Your subscription is configured to use the latest machine image available at the time you run your first build on Shippable.**

| Date when the first build ran for your Subscription | Default Machine Image| Default image tag |
|-----------------------------------------------------|-------------------|-------------------|
| After March 11, 2017                                | v5.3.2          | [v5.3.2](#532)             |
| On or before March 10, 2017                         | Stable             | [prod](#prod)               |

<br>
You can choose to override this default by [changing your Subscription's machine image](machineImages.md#changing-the-machine-image-for-a-subscription), Please note that changing Machine image will also change your default CI images.

The following sections provide more information about what is installed on standard images, depending on image_name and tag.

<a name="532"></a>
### Image tag: v5.3.2

This section describes what is installed on images with the tag `v5.3.2`.

<a name="common-532"></a>
#### Common components

All images with tag `v.5.3.2` will have the components and services listed below.

In addition, each image, depending on language, also has language versions installed. Please check the next section for the image names and language versions included.

Pre-installed components:

* Basic packages: build-essential, curl, gcc, gettext, git, htop, jq, libxml2-dev, libxslt-dev, make, nano, openssh-client, openssl, python-dev, python-pip, python-software-properties, software-properties-common, software-properties-common, sudo, texinfo, unzip, virtualenv, wget
* awscli 1.11.44
* awsebcli 3.9
* gcloud 145.0.0
* jfrog-cli 1.7.0
* kubectl 1.5.1
* packer 0.12.2
* terraform 0.8.7

Pre-installed services
* couchdb 1.6
* elasticsearch 5.1.2
* neo4j 3.1.1
* memcached 1.4.34
* mongodb 3.4
* mysql 5.7
* postgres 9.6
* rabbitmq 3.6
* redis 3.2
* rethinkdb 2.3
* riak 2.2.0
* selenium 3.0.1
* sqllite 3

#### Node.js

We have 2 primary build images for Node projects, which should be sufficient for most projects:

* [dry-dock/u16nodall:v.5.3.2](https://github.com/dry-dock/u16nodall): Ubuntu 16.04 image with Node
* [dry-dock/u14nodall:v.5.3.2](https://github.com/dry-dock/u14nodall): Ubuntu 14.04 image with Node

Both images contain the following:

* Node versions:
    * 0.10
    * 0.12 (default if no runtime specified)
    * 4.2.3
    * 4.6.0
    * 5.12.0
    * 6.7.0
    * 6.8.0
    * 6.9.4
    * 7.0.0
    * 7.2.1
    * 7.3.0
    * 7.4.0
    * iojs 1.0
    * iojs 2.0
    * iojs 3.3.1

* Additional packages:
    * nvm
    * Java 1.8
    * Ruby 2.3.3

Please note that in addition, both images also contain everything listed in [Common components](#common-532)

#### Java

We have 2 primary build images for Java projects, which should be sufficient for most projects:

* [dry-dock/u16javall:v.5.3.2](https://github.com/dry-dock/u16javall): Ubuntu 16.04 image with Java
* [dry-dock/u14javall:v.5.3.2](https://github.com/dry-dock/u14javall): Ubuntu 14.04 image with Java

Both images contain the following:

* Java versions
    * openjdk7
    * openjdk8
    * oraclejdk7
    * oraclejdk8

* Additional packages:
    * Node 7.x
  	* Ruby 2.3.3

Please note that in addition, both images also contain everything listed in [Common components](#common-532)

#### Python

We have 2 primary build images for Python projects, which should be sufficient for most projects:

* [dry-dock/u16pytall:v.5.3.2](https://github.com/dry-dock/u16pytall): Ubuntu 16.04 image with Python
* [dry-dock/u14pytall:v.5.3.2](https://github.com/dry-dock/u14pytall): Ubuntu 14.04 image with Python

Both images contain the following:

* Python versions
    * 2.6
    * 2.7 (default if no runtime specified)
    * 3.2
    * 3.3
    * 3.4
    * 3.5
    * 3.6
    * pypy 4.0.1
    * pypy3 2.4.0

* Additional packages:
    * virtualenv
    * Java 1.8
    * Node 7.x
    * Ruby 2.3.3

Please note that in addition, both images also contain everything listed in [Common components](#common-532)

#### Ruby

We have 2 primary build images for Ruby projects, which should be sufficient for most projects:

* [dry-dock/u16ruball:v.5.3.2](https://github.com/dry-dock/u16ruball): Ubuntu 16.04 image with Ruby
* [dry-dock/u14ruball:v.5.3.2](https://github.com/dry-dock/u14ruball): Ubuntu 14.04 image with Ruby

Both images contain the following:

* Ruby versions
    * 1.8.7
    * 1.9.3
    * 2.0.0
    * 2.1.5
    * 2.2.1
    * 2.2.5
    * 2.3.0
    * 2.3.1
    * 2.3.2
    * 2.3.3
    * jruby 1.7.19
    * jruby 9.0.0
    * jruby 9.1.2
    * jruby 9.1.5
    * ree 1.8.7

* Additional packages:
    * rvm
    * Java 1.8
  	* Node 7.x

Please note that in addition, both images also contain everything listed in [Common components](#common-532)

#### Go

We have 2 primary build images for Go projects, which should be sufficient for most projects:

* [dry-dock/u16golall:v.5.3.2](https://github.com/dry-dock/u16golall): Ubuntu 16.04 image with Go
* [dry-dock/u14golall:v.5.3.2](https://github.com/dry-dock/u14golall): Ubuntu 14.04 image with Go

Both images contain the following:

* Go versions
    * 1.1
    * 1.2
    * 1.3
    * 1.4
    * 1.5
    * 1.5.4
    * 1.6
    * 1.6.4
    * 1.7
    * 1.7.5

* Additional packages:
    * gvm
    * Java 1.8
    * Node 7.x
    * Ruby 2.3.3

Please note that in addition, both images also contain everything listed in [Common components](#common-532)

#### PHP

We have 2 primary build images for PHP projects, which should be sufficient for most projects:

* [dry-dock/u16phpall:v.5.3.2](https://github.com/dry-dock/u16phpall): Ubuntu 16.04 image with PHP
* [dry-dock/u14phpall:v.5.3.2](https://github.com/dry-dock/u14phpall): Ubuntu 14.04 image with PHP

Both images contain the following:

* PHP versions
    * 5.6
    * 7.0
    * 7.1

* Additional packages:
    * phpenv
    * Java 1.8
  	* Node 7.x
  	* Ruby 2.3.3

Please note that in addition, both images also contain everything listed in [Common components](#common-532)

#### Clojure

We have 2 primary build images for Clojure projects, which should be sufficient for most projects:

* [dry-dock/u16cloall:v.5.3.2](https://github.com/dry-dock/u16cloall): Ubuntu 16.04 image with Clojure
* [dry-dock/u14cloall:v.5.3.2](https://github.com/dry-dock/u14cloall): Ubuntu 14.04 image with Clojure

Both images contain the following:

* Clojure versions
    * 1.3.0
    * 1.4.0
    * 1.5.1
    * 1.6.0
    * 1.7.0
    * 1.8.0

* Additional packages:
  	* leiningen
    * Java 1.8
  	* Node 7.x
  	* Ruby 2.3.3

Please note that in addition, both images also contain everything listed in [Common components](#common-532)

#### Scala

  We have 2 primary build images for Scala projects, which should be sufficient for most projects:

  * [dry-dock/u16scaall:v.5.3.2](https://github.com/dry-dock/u16scaall): Ubuntu 16.04 image with Scala
  * [dry-dock/u14scaall:v.5.3.2](https://github.com/dry-dock/u14scaall): Ubuntu 14.04 image with Scala

Both images contain the following:

* Scala versions
    * 2.9.3
    * 2.10.6
    * 2.11.8
    * 2.12.0
    * 2.12.1

* Additional packages:
    * sbt
    * Java 1.8
  	* Node 7.x
  	* Ruby 2.3.3

Please note that in addition, both images also contain everything listed in [Common components](#common-532)

#### C/C++

We have 2 primary build images for C/C++ projects, which should be sufficient for most projects:

* [dry-dock/u16cppall:v.5.3.2](https://github.com/dry-dock/u16cppall): Ubuntu 16.04 image with C/C++
* [dry-dock/u14cppall:v.5.3.2](https://github.com/dry-dock/u14cppall): Ubuntu 14.04 image with C/C++

Both images contain the following:

* C/C++ cpmpiler versions
    * gcc 6
    * clang 3.9.0

* Additional packages:
    * Java 1.8
  	* Node 7.x
  	* Ruby 2.3.3

Please note that in addition, both images also contain everything listed in [Common components](#common-532)

<a name="prod"></a>
### Image tag: prod                                                                              

This section describes what is installed on images with the tag `prod`.

<a name="common-prod"></a>
#### Common components

All images with tag `prod` will have the components and services listed below.

In addition, each image, depending on language, also has language versions installed. Please check the next section for the image names and language versions included.

Pre-installed components:

* Git
* Basic packages sudo, build-essential, curl, gcc, make, openssl, software-properties-common, wget, nano, unzip, libxslt-dev, libxml2-dev
* Python packages python-pip, python-software-properties, python-dev
* awscli
* google-cloud-sdk

Pre-installed services:
* couchdb 1.6
* elasticsearch 1.5
* neo4j 2.2
* memcached 1.4
* mongodb 3.0
* mysql 5.6
* postgres 9.4
* rabbitmq 3.5
* redis 3.0
* rethinkdb 2.0
* riak
* selenium 2.52
* sqllite 3

#### Node.js

We have the following default build images for Node, which should be sufficient for most projects:

* [dry-dock/u14nodall:prod](https://github.com/dry-dock/u14nodall): Ubuntu 14.04 image with Node

Both images contain the following:

* Node versions:
    * 0.10
    * 0.12 (default if no runtime specified)
    * 4.2.3
    * iojs 1.0
    * iojs 2.0

* Additional packages:
    * Node.js packages grunt-cli, mocha, vows, phantomjs, casperjs
    * Selenium 2.48.2
    * Bower
    * Default Java versions: default-jre, default-jdk, openjdk-6, oracle jdk 7
    * Default Ruby version


Please note that in addition, the image also contain everything listed in [Common components](#common-prod)

#### Java

We have the following default build images for Java, which should be sufficient for most projects:

* [dry-dock/u14javall:prod](https://github.com/dry-dock/u14javall): Ubuntu 14.04 image with Java

Both images contain the following:

* Java versions
    * openjdk7
    * openjdk8
    * oraclejdk7
    * oraclejdk8

* Additional packages:
    * Gradle 2.3
    * Apache maven 3.2.5
    * Apache ant 1.9.6
    * Node version 0.10
    * Python 2.7.6
    * Default Ruby version

Please note that in addition, the image also contain everything listed in [Common components](#common-prod)

#### Python

We have the following default build image for Python, which should be sufficient for most projects:

* [dry-dock/u14pytall:prod](https://github.com/dry-dock/u14pytall): Ubuntu 14.04 image with Python

Both images contain the following:

* Python versions
    * 2.6
    * 2.7 (default if no runtime specified)
    * 3.2
    * 3.3
    * 3.4
    * 3.5
    * pypy
    * pypy3 3

* Additional packages:
    * virtualenv 13.1.2
    * Pip 7.1.2
    * Python pre-reqs libxml2, libxml2-dev, libxslt1.1, libxslt1-dev, libffi-dev, libssl-dev
    * Default Java versions: default-jre, default-jdk, openjdk-6, oracle jdk 7
    * Node 0.10
    * Default Ruby version

Please note that in addition, the image also contain everything listed in [Common components](#common-prod)

#### Ruby

We have the following default build image for Ruby, which should be sufficient for most projects:

* [dry-dock/u14ruball:prod](https://github.com/dry-dock/u14ruball): Ubuntu 14.04 image with Ruby

Both images contain the following:

* Ruby versions
    * 1.8.7
    * 1.9.2
    * 1.9.3
    * 2.0
    * 2.1.x
    * 2.2.x
    * jruby 18-mode
    * jruby-19mode
    * jruby-head
    * rbx
    * ruby-head
    * ree

* Additional packages:
    * bundler for each Ruby version
    * rvm
    * Default Java versions: default-jre, default-jdk, openjdk-6, oracle jdk 7
    * Python 2.7.6
  	* Node 0.10

Please note that in addition, the image also contain everything listed in [Common components](#common-prod)

#### Go

We have the following default build images for Go, which should be sufficient for most projects:

* [dry-dock/u14golall:prod](https://github.com/dry-dock/u14golall): Ubuntu 14.04 image with Go

Both images contain the following:

* Go versions
    * 1.1 (default if tag not included in yml)
    * 1.2
    * 1.3
    * 1.4
    * 1.5
    * tip

* Additional packages:
    * Packages autotools-dev, autoconf, bison, git, mercurial, cmake, scons, binutils
    * gvm
    * Default Java versions: default-jre, default-jdk, openjdk-6, oracle jdk 7
    * Node 0.10
    * Python 2.7.6
    * Default Ruby

Please note that in addition, the image also contain everything listed in [Common components](#common-prod)

#### PHP

We have the following default build image for PHP, which should be sufficient for most projects:

* [dry-dock/u14phpall:prod](https://github.com/dry-dock/u14phpall): Ubuntu 14.04 image with PHP

Both images contain the following:

* PHP versions
    * 5.4
    * 5.5
    * 5.6
    * 7 (without extensions)

* Additional packages:
    * Extensions memcache, memcached, mongo, amqp-1.6.8, zmq-beta, redis
    * phpUnit
    * composer
    * phpenv
    * pickle
    * librabbitmq
    * Packages wget cmake, libmcrypt-dev, libreadline-dev, libzmq-dev, wget, cmake, libmcrypt-dev, libreadline-dev, libzmq-dev, php5-dev
    * Default Java versions: default-jre, default-jdk, openjdk-6, oracle jdk 7
  	* Node 0.10
  	* Default Ruby version
    * Python 2.7.6

Please note that in addition, the image also contain everything listed in [Common components](#common-prod)

#### Clojure

We have the following default build image for Clojure, which should be sufficient for most projects:

* [dry-dock/u14cloall:prod](https://github.com/dry-dock/u14cloall): Ubuntu 14.04 image with Clojure

Both images contain the following:

* Clojure versions
    * 1.3.0
    * 1.4.0
    * 1.5.1
    * 1.6.0

* Additional packages:
  	* leiningen
    * Default Java versions: default-jre, default-jdk, openjdk-6, oracle jdk 7
    * Node 0.10
  	* Default Ruby version
    * Python 2.7.6


Please note that in addition, the image also contain everything listed in [Common components](#common-prod)

#### Scala

  We have the following default build image for Scala, which should be sufficient for most projects:

  * [dry-dock/u14scaall:prod](https://github.com/dry-dock/u14scaall): Ubuntu 14.04 image with Scala

Both images contain the following:

* Scala versions
    * 2.9.x
    * 2.10.x
    * 2.11.x

* Additional packages:
    * sbt
    * Default Java versions: default-jre, default-jdk, openjdk-6, oracle jdk 7
    * Node 0.10
  	* Default Ruby version
    * Python 2.7.6

Please note that in addition, the image also contain everything listed in [Common components](#common-prod)

#### C/C++

We have the following default build image for C/C++, which should be sufficient for most projects:

* [dry-dock/u14cppall:prod](https://github.com/dry-dock/u14cppall): Ubuntu 14.04 image with C/C++

Both images contain the following:

* C/C++ cpmpiler versions
    * gcc v5.3.0
    * clang v3.8.0

* Additional packages:
    * Default Java versions: default-jre, default-jdk, openjdk-6, oracle jdk 7
    * Node 0.10
    * Default Ruby version
    * Python 2.7.6

Please note that in addition, the image also contain everything listed in [Common components](#common-prod)

---
