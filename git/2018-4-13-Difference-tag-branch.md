---
layout: default
title: Difference between tag and branch
parent: Git
nav_order: 1
---

What's the difference between **tag** and branch in **git**?

When writing the backend logic with jgit, I realize that `git checkout` will be executed with a priority, i.e, first checkout the **branch** and then the **tag**.

A **tag** represents a version of particular moment in a branch. A **branch** represents a separate thread of development where you can push the evolutions and it may be merged into the main workflow.
Usually, a **tag** allows to recreate a version from it, e.g, a released version. While a **branch** contains the on-going updates on a particular version. You'll make a main line branch based on a particular version and separate a particular branch for fixing a bug or for delivering to a client. And eventually, these branches will be merged back to the main line.

More technical:
    
* **tags** reside in `refs/tags/` namespace, and can point to tag objects (annotated and optionally GPG signed tags) or directly to commit object (less used lightweight tag for local names), or in very rare cases even to tree object or blob object (e.g. GPG signature).
* **branches** reside in `refs/heads/` namespace, and can point only to commit objects. The `HEAD` pointer must refer to a branch (symbolic reference) or directly to a commit (detached `HEAD` or unnamed branch).
* **remote-tracking branches** reside in `refs/remotes/<remote>/` namespace, and follow ordinary branches in remote repository `<remote>`
