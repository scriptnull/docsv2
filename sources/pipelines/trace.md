page_title: Unified Pipeline Trace
page_description: description of trace functionality
page_keywords: pipelines, Continuous Integration, Continuous Deployment, CI/CD, automation, history


<br>
#Pipeline Traceability
The first question people tend to ask when their application breaks is: what changed? Typically, the more complicated a pipeline gets, the more difficult this question becomes to answer.  To make this easy on the user, Shippable has embedded trace functionality in every pipeline job. It allows you to pick any particular job in your pipeline, and view the entire tree of events that led up to that particular version of that particular job with just a few clicks.  This allows you to go all the way back to the initial code commit that triggered the workflow to begin with.

##Trace
The trace view shows the inputs to a given job. To get to the trace view, click on a job in the SPOG to open the console. Next, click the "trace" tab on the left-hand side. This opens a list of the job's inputs; you can then drill down to see what those inputs are made of.

Let's say you created a release, and now want to see what, exactly, the release contains. When you click trace, the list shows the inputs to that release job:

<img src="../images/trace/trace1.png" alt="majestic trace tab" style="width:700px;"/>

<br>
Here, the release has two inputs: a deploy job (`deploy-pipelines-demo`), and a version resource (`pipelines-demo-version`). To see what the deploy is made of, click the arrow icon to show the inputs to that deploy job:

<img src="../images/trace/trace2.png" alt="snips and snails" style="width:700px;"/>

<br>
This deploy job has three inputs: `docker-options`, `ecs-cluster`, and `pipelines-demo-manifest`. Since the manifest will have the details on the images being deployed, let's look at that:

<img src="../images/trace/trace3.png" alt="presto expando" style="width:700px;"/>

<br>

Great - the release job contains `pipelines-demo-image`, and the image tag is `master.9`. But what's in `master.9`? Clicking on it takes you to the CI job that created and pushed the image:

<img src="../images/trace/trace4.png" alt="CI!" style="width:700px;"/>

<br>

From there, clicking on the Sha link takes you to that commit on the source provider. Congratulations! You've traced a release back to its source code, all in just a few clicks.
