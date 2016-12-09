page_title: JFrog Artifactory integration
page_description: Setting up Shippable account integrations for JFrog Artifactory
page_keywords: registry, repository, artifactory, jfrog, maven

# JFrog Artifactory integration

You will need JFrog Artifactory integration if you want to do the following -

- Upload files to JFrog Artifactory
- Download files from JFrog Artifactory
- Deploy files from JFrog Artifactory to a cluster of nodes

##Adding the Account Integration.

You will need to configure this integration to store credentials to artifactory.

* Click on the gear icon for Account Settings in your top navigation bar and then click on the 'Integrations' section.
* Click on the `Add Integration` button.
* For 'Integration type', choose `JFrog Artifactory` from the list of choices.
* For 'Integration Name' use a distinctive name that's easy to associate to the integration and recall. Example: `jfrog-integration`
* Enter your HTTP Endpoint(url), username and password. You can follow instructions in [JFrogCLI-Authentication](https://www.jfrog.com/confluence/display/RTF/JFrog+CLI#JFrogCLI-Authentication)
* Choose the subscriptions, in which the integration can be accessible.
* Click on `Save`

<img src="/ci/images/integrations/artifactRegistries/jfrogArtifactory/addInt.png" alt="Add JFrog Artifactory Integration" style="width:700px;"/>

##Using JFrog Integration in CI
JFrog Artifactory integration can be used in a project's CI by adding the following block in `shippable.yml`

```
integrations:
  hub:
    - integrationName: mycompany-jfrog
      type: artifactory
```

##Download a file from Artifactory
Configure your `shippable.yml` to associate the JFrog integration for your project as part of CI.

Download an artifact ( in this case a html page ) by using the jfrog command into `artifacts` directory.

```
build:
  pre_ci:
    - jfrog rt dl pages/data.json
```

##Upload a file to Artifactory
When the build is complete, the resulting artifact can be uploaded to artifactory.

If your build image, has JFrog installed then you can run this in any section of build of shippable.yml, Shippable will configure the cli automatically.

```
build:
  ci:
    - jfrog rt u pages/artifact.tar
```

A sample yml might look something like the below.

```
language: none

build:
  pre_ci_boot:
    image_name: drydock/u14
    image_tag: tip
    pull: false
    options: '--privileged=true --net=bridge -e FOO=true -e BOO=false'

  ci:
    - jfrog rt dl pages/data.json
    - sudo apt-get install nodejs
    - npm install
    - node index.js
    - jfrog rt u result.html pages/index.$BUILD_NUMBER.html

integrations:
  hub:
    - integrationName: mycompany-jfrog
      type: artifactory

```
Every time the build runs, the `ci` section will download an artifact from JFrog and use it in a build process. If build is success, a resulting artifact is pushed with a unique identifier (`index.$BUILD_NUMBER.html`) and uploaded to JFrog.

##Deleting the Integration

 To remove the JFrog Artifactory integration, you'll need to remove this integration from all dependencies configured to use it. To find all the dependencies:

 1. Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
 2. Select the JFrog Artifactory integration from the list of integrations. If you have many entries, use the Filters dropdown and select JFrog Artifactory. Alternatively, you can use the Integration Name field to provide the name of your JFrog integration.
 3. Your JFrog integration shows up in the list.
 4. Click on the `Delete` button.
 5. A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration. The list will include any Subscription dependent on this integration.
 6. If there are dependencies, individually access the Settings tab for each Subscription and delete the JFrog Artifactory integration.
 7. Once all dependencies of the JFrog Artifactory integration have been removed, Step 5 will show the message: `No dependency`.
 8. Click the Yes button to delete the JFrog Artifactory Integration.
