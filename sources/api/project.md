page_title: Shippable API for Projects
page_description: How to interface with Shippable's API
page_keywords: shippable, API, HTTP

# Projects

The Projects endpoint will provide you with information about your projects.

## Get a list of all projects

Will a return a list projects, and some info about the projects

```
GET /projects
```

###Query parameters

|Type      |    Name     |    Description    |     Required    |    Schema    |  Default    |
|----------|-------------|-------------------|----------------|--------------|-------------|
| QueryParameter| projectIds| Filter by one or more project ids.| no | string | |
| QueryParameter| subscriptionIds| Filter by subscription.| no | string | |
| QueryParameter| autoBuild| If set to true, returns all projects enabled on Shippable. If set to false, returns all projects not enabled on Shippable.| no | boolean | |
| QueryParameter| isPrivateRepository| If set to true, returns all private repositories. If set to false, returns all public repositories. | no | boolean | |
| QueryParameter| isFork| If set to true, returns all repositories that are forked from another repo. If set to false, will return repositories that are not forks.| no | boolean | |
| SortParameter| enabledDate| When combined with autoBuild=true, sort=enabledDate will sort all enabled repositories by when they were enabled on Shippable.| no | string | |


###Response
|HTTP Code      |    Description     |
|---------------|--------------------|
|200|success|

```
[
  {
    "branches": [
      "master"
    ],
    "sourceRepoOwner": {
      "login": "Shippable",
      "id": 5647221,
      "avatar_url": "https://avatars.githubusercontent.com/u/5647221?v=3",
      "gravatar_id": "",
      "url": "https://api.github.com/users/Shippable",
      "html_url": "https://github.com/Shippable",
      "followers_url": "https://api.github.com/users/Shippable/followers",
      "following_url": "https://api.github.com/users/Shippable/following{/other_user}",
      "gists_url": "https://api.github.com/users/Shippable/gists{/gist_id}",
      "starred_url": "https://api.github.com/users/Shippable/starred{/owner}{/repo}",
      "subscriptions_url": "https://api.github.com/users/Shippable/subscriptions",
      "organizations_url": "https://api.github.com/users/Shippable/orgs",
      "repos_url": "https://api.github.com/users/Shippable/repos",
      "events_url": "https://api.github.com/users/Shippable/events{/privacy}",
      "type": "Organization",
      "site_admin": false
    },
    "propertyBag": {
      "repositoryHttpsUrl": "https://github.com/Shippable/docsv2.git",
      "isPaused": false,
      "enablePullRequestBuild": true,
      "enableCommitBuild": true,
      "enableTagBuild": false,
      "enableReleaseBuild": false,
      "cacheTag": 0,
      "serialize": false,
      "serializeBranches": [],
      "lowCoverageLimit": 0,
      "unstableOnLowCoverage": false,
      "dashboardBranchSettings": {
        "runFilter": "commit",
        "branchFilter": [
          "master"
        ]
      }
    },
    "id": "55511f38edd7f2c052e8c637",
    "projectId": 4675042,
    "name": "docsv2",
    "fullName": "Shippable/docsv2",
    "subscriptionId": "540e75583479c5ea8f9e71e1",
    "ownerAccountId": "540e7735399939140041d885",
    "builderAccountId": "540e652c399939140040eb8a",
    "repositoryUrl": "https://api.github.com/repos/Shippable/docsv2",
    "repositorySshUrl": "git@github.com:Shippable/docsv2.git",
    "repositoryHtmlUrl": "https://github.com/Shippable/docsv2",
    "sourceDefaultBranch": "master",
    "isPrivateRepository": false,
    "isOrg": true,
    "isFork": false,
    "sourceId": "35451002",
    "sourcePushedAt": "2017-01-30T23:08:05.000Z",
    "sourceCreatedAt": "2015-05-11T21:26:37.000Z",
    "sourceUpdatedAt": "2016-10-25T04:25:37.000Z",
    "lastBuildGroupNumber": 1275,
    "autoBuild": true,
    "deployPublicKey": "ssh-rsa abc123 Shippable\n",
    "language": "CSS",
    "providerId": "562dbd9710c5980d003b0451",
    "providerLastSyncEndDate": "2017-01-30T23:17:40.740Z",
    "providerLastSyncStartDate": "2017-01-30T23:17:40.391Z",
    "enabledDate": "2015-05-13T00:47:37.105Z",
    "enabledBy": "540e7735399939140041d88b",
    "enabledByUserName": "avinci",
    "type": "ci",
    "consolidateReports": false,
    "createdBy": "540e55445e5bad6f98764522",
    "updatedBy": "540e55445e5bad6f98764522",
    "createdAt": "2016-07-23T10:48:21.580Z",
    "updatedAt": "2017-01-30T23:17:40.740Z"
  }
]
```
---

## Get a specific project

This route returns in-depth information about the specified project.

```
GET /projects/:projectId
```

###Response

|HTTP Code      |    Description     |
|---------------|--------------------|
|200|success|
|500|Internal Server Error. Check job id.|

