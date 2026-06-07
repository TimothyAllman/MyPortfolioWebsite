---
title: How to Git
subtitle: Like Sharepoint... but better
date: 2026-03-18
authors:
  - name: Timothy Allman
---

## Some Background

I was first introduced to git in an undergraduate digital signals course, in all honesty I didn't think much of it at the time and missed the point of what was being demonstrated. After that most of the time I just went to the github repo for the course to download the assignment for the week manually, and never really thought about git again. I knew it existed but didn't really understand what it was

A few years later when starting my first job I was reintroduced to git. I remember knowing that it was going to be something I was going to use every day but thinking that it was going to be hugely complicated and technically complex.  
Little did I know it would change my life forever and become one of the things I things I love most about writing code

## What Is Git

in very loose simple terms Git is like saving your word document or your excel files on a remote sharepoint
it stores versions

git is much more powerful than that because it can be used without fully locally and doesn't need the remote. But the remote origin (copy in the cloud) is by far one of the most prevalent features and so it will be the one we focus on here

<!-- fetch
pull [to you local laptop]
push [to the "cloud"/remote]
[create] branch

stage + commit

lets say you have a new project -->

:::{attention}
we will be explaining Git in the  context of comparing it to Sharepoint (but any remote-copy local-copy and versioning system would sufficeas a point of comparison)
:::

## The folder in the cloud
Imagine you have a project/thing/codebase/collection of files that exists on sharepoint in the cloud
```image

```
## your local copy\
The first thing we will want to do is get this group of files onto our local computer 
(i.e. create a copy of what is on the cloud onto our local drive so that we can interact with it more efficinetly/when we don't have internet)

in sharepoint this is done my mapping a drive or linking the folder that contains everything

inthe Git workflow what we will need is the `clone` command. This clones the files from the cloud and creates a duplicate on our machine

## branching off in a new direction

after we have the files on our computer we will probably start doing work/adding new files.
In git we don't want to do this in the `main` branch. We try to keep that untouched. And so the next thing we will want to do before making and changes is to create a `branch`. when we do this we will give the branch a meaningful/descriptive name so that we and anybody else who sees this branch knows what work we are doing in it/what edits we will be making. For example we may name a branch `feature/AddNewHelloWorldFunction` if we will be adding a new file/function or `fix/FolderNotFoundErrorInFunctionXyz if the xyz function is having problems with its folder creation process 

this is similar to duplicating the folder inside the drive and giving it a new name in sharepoint


## making the edits
Now that we have an isolated branch we can make our edits. 
In this case lets assume we 

this is similar to just making your changes in 


## Lock the Changes in
now that the changes are made we need to save them.
saving in git is called committing as you are commiting the changes and finalising the edits you made
In a git based workflow this is a three part process.
-first we stage them 
-second we write a message about what it is we have done (this is know as a commit message)
-lastly we commit them (either via a commit button or via the commit command in a terminal)


this is very similar in sharepoint or other programs where you first have to 
-select the file you want to save
-then give it a new name 
-and finally clicking the save button to 


## Push it back to the cloud

## Pull in other branches
it is very





will call this the top level folder

now imagine that in this folder we have a box/container and in this box you have a bunch of files

```image

```

lets say we wnat to add to this box (like add another file) and lets say we have to do this by pressing

## The Remote Cloud Far Far Away

## The Benefits

This has a number of benefits the main
branch helps keep things isolated and understandable
you can have one codebase/workspace
and you might need to make 3 "changes"
you could bundle these up in one change/folder
but it is much better practise to create three seperate folders/copies of the main workspace
and then make each change seperate so that each bundle of work only makes on change
branching encourages this sort of behaviour and thus leads to nicer organisation which is especially helpful in large projects 

## Quick Summary
if you forget everything else here is a quick cheat sheet

```{list-table} 
:header-rows: 1
:widths: 20 80

* - Git Command
  - The Thing It Is Most Similar To

* - `clone` [to you local laptop]
  - downloding a workspace from the cloud onto your locacl machine

* - `branch` [to you local laptop]
  - creating a new folder inside the workspace where you are going to make edits

* - `stage` + `commit message` + `commit` 
  - saving your new edits via save as

* - `push` [to the cloud/remote]
  - uploading your new folder to sharepoint

* - `pull` [to you local laptop]
  - downloading someone elses folder to sharepoint

* - `fetch`
  - refreshing you local folder to see all the files
```
