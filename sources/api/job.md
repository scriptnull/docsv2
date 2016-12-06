page_title: Shippable API for Jobs
page_description: How to interface with Shippable's API
page_keywords: shippable, API, HTTP

# Jobs
Jobs are individual builds in a Run.

For example, a matrix build will have multiple Jobs that are a part of the Build Run, one for each value in the matrix.

For more on matrix builds, [check out our documentation on build config](/ci/advancedOptions/matrixBuilds/)

##Get all jobs
This route returns all jobs across projects and subscriptions and runs.

```
GET /jobs
```

###Response

|HTTP Code      |    Description     |
|---------------|--------------------|
|200|success|


```
{
    "version": "0.12",
    "jobNumber": 1,
    "runNumber": 5,
    "runId": "56d28c4e42bc2e11001ed507",
    "subscriptionId": "56cfe9f3c77dae78a8eaa746",
    "subscriptionOrgName": "shippableapi",
    "projectId": "56cfe9f4c77dae78a8eaa748",
    "projectName": "sample_node",
    "statusCode": 70,
    "startedAt": "2016-02-28T05:57:35.571Z",
    "endedAt": "2016-02-28T05:57:48.307Z",
    "jobLengthInMS": 12736,
    "timeoutMS": 5400000,
    "branchName": "master",
    "isPullRequest": false,
    "pullRequestNumber": null,
    "createdBy": "56cfe9b668147b1300df0c69",
    "commitSha": "4bec06511580963fa011e07bf467732a6d562b29",
    "isPrivateRepository": false,
    "createdAt": "2016-02-28T05:57:34.856Z",
    "allowFailure": false,
    "branchCoveragePercent": 0,
    "sequenceCoveragePercent": 0,
    "testsErrors": 0,
    "testsSkipped": 0,
    "testsPassed": 0,
    "testsFailed": 0,
    "totalTests": 0,
    "env": [
      "FOO=FUBU",
      "FO1=BUBU",
      "BOO=GUBU"
    ],
    "isCompleted": true,
    "id": "56d28c4e0dc2d81300a2b634"
}
```

---

## Get a job

```
GET /jobs/:id

```

###Response

|HTTP Code      |    Description     |
|---------------|--------------------|
|200|success|
|500|Internal Server Error. Check job id.|

```
Status 200 OK
```

