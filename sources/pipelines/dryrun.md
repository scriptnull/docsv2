page_title: Pipeline Dry Run
page_description: description of dry run functionality
page_keywords: pipelines, Continuous Integration, Continuous Deployment, CI/CD, automation, history, dry run

<br>
#Pipeline Dry Run
Dry Run feature allows you to validate and test your syncRepo pipelines before actually adding them.
Currently we support dry-run pipelines for branches only, dry-run pipelines for Pull Requests is work in progress.

##For branches
To check dry run pipeline for a branch, goto subscriptions pipeline tab. On the right, click
on `Dry Run` button. Search and select the project for which you want to check dry run pipeline.

<br>
<img src="../images/dryrun/search_project.png" alt="search project" style="width:700px;"/>

<br>
After selecting the project, you should select a branch for which you whould like to view dry run pipeline.

<img src="../images/dryrun/search_branch.png" alt="search branch" style="width:700px;"/>

<br>
After selecting the branch, your dry run pipeline will show up. It will look exactly same as how it will look if you add an actual syncRepo for selected project and branch.
Any errors or warnings will be shown to you so that you can correct them before adding syncRepo.
<img src="../images/dryrun/dry_run_pipeline.png" alt="dry run pipeline" style="width:700px;"/>

To check the dry run pipeline for any other project or branch, you can select the same from dropdowns and dry run pipelines for those will be shown.

Please note that, since we support resource sharing between syncRepos in same subscription, if any step(IN, OUT or NOTIFY) resources or jobs for a job are not present in current syncRepo but are present
in other syncRepo in the same subscription then those resources or jobs
will show up in dry run pipeline.
