---
layout: lab
toc: true
title: "Lab Instructions"
order: 5
sidebar: true
under_construction: false
icon: fas fa-person-chalkboard
---

## Setup
Code and submission is done using Github Classroom. 


1. Use this [invitation link](https://classroom.github.com/a/mVt5QJY0) to create a private Github repository that you will use for the class.

1. Your repository will begin empty, but you will need to import the starter code.  To do this we will do a bare clone of the starter code repository, and push it to your repository.  Then you can delete this clone.  Make sure to replace the URL in the third step with the URL of your repository, that you can find by clicking the *SSH* button on your repository page.  You can run these commands in any directory you want. 

        git clone --bare git@github.com:byu-cpe/ecen522r_security_student.git
        cd ecen522r_security_student.git/
        git push --mirror git@github.com:byu-ecen522r-security-classroom/labs-jgoeders.git
        cd ..
        rm -rf ecen522r_security_student.git

1. **Go to LearningSuite and complete the "Github URL" quiz.  You must do this to receive credit for labs in the course.**

## Getting New Changes to the Lab Files

I will regularly push updates to the starter code repository.  You can pull these changes into your repository by following these steps:

  - When you clone your repo, Git will create a *remote* called *origin* that is connected to your repo. 
  - You can view your remotes by typing `git remote`, and even check the linked URL by typing `git remote get-url origin`.
  - Next, you should add a new remote.  You can call it whatever you like, but a good name is *startercode*.  This remote will link to the starter code repository.  This will allow you to retrieve any updates I make to the code provided to you (I will probably need to make some updates to the code during the semester).  You do this like so:

        git remote add startercode https://github.com/byu-cpe/ecen522r_security_student.git


Then, if you ever need to pull down changes I make, you can do the following to fetch the latest changes from the starter code and merge them into your code:
  
    git fetch startercode
    git merge startercode/main

## Submission

To submit your lab you will create a *tag* in your Git repository that points to your submission for the lab.

Once you are done the lab, and want to submit, create a tag in your repo that the TAs can use to grade this lab, and then push like this:

    git tag lab_haha
    git push origin lab_haha

This tag will point to your most recent commit of whichever branch you are currently located on (so make sure all of your changes are committed before running this).  If you are not confident you did this correctly, you may want to go to a new directory (not in your repo) and run `git clone --branch lab_haha <repo_url>` to clone your tag and verify that it builds and runs correctly.

If, after you create this tag, you want to change it (ie, re-submit your code), you can re-run the above commands and include the `--force` option to overwrite the tag, ie:

    git tag --force lab_haha
    git push --force origin lab_haha


For later labs, update the tag name appropriately:
  * lab_bus_snooping
  * lab_modchip
  * lab_trojan_i
  * lab_trojan_ii
  * lab_spa
  * lab_jtag
  * lab_dpa_i
  * lab_dpa_ii
  * lab_puf_trng

## Important!
* <span style="color:red">**If you don't type the tag name correctly, it won't count as submitted.**.</span>

* <span style="color:red">**Make sure you have [this](https://github.com/byu-cpe/ecen522r_security_student/blob/main/.github/workflows/submission.yml) file in your repo (and it isn't modified), as it will log the submission time of your lab.**
</span>




