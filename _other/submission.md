---
layout: page
toc: false
title: Lab Submission
short_title: Lab Submission
indent: 0
number: 2
---


To submit your lab you will create a *tag* in your Git repository that points to your submission for the lab.

Once you are done the lab, and want to submit, create a tag in your repo that the TAs can use to grade this lab, and then push like this:

    git tag lab1_submission
    git push origin lab1_submission

This tag will point to your most recent commit of whichever branch you are currently located on (so make sure all of your changes are committed before running this).  If you are not confident you did this correctly, you may want to go to a new directory (not in your repo) and run `git clone --branch lab1_submission <repo_url>` to clone your tag and verify that it builds and runs correctly.

If, after you create this tag, you want to change it (ie, re-submit your code), you can re-run the above commands and include the `--force` option to overwrite the tag, ie:

    git tag --force lab1_submission
    git push --force origin lab1_submission


For later labs, update the tag name appropriately:
  * lab1_submission
  * lab2_submission
  * lab3_submission

## Important!
* <span style="color:red">**If you don't type the tag name correctly, it won't count as submitted.**.</span>

* <span style="color:red">**Make sure you have [this](https://github.com/byu-cpe/ecen629_student/blob/master/.github/workflows/submission.yml) file in your repo (and it isn't modified), as it will log the submission time of your lab.**
</span>
