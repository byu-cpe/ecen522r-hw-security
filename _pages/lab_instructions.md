---
layout: lab
toc: true
title: "Lab Instructions"
order: 5
sidebar: true
under_construction: false
---

## Setup
Code and submission is done using Github Classroom. 


1. Use this [invitation link](https://classroom.github.com/a/gzzaL2NG) to create a private Github repository that you will use for the class.

1. Your repository will begin empty, but you will need to import the starter code.  To do this we will do a bare clone of the starter code repository, and push it to your repository.  Then you can delete this clone.  Make sure to replace the URL in the third step with the URL of your repository, that you can find by clicking the *SSH* button on your repository page.  You can run these commands in any directory you want. 

        git clone --bare git@github.com:byu-cpe/ecen522r_security_student.git
        cd ecen522r_security_student.git/
        git push --mirror git@github.com:byu-ecen522r-security-classroom/labs-jgoeders.git
        cd ..
        rm -rf ecen522r_security_student.git



## Submission

To submit your lab you will create a *tag* in your Git repository that points to your submission for the lab.

Once you are done the lab, and want to submit, create a tag in your repo that the TAs can use to grade this lab, and then push like this:

    git tag lab0_submission
    git push origin lab0_submission

This tag will point to your most recent commit of whichever branch you are currently located on (so make sure all of your changes are committed before running this).  If you are not confident you did this correctly, you may want to go to a new directory (not in your repo) and run `git clone --branch lab1_submission <repo_url>` to clone your tag and verify that it builds and runs correctly.

If, after you create this tag, you want to change it (ie, re-submit your code), you can re-run the above commands and include the `--force` option to overwrite the tag, ie:

    git tag --force lab0_submission
    git push --force origin lab0_submission


For later labs, update the tag name appropriately:
  * lab1_submission
  * lab2_submission
  * lab3_submission

## Important!
* <span style="color:red">**If you don't type the tag name correctly, it won't count as submitted.**.</span>

* <span style="color:red">**Make sure you have [this](https://github.com/byu-cpe/ecen522r_security_student/blob/main/.github/workflows/submission.yml) file in your repo (and it isn't modified), as it will log the submission time of your lab.**
</span>