```
{
  "branches": [
    "master"
  ],
  "sourceRepoOwner": {
    "login": "Shippable",
    "id": 5647221,
    "avatar_url": "https://avatars.githubusercontent.com/u/5647221?v=3",
    "gravatar_id": "",
    "url": "https://api.github.com/users/Shippable",
    "html_url": "https://github.com/Shippable",
    "followers_url": "https://api.github.com/users/Shippable/followers",
    "following_url": "https://api.github.com/users/Shippable/following{/other_user}",
    "gists_url": "https://api.github.com/users/Shippable/gists{/gist_id}",
    "starred_url": "https://api.github.com/users/Shippable/starred{/owner}{/repo}",
    "subscriptions_url": "https://api.github.com/users/Shippable/subscriptions",
    "organizations_url": "https://api.github.com/users/Shippable/orgs",
    "repos_url": "https://api.github.com/users/Shippable/repos",
    "events_url": "https://api.github.com/users/Shippable/events{/privacy}",
    "type": "Organization",
    "site_admin": false
  },
  "propertyBag": {
    "repositoryHttpsUrl": "https://github.com/Shippable/docsv2.git",
    "isPaused": false,
    "enablePullRequestBuild": true,
    "enableCommitBuild": true,
    "enableTagBuild": false,
    "enableReleaseBuild": false,
    "cacheTag": 0,
    "serialize": false,
    "serializeBranches": [],
    "lowCoverageLimit": 0,
    "unstableOnLowCoverage": false,
    "dashboardBranchSettings": {
      "runFilter": "commit",
      "branchFilter": [
        "master"
      ]
    }
  },
  "id": "55511f38edd7f2c052e8c637",
  "projectId": 4675042,
  "name": "docsv2",
  "fullName": "Shippable/docsv2",
  "subscriptionId": "540e75583479c5ea8f9e71e1",
  "ownerAccountId": "540e7735399939140041d885",
  "builderAccountId": "540e652c399939140040eb8a",
  "repositoryUrl": "https://api.github.com/repos/Shippable/docsv2",
  "repositorySshUrl": "git@github.com:Shippable/docsv2.git",
  "repositoryHtmlUrl": "https://github.com/Shippable/docsv2",
  "sourceDefaultBranch": "master",
  "isPrivateRepository": false,
  "isOrg": true,
  "isFork": false,
  "sourceId": "35451002",
  "sourcePushedAt": "2017-01-30T23:08:05.000Z",
  "sourceCreatedAt": "2015-05-11T21:26:37.000Z",
  "sourceUpdatedAt": "2016-10-25T04:25:37.000Z",
  "lastBuildGroupNumber": 1275,
  "autoBuild": true,
  "deployPublicKey": "ssh-rsa abc123 Shippable\n",
  "language": "CSS",
  "providerId": "562dbd9710c5980d003b0451",
  "providerLastSyncEndDate": "2017-01-30T23:17:40.740Z",
  "providerLastSyncStartDate": "2017-01-30T23:17:40.391Z",
  "enabledDate": "2015-05-13T00:47:37.105Z",
  "enabledBy": "540e7735399939140041d88b",
  "enabledByUserName": "avinci",
  "type": "ci",
  "consolidateReports": false,
  "createdBy": "540e55445e5bad6f98764522",
  "updatedBy": "540e55445e5bad6f98764522",
  "createdAt": "2016-07-23T10:48:21.580Z",
  "updatedAt": "2017-01-30T23:17:40.740Z"
}
```
---

##Get latest run for a branch

```
/projects/:projectId/branchRunStatus
```

###Response

|HTTP Code      |    Description     |
|---------------|--------------------|
|200|success|  |
|500|Internal Server Error| Check project id.|

```
{
  "projectId": "55511f38edd7f2c052e8c637",
  "branchName": "master",
  "runNumber": 1275,
  "runLengthInMS": 278663,
  "id": "588fc758e8c7130f000b45bd",
  "statusCode": 30,
  "subscriptionId": "540e75583479c5ea8f9e71e1",
  "isPrivate": false,
  "lastCommitShortDescription": "Update shippable.yml",
  "commitSha": "edba134e13c3cd189e1d7db231cee5cf1e8c405b",
  "triggeredBy": {
    "login": "manishas",
    "displayName": "Manisha",
    "email": "manishas@users.noreply.github.com",
    "avatarUrl": "https://avatars.githubusercontent.com/u/2983749?v=3"
  },
  "startedAt": "2017-01-30T23:08:28.791Z",
  "createdAt": "2017-01-30T23:08:08.682Z",
  "endedAt": "2017-01-30T23:13:07.454Z",
  "isPullRequest": false,
  "pullRequestNumber": null,
  "commitUrl": "https://github.com/Shippable/docsv2/commit/edba134e13c3cd189e1d7db231cee5cf1e8c405b",
  "committer": {
    "login": "web-flow",
    "displayName": "GitHub Web Flow",
    "email": "noreply@github.com",
    "avatarUrl": "https://avatars.githubusercontent.com/u/19864447?v=3"
  },
  "isGitTag": false,
  "isRun": true
}
```

---

## Trigger a new run

This route triggers a new build for the default branch of a project.

```
POST /projects/:projectId/newBuild
```

###Response

|HTTP code      |    Status     |    Description    |
|----------|-------------|-------------------|
| 200| OK| New build was successfully triggered|
| 500| Internal Server Error| Check project id|

```
{
  "runId": "56cfe31f388a4f2d00382db1"
}
```

###Trigger a run for a specific branch
You can specify the branch for which you want to trigger a run by specifying a `branchName` in the JSON payload in the request body of the `POST`.

```
{
  "branchName": "xyzFeature"
}
```

Alternatively, branch name can also be specified as a parameter like this:
`POST /projects/:projectId/newBuild?branch=xyzFeature`


###Injecting Global Env Variables
You can also inject global environment variables into the new build by specifying key-value pairs in the JSON payload in the request body of the `POST`.

These key-value pairs have to be set in the `globalEnv` property.

```
{
  "globalEnv": {
    "FOO": "bar",
    "FIZZ": "buzz"
  }
}
```
---
