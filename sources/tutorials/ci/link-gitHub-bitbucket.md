page_title: Shippable Account Settings
page_description: Account Settings, Integratons, Account Tokens, Credit Cards
page_keywords: containers, lxc, docker, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, Tokens, account settings

# Link your Bitbucket and GitHub Accounts

You can use Shippable to build both GitHub and Bitbucket repositories, by connecting both the accounts to your Shippable account. This will help you to get a consolidated view of all your projects from the source code systems in one Shippable account.

To connect your GitHub/BitBucket accounts:

1. Log in to [Shippable](<http://www.shippable.com>) with either GitHub or BitBucket account. Choose the one that you want as your primary account.

2. Click on the Account Settings icon on the upper, right navigation bar.
![account_settings](/tutorials/images/ci/account-settings.gif)
3. If you signed in with GitHub, you will see an option to enable Bitbucket under the **Git Identities Section**. Click on **Enable** and follow the authorization flow for Bitbucket.
4. If you signed in with Bitbucket, you will see an option to enable GitHub under the **Git Identities Section**. Click on **Enable** and follow the authorization flow for GitHub.

Once both your accounts are connected to Shippable, you should see a
consolidated list of orgs and projects in your account. You can sign in to Shippable
with either of your GitHub/BitBucket credentials going forward.

> **Tip**
>
> To see the linked account repositories updated immediately, click on the **Sync** button on your Account Settings section
>
> <img src="/tutorials/images/ci/account_settings_sync.png" alt="Aye0Aye Successful Build" style="width:700px;"/>


**Note**
If you have already logged into Shippable using your GitHub account **AND** BitBucket account separately, you'll be unable to link these accounts within Shippable. To enable the linking, open a support ticket with Shippable to delete one of the accounts within Shippable. Upon deletion, log in to Shippable with the other, existing account & follow the above steps to link your GitHub & BitBucket accounts.

You can also check our blog on [how to link github and bitbucket accounts](http://blog.shippable.com/how-to-link-github-and-bitbucket-accounts).
