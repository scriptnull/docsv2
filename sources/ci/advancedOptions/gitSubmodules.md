page_title: Using git submodules in your Continuous Integration/Continuous Delivery projects
page_description: How to to use git submodules in your Build Configuration
page_keywords: getting started, questions, documentation, shippable, config, yml, GitHub, Bitbucket, GitLab, Docker


# Using git submodules

If you're building a big project, you might want to break it down into smaller chunks and use git submodules to make everything work.

If you have a `.gitmodules` file at the root of the repo, it is automatically intialized as part of your build by default.


---

###Public submodules
If your submodules are in a public repository, use the public Git URL in your .gitmodules file:

```
https://github.com/someuser/somelibrary.git
```

---

###Private submodules
If your submodules are in a private repository, you will need to add your Shippable subscription's deploy key to the private repository on your source control, so that we have access to pull from the repo.

To do this, go to your Shippable's subscription settings and copy the deploy key (key in image is redacted for privacy):

<img src="../../images/advancedOptions/deployKey.png" alt="deploy key for git submodules" style="width:1000px;"/>

Then, go to the private repository that contains the required submodule and add a deploy key in your repo settings (key in image is redacted for privacy):

<img src="../../images/advancedOptions/deployKeyGitHub.png" alt="deploy key for git submodules" style="width:1000px;"/>

You can now include the path of your submodule repo in the `.gitmodules` file of the repository you're building:

```
git@github.com:someuser/somelibrary.git
```

---

###Turning off submodules

To turn off submodules for your build, include the following in your yml:
```
git:
   submodules: false
```




---
