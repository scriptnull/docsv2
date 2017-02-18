
# C/C++
This page explains yml configuration that is specific to C/C++ projects. For a complete yml reference, please read the [Build configuration section](../shippableyml.md)

##yml configuration

The sections below explore sections of the yml that are specific to C/C++ projects. 


###language 


For C/C++ projects, this tag should always be set to c as show below:

```
language: c
```

### compiler

You can set the compiler you want to use using the `compiler` tag:

```
compiler:
  - gcc
```

You can also test against multiple versions of compilers by specifying them all in this section:

```
compiler:
  - gcc
  - clang
```

**Important note:** The compiler tag only works with official CI images provided by Shippable. If you are using a custom image for your build, you will need to switch the compiler in the `ci` section of your yml.

### pre_ci and pre_ci_boot

Depending on the `language` and `services` tags in your yml, an official build image is chosen for your build by default, and your build container is started with standard options. The default images for C/C++ builds are explained below.

The pre_ci and pre_ci_boot sections are primarily used in one of the following scenarios:

* You want to use a custom Docker image for your CI 
* You want to override the default options that are used to boot up the default CI image

If you do not want to do either of the above, you should skip these tags in the yml.

#### Default C/C++ image
We have a standard build image for C/C++ projects, which should be sufficient for most projects: 

* [dry-dock/u14cpp](https://github.com/dry-dock/u14cpp) is used if you specify `language: C` in your yml and do not specify a `services` tag. This image contains the following:
	
	* Ubuntu 14.04
	* gcc: v5.3.0
	* clang: v3.8.0
	* Git
	* Basic packages sudo, build-essential, curl, gcc, make, openssl, software-properties-common, wget, nano, unzip, libxslt-dev, libxml2-dev
	* Default Java versions: default-jre, default-jdk, openjdk-6, oracle jdk 7  
	* Python packages python-pip, python-software-properties, python-dev
	* Node version 0.10 
	* Default Ruby version
	* Python 2.7.6
	* awscli
	* google-cloud-sdk 


If this official images do not satisfy your requirements, you can do one of 2 things:

- Continue using official images and include commands to install any missing dependencies or packages in your yml
- Use a custom build image that contains exactly what you need for your CI
	
#### Using a custom build image
If you do decide to use a custom CI image, you will need to configure the `pre_ci_boot` section and optionally, the `pre_ci` section if you're also building the CI image as part of the workflow. Details on how to configure this are available in the [`pre_ci` and `pre_ci_boot` sections of the Build configuration page](../shippableyml.md#build). 

### ci
The `ci` section should contain all commands you need for your `ci` workflow. Commands in this section are executed sequentially. If any command fails, we exit this section with a non zero exit code.

#### Installing dependencies
You can install the required dependencies for your project:

```
build:
  ci:
    - ./install.sh
    - command2
```


#### Adding test commands 
After installing dependencies, you can include your test commands. For example:  

```
build:
  ci:
    - make test 
```


#### Test and code coverage
You can view your test and code coverage results in a consumable format and drill down further to find out which tests failed or which sections of your code were not covered by your tests.

Your tests results data needs to be in junit format and your code coverage results need to be in cobertura format in order to see these visualizations. Test and code coverage results need to be saved to shippable/testresults and shippable/codecoverage folders so that we can parse the reports.


#### Default commands

If the `ci` section is blank, we will run a default command. This has the same effect as the yml snippet below:

```
build:
  ci:
    - ./configure && make && make test
```

To avoid executing the default command, include a simple command in like `pwd` or `ls` in this section.







