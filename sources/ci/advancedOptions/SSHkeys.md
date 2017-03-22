# SSH keys

SSH keys are used to access your projects on the source control. Shippable provides two SSH keys. 

* A subscription SSH key is created when a subscription is created on shippable and the public key for it is the one on the subscription settings tab. By default this key does not have access to any projects. Adding the deploy key to your account on source control gives access to all CI jobs in the subscription.

* Project SSH key is created when a project is enabled on shippable. Any private project will have a deploy key added to the repository. Public projects will not have this key added to the repository. This key has access to only a particular project and cannot be used to pull or push to/from other repositories. 

Every build container has these two ssh keys. The subscription SSH key is available at /tmp/ssh/00_sub  and the project key at /tmp/ssh/01_deploy. By default the subscription SSH key(/tmp/ssh/00_sub) will be used when cloning a project. The project SSH key will only be used if the subscription SSH key has no access.

``` 
We recommend you use the subscription SSH key to pull other repositories (for example, private submodules) and Project SSH key to pull or push to the same repository
```

You can also use key integrations to access your projects on source control. For a more detailed explanation, check out our SSH key integration page. 