```
{
    "version": "0.12",
    "infra": {
        "pre_prov": [],
        "pre_prov_boot": {
            "image_name": "drydock/u14nod",
            "image_tag": "prod",
            "pull": true,
            "options": "--privileged=true --net=bridge",
            "envs": "",
            "isOfficialImage": true,
            "isLegacyImage": false
        },
        "prov": [],
        "post_prov": [],
        "post_prov_boot": {
            "image_name": "drydock/u14nod",
            "image_tag": "prod",
            "pull": true,
            "options": "--privileged=true --net=bridge",
            "envs": "",
            "isOfficialImage": true,
            "isLegacyImage": false
        },
        "smoke_test": [],
        "on_success": [],
        "on_failure": []
    },
    "build": {
        "pre_ci": [],
        "pre_ci_boot": {
            "image_name": "drydock/u14nod",
            "image_tag": "prod",
            "pull": true,
            "options": "--privileged=true --net=bridge",
            "envs": "",
            "isOfficialImage": true,
            "isLegacyImage": false,
            "containerName": "c.exec.sample_node.5.1",
            "sectionName": "build_pre_ci_boot"
        },
        "ci": [
            "ps -eax",
            "printenv",
            "ls -al",
            "ssh-add -l"
        ],
        "post_ci": [],
        "post_ci_boot": {
            "image_name": "drydock/u14nod",
            "image_tag": "prod",
            "pull": true,
            "options": "--privileged=true --net=bridge",
            "envs": "",
            "isOfficialImage": true,
            "isLegacyImage": false
        },
        "smoke_test": [],
        "on_success": [],
        "on_failure": [],
        "cache": false
    },
    "deploy": {
        "pre_ci": [],
        "pre_start_boot": {
            "image_name": "drydock/u14nod",
            "image_tag": "prod",
            "pull": true,
            "options": "--privileged=true --net=bridge",
            "envs": "",
            "isOfficialImage": true,
            "isLegacyImage": false
        },
        "start": [],
        "post_start": [],
        "post_start_boot": {
            "image_name": "drydock/u14nod",
            "image_tag": "prod",
            "pull": true,
            "options": "--privileged=true --net=bridge",
            "envs": "",
            "isOfficialImage": true,
            "isLegacyImage": false
        },
        "smoke_test": [],
        "on_success": [],
        "on_failure": []
    },
    "submoduleEnabled": true,
    "jdk": "",
    "gemfile": "",
    "compiler": "",
    "jobNumber": 1,
    "runNumber": 5,
    "runId": "56d28c4e42bc2e11001ed507",
    "subscriptionId": "56cfe9f3c77dae78a8eaa746",
    "subscriptionOrgName": "shippableapi",
    "projectId": "56cfe9f4c77dae78a8eaa748",
    "projectName": "sample_node",
    "scmURL": "https://github.com/shippableapi/sample_node.git",
    "statusCode": 70,
    "statusMessage": "job generated",
    "startedAt": "2016-02-28T05:57:35.571Z",
    "endedAt": "2016-02-28T05:57:48.307Z",
    "jobLengthInMS": 12736,
    "timeoutMS": 5400000,
    "branchName": "master",
    "isPullRequest": false,
    "pullRequestNumber": null,
    "pullRequestBaseBranch": null,
    "createdBy": "56cfe9b668147b1300df0c69",
    "commitSha": "4bec06511580963fa011e07bf467732a6d562b29",
    "addons": [],
    "isPrivateRepository": false,
    "node": "56d28ada0dc2d81300a27b0e",
    "updatedAt": "2016-02-28T05:58:19.702Z",
    "createdAt": "2016-02-28T05:57:34.856Z",
    "allowFailure": false,
    "branchCoveragePercent": 0,
    "sequenceCoveragePercent": 0,
    "jobErrorMsgs": [],
    "testsErrors": 0,
    "testsSkipped": 0,
    "testsPassed": 0,
    "testsFailed": 0,
    "totalTests": 0,
    "steps": [
        {
            "id": "58c89b5c-4d9c-40b6-aaae-63873f693b7a",
            "execOrder": 11,
            "scriptType": "collect_stats",
            "who": "mexec",
            "script": "#!/bin/bash -e\n\n######################## META SECTION ################################\n#\n# Prints the command start and end markers with timestamps\n# and executes the supplied command\n#\n\nbn  trap before_exit EXIT\n  get_bash_info\n  ret=$?\n  [ \"$ret\" != 0 ] && return $ret;\n  is_success=true\n}\n\ntrap before_exit EXIT\nexec_grp \"job_node_info\" \"Job node info\"\n"
        },
        {
            "id": "e73ba84f-d088-46c8-9ea4-b29a236c3f3c",
            "execOrder": 30,
            "scriptType": "boot",
            "who": "mexec",
            "script": "#!/bin/bash -e\n\n######################## META SECTION ################################\n#\n# Prints the command start and end markers with timestamps\n# and executes the supplied command\n#\n\nbefore_script_exit() is_success=true\n  echo \"$SECTION script completed\"\n}\n\nif [ \"$PULL\" == true ] && [ \"$SHIPPABLE_CACHE_ENABLED\" == false -o \"$RESET_CACHE\" == true ]; then\n  trap before_exit EXIT\n  exec_grp \"pull_job_image\"\nfi\n\ntrap before_exit EXIT\nexec_grp \"boot_container\"\nexec_grp \"wait_for_exit\" \"wait_for_exit\" \"false\"\n"
        },
        {
            "id": "92f4cae1-3841-4f8e-8798-ae6f2381fc03",
            "execOrder": 50,
            "scriptType": "run",
            "who": "cexec",
            "script": "#!/bin/bash -e\n\n######################## META SECTION ################################\n#\n# Prints the command start and end markers with timestamps\n# and executes the supplied command\n#\n\nCall build_ci\n#\ntrap before_exit EXIT\nexec_grp \"build_ci\"\n"
        },
        {
            "id": "1dae974b-4cb2-4380-a706-c4b59fa0c521",
            "execOrder": 60,
            "scriptType": "reports",
            "who": "cexec",
            "script": "#!/bin/bash -e\n\n######################## META SECTION ################################\n#\n# Prints the command start and end markers with timestamps\n# and executes the supplied command\n#\n\nexec_cmd \"$parse_coverage_cmd\"\n  else\n    exec_cmd \"echo 'No coverage reports exist, skipping coverage report processing'\"\n  fi\n\n  is_success=true\n}\n\n\ntrap before_exit EXIT\nexec_grp \"copy_test_reports\" \"parse test reports\" \"true\"\n\ntrap before_exit EXIT\nexec_grp \"copy_coverage_reports\" \"parse coverage reports\" \"true\"\n"
        }
    ],
    "keyIntegrations": [],
    "deployIntegrations": [],
    "hubIntegrations": [],
    "notificationIntegrations": [
        {
            "recipients": [
                "shippableapi@gmail.com"
            ],
            "integrationName": "email",
            "type": "email",
            "on_success": "change",
            "on_failure": "always",
            "on_pull_request": "always",
            "on_start": "never",
            "isValid": true,
            "accountIntegration": null
        }
    ],
    "services": [],
    "bundlerArgs": [],
    "env": [
        "FOO=FUBU",
        "FO1=BUBU",
        "BOO=GUBU"
    ],
    "isCompleted": true,
    "id": "56d28c4e0dc2d81300a2b634"
}
```
---

