# Console logs

Console logs are available real time for every build as explained in the [Build UI section](/navigatingUI/builds/detail/#console-section).

We store your console logs for at least 3 months after your build completes. Please note that for builds that ran prior to February 17, 2017, this period was 14 days.

There are some limits for log size beyond which we take specific actions:   

* If the log for your build exceeds 30,000 lines, it will not be displayed in the UI. However, you will still be able to download it.
* Maximum allowed log size per build is 64mb at this time. If your log for a build  exceeds 64 MB, it will not be displayed in the UI and you will not be able to download it. Your build will continue and the result will be unaffected but debugging might be a challenge due to lack of logs.
