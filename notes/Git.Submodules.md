---
id: xhmpt7uixakfzneqjwgxn59
title: Submodules
desc: ''
updated: 1669744301814
created: 1669592357363
---

This guide will help you manage submodules in Git. The full guide for submodules can be found [here](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

## What are submodules?

Let's say that you want to use an external Git repository inside of your project. How do you use it without including it inside the main repository itself, and be able to fetch the latest version of it every time you need it?

To solve this issue, Git introduced submodules in the version 7.2. Submodules allow you to **clone an external repository and use it as part of your project, without including into yours**. This way, you are able to treat the two projects as separate.

## What can you do with submodules?

- You can separate the two or more projects that you include in your main repository.
- You can push changes to your repository submodule from your main repository.