## Download console logs

```
GET /jobs/:jobId/consoles?download=true

```

###Response

|HTTP Code      |    Description     |
|---------------|--------------------|
|200|success|
|500|Internal Server Error. Check job id.|

Response will contain the console log for the job.

---

## Get coverage report

```
GET /jobs/:jobId/jobCoverageReports
```

###Response

|HTTP Code      |    Description     |
|---------------|--------------------|
|200|success|
|500|Internal Server Error. Check job id.|

```
{
        "jobId": "56d300466492cb120027648c",
        "createdBy": "56cfe9b668147b1300df0c69",
        "updatedAt": "2016-02-28T14:19:41.682Z",
        "createdAt": "2016-02-28T14:19:41.682Z",
        "classes": [
            {
                "lineRate": "1",
                "branchRate": "1",
                "name": "sample_node_mongo",
                "fileName": "Gruntfile.js",
                "id": "56d301fd6492cb1200276aa6"
            },
            {
                "lineRate": "1",
                "branchRate": "1",
                "name": "sample_node_mongo",
                "fileName": "test.js",
                "id": "56d301fd6492cb1200276aa5"
            }
        ],
        "branchCoverage": {
            "coveredBranches": 0,
            "validBranches": 0,
            "branchRate": 1
        },
        "lineCoverage": {
            "lineRate": 1,
            "validLines": 0,
            "coveredLines": 0
        },
        "id": "56d301fd6492cb1200276aa4"
    }

```
---

## Get test result report

```
GET /jobs/:id/jobTestReports
```

###Response

|HTTP Code      |    Description     |
|---------------|--------------------|
|200|success|
|500|Internal Server Error. Check job id.|

```

{
        "jobId": "56d300466492cb120027648c",
        "total": 3,
        "totalPassing": 3,
        "totalFailures": 0,
        "totalErrors": 0,
        "totalSkipped": 0,
        "updatedAt": "2016-02-28T14:19:41.329Z",
        "createdAt": "2016-02-28T14:19:41.329Z",
        "failureDetails": [],
        "errorDetails": [],
        "id": "56d301fd13067a12006bf2ec"
}

```
---

## Delete a job

```
DELETE /jobs/:id
```
###Response

|HTTP Code      |    Description     |
|---------------|--------------------|
|200|success|
|500|Internal Server Error. Check job id.|

This will return the entire Job object that is being deleted. The schema is the same as the one for GET /jobs/:id.

---
