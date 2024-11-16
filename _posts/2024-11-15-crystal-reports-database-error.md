---
layout: post
published: true
status: publish
title:  "Crystal Reports - Failed to load database information"
author:
  display_name: makoa
  login: makoa
  email: makoa@makoajacobsen.com
  url: ''
categories:
- Dev
tags:
- Crystal Reports
---

I've never worked with Crystal Reports before. The last step between me and sunsetting an old windows click-install application was running one crystal report in a loop and writing out PDF files for entry, named appropriately.

After installing the proper visual studio extension, much to my surprise, I was able to open the crystal report and edit it. Without much effort, I was also able to get it to execute in code after references the appropriate Crystal Reports DLLs. But I could not get passed the following:

```
CrystalDecisions.CrystalReports.Engine.DataSourceException: 'Error in File TestCrystalReport 19448_11024_{B02CC796-AEC3-4C2B-97F0-84F3FFE4D5F0}.rpt:
Failed to load database information.'
```

And it occurred right when I `SetDataSource()` with a `DataSet` object. I tried changing the data sources, referencing the database directly, which would run the report (and put everything in one PDF file), but it would not allow me to set the data source. And I needed to be able to set the data source so that I could get the PDF generation in a loop and set each report run one at a time (performance was not a priority - this was a run and done process once finished). And that was the best information I could get from Crystal Reports for troubleshooting - something about database information.

After some searching and AI prompting, it was clear that it was a generic error that could mean anything, even not related to the database or its connection.

So after wandering, I found [a site](http://www.crystalreportsbook.com/forum/forum_posts.asp?TID=12308) that made mention of making sure to install either the 64-bit or 32-bit versions of the DLLs appropriately depending on ODBC driver. I remembered early in the configuring of the build settings that I had the application building as a 32-bit app even thinking that since the Crystal Reports SDK installed in Program Files (x86) that it would be best. So I decided to check my references, and I ended up including the x64 version of the DLLs (for whatever reason). I wasn't going to redo the references so why not just change the build to force x64 instead of x86 (simple change in Visual Studio). Build and run. And then viola! The report executed with a PDF containing the first record's report!

Lesson learned: make sure the referenced DLL build matches the target build.