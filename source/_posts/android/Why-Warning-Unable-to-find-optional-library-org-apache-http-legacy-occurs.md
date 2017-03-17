---
title: 'Why Warning:Unable to find optional library: org.apache.http.legacy occurs?'
date: 2017-03-13 23:02:30
categories: android
tags: [android,legacy,http]
---
I guess, the easier way to solve this issue without having to reinstall the SDK is to create a file called optional.json in <sdk-path>\platforms\android-23\optional\ directory with the following content:
``` shell
[
  {
    "name": "org.apache.http.legacy",
    "jar": "org.apache.http.legacy.jar",
    "manifest": false
  }
]
```
来源：[http://stackoverflow.com/questions/33898857/why-warningunable-to-find-optional-library-org-apache-http-legacy-occurs](http://stackoverflow.com/questions/33898857/why-warningunable-to-find-optional-library-org-apache-http-legacy-occurs)