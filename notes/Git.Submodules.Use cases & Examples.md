---
id: l4fnrqxx339pk5781l2n8pb
title: Use cases & Examples
desc: ''
updated: 1669861038325
created: 1669734539132
---

Here are some use cases that you may encounter with git submodules

> These examples work with a repository called **STEM** (located at https://github.com/LamboLead/STEM) and a submodule called **LLT** (located at https://github.com/LamboLead/LLT).

## Index

 1. [[Adding a new submodule into your repository | #1-adding-a-new-submodule-into-your-repository]]
 2. [[Cloning a project with submodules | #2-cloning-a-project-with-submodules]]
 3. [[Working on a project with submodules | #3-working-on-a-project-with-submodules]]
     1. [[Pulling changes from the submodule | #31-pulling-changes-from-the-submodule]]
        1. [[Pointing to other branch when pulling from the submodule | #311-pointing-to-other-branch-when-pulling-from-the-submodule]]
     2. [[Pulling changes from the main repository | #32-pulling-changes-from-the-main-repository]]
 4. [[Working on a submodule | #4-working-on-a-submodule]]
    1. [[Tracking changes in the submodule | #41-tracking-changes-in-the-submodule]]
    2. [[Publishing changes in the submodule | #42-publishing-changes-in-the-submodule]]
    3. [[Merging submodule changes | #43-merging-submodule-changes]]
 5. [[Other use cases | #5-other-use-cases]]
    1. [[Update the submodule without the --merge option|Git.Submodules.Use cases & Examples#update-the-submodule-without-the---merge-option]]
    2. [[Update the submodule while haven't commited changes in the submodule|Git.Submodules.Use cases & Examples#update-the-submodule-while-havent-commited-changes-in-the-submodule]]
    3. [[Push new changes into the main repository without pushing changes from the submodule|Git.Submodules.Use cases & Examples#push-new-changes-into-the-main-repository-without-pushing-changes-from-the-submodule]]

## To finish

- [ ] [[Merging submodule changes | #43-merging-submodule-changes]]

## 1. Adding a new submodule into your repository

To add a new submodule into your repository, use the `git submodule add` command and the URL of the GitHub repository.

```git 
  $ git submodule add https://github.com/LamboLead/LLT
```

This should do a few things:
- Create a new folder whose name is the name of the repository and clone the repo inside of it.
- Create a new file **`.gitmodules`**, which is a config file that stores the mapping between the project's URL and the local subdirectory that stores the submodule.

> Git sees the new created folder as a submodule and doesn't track its contents when the HEAD is not in that directory.

Remember to commit and push the new changes:
```git
  $ git commit -am "Added LLT module"
  $ git push origin master
```

## 2. Cloning a project with submodules

When you clone a project that has submodules inside, _by default you get the directories that contain submodules, but none of the files within them yet_.

You need to follow these steps:

1. Run `git submodule init` to initialize the local config file.
2. Run `git submodule update` to fetch all the data from that project and check out the appropiate commit listed in your project.
   
You can simplify the previous commands by running:
```git
  $ git submodule update --init --recursive
```
If you haven't cloned the repository yet, you can run the  command:
```git
  $ git clone --recurse-submodules https://github.com/LamboLead/STEM
```

## 3. Working on a project with submodules

Now that you have a copy of a project with submodules, you can collaborate on your teammates on both the main project and the submodule project.

### 3.1. Pulling changes from the submodule

This is one of the simplest use cases: To consume a subproject and get updates from it from time to time, but not actually modifying anything.

If you want to check for new updates in a submodule, you go into the submodule's directory and run:
```git
  $ git fetch
  $ git merge origin/master
```
If you go back to the main project and run `git diff --submodule` you can see that the submodule was updated, and get a list of commits that were added to it.

If you want to simplify the previous commands, run `git submodule update --remote`. This will fetch and update all the submodules for you.

> If you make a commit at this point, then you will lock the submodule into having the new code when other people update.

> **This command will assume that you want to update the submodule at checkout to the HEAD of the remote repository.**

### 3.1.1. Pointing to other branch when pulling from the submodule

If you want to bring the changes from a specific branch from the submodule (for example, the _stable_ branch), you need to set it up:

```git
  $ git config -f .gitmodules submodule.LLT.branch stable
```
If you leave off the `-f`, **`.gitmodules`** will only make the change for you, but it makes more sens to track that information with the repository.
> After you make these changes, you need to commit and push to your main repository.

### 3.2. Pulling changes from the main repository

When you want to pull changes from the remote main repository, `git pull` is not enough to update the submodules (this command only fetches the changes, but doesn't integrate them).

To finalize the update, you need to run the command `git submodule update --init --recursive`

## 4. Working on a submodule

### 4.1. Tracking changes in the submodule

When using submodules, you may need to update the submodule as well at the same time as you're working on the code in the main project.

When you run `git submodule update` to fetch changes from the submodule repositories, Git will get the changes and update the files in the subdirectory but will leave the repository in what's called a **_detached HEAD_** state.

> The **_detached HEAD_** state means that there is no local working branch tracking changes. This means that even if you commit changes to the submodule, those changes will be lost next time you run `git submodule update`.

To track changes in a submodule, you need to go into its folder and check out a branch to work on:

```git
  $ cd LLT/
  $ git checkout stable
```

Now, you need go to the main repository folder and update the repository with the `--merge` option:

```git
  $ git submodule update --remote --merge
```

Now you have the new changes already merged into your local `stable` branch.

> When you update the submodule, if you forget the `--merge` option, Git will update the submodule to whatever is on the server and reset the submodule to a **_detached HEAD_** state.

### 4.2. Publishing changes in the submodule

Let's say that you've made some changes into your submodule directory and you need to publish them.

To do so, you need to go into the submodule's folder and push the changes normally:

```git
  $ git commit -am "Made some changes into the LLT submodule"
  $ git push LLT stable
```

### 4.3. Merging submodule changes

**_Go to the fucking tutorial and try to figure it out_**

## 5. Other use cases

These are other cases that may happen to you.

### Update the submodule without the `--merge` option

  If this happens, you can go back into the directory and check out to your branch (which contains your work) and merge or rebase or rebase `origin/stable` or whatever remote branch you want, manually.

### Update the submodule while haven't commited changes in the submodule

  Git will fetch the changes but not overwrite unsaved work in your submodule directory.
  If you made changes that conflict with something changed in the submodule, Git will let you know when you run the update.

  In this case, you can go into the submodule directory and fix the conflict just as you normally would.

### Push new changes into the main repository without pushing changes from the submodule

  If you push changes from the main repository without pushing the submodules' as well, other people who try to check out our changes are going to be in trouble. Those changes in the submodule will only exist in our local copy.

  To make sure this doesn't happen, you can **check that all your submodules have been pushed properly before pushing the main project**:

  ```git
    $ git push --recurse-submodules=check
  ```

  The `--recurse-submodules=check` option will make the push to fail if any of the committed submodule changes haven't been pushed.

  You can **push your main project changes along with the submodule changes** by running:

  ```git
    $ git push --recurse-submodules=on-demand
  ```

  > You can set the previous behavior as default by running `git config push.recurseSubmodules check` or `git config push.recurseSubmodules on-demand`.