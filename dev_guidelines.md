---
layout: default
title: Dev Model
permalink: /dev-model/
---

# LCSR Development Model

## Source Code Repository Hosting

There are two servers which host open-source LCSR resources, depending on their
licenses and intended audience. Code meant for public use under an open-source
license is hosted on [GitHub](github.com), while any private code or code which
is published only for reference may be hosted internally on the [LCSR
GitLab](https://git.lcsr.jhu.edu/public/projects). Limited-access to private
source code resources may also be granted on a case-by case basis.

See [Software](/software) for an index of all active JHU LCSR GitHub organizations.

## Git Forking Model

LCSR repositories follow a centralized git development model. The "central"
repository follows a strict branching and tagging model (described below), but
both developers and non-privileged users are encouraged to develop in forks and
submit pull requests for contributions. See the section on [code
reviews](#code-reviews) below for details on reviewing pull requests.

## Git Branching Model

The branching model used by LCSR projects is essentially a simplified version
of the [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/)
development model. All repositories hosted on LCSR GitHub organizations must
conform to this branching model.

Similarly to *Git Flow*, we use following standard branches and branch types
(described in more detail in the subsequent sections):

- **master** -- The main "operational" branch, which should never be broken.
- **devel** -- The "volatile" branch where the latest features and bugfixes get integrated for multiple users to test. 
- **feature-***-- Branches for backwards-compatible features pending merge into `devel`
- **fix-***-- Branches for backwards-compatible bugfixes pending merge into `devel`
- **breaker-***-- Branches for breaking features pending merge into `devel`
- **hack-*** -- Branches for variants of functionality which are done quickly for prototyping purposes.

Due to limited resources, we do not maintain release branches. As such,
bugfixes do not get pushed back into older versions of the code, they are only
merged into `devel` before being merged into `master`.

### **master**

The `master` branch should always have an up-to-date `README.md` which
describes how to build it for the target platforms.

### **devel**

The `devel` branch is used to test integration of features, breakers, and
bugfixes before merging into `master`.

### **feature-***

These branches are *guaranteed* to not break any APIs or behaviors. If you
can't guarantee that, then you should create a `breaker-*` branch, detailed in
the next section.

* Adding functionality
* Marking functionality as "deprecated"
* Refactoring -- restructuring without changing public interfaces

### **fix-***

These branches shouldn't break APIs, but they may change behaviors if the
behaviors were previously incorrect. If these changes aren't
backwards-compatible, then they should be in a `breaker-*` branch.

### **breaker-***

These branches contain potentially-breaking changes which have any of the
following effects:

* Removing functionality
* Changing the behavior of an algorithm 
* Changing the behavior of an APIa
* Changing configuration properties

### **hack-***

These branches are expected to be made when prototyping or pushing towards a
deadline. They're useful for archiving variants for experimental code which may
or may not eventually need to be merged into `master`.

## Git Tagging Model

Git tags are used to track breaking changes in a repository via [semantic
versioning](http://semver.org/) where each tag is a 3-element version number
`MAJOR.MINOR.PATCH`. These elements are defined as follows:

- `MAJOR` version when you make incompatible API changes,
- `MINOR` version when you add functionality in a backwards-compatible manner, and
- `PATCH` version when you make backwards-compatible bug fixes.

This enables someone to determine when they can easily update to a newer
`master` without having to change their dependent code.

## Merging into `master` and Tagging Semantic Versions

Occasionally, as features and bugfixes are incorporated `devel` will be merged
into `master` and `master` will be tagged with a new version. This is the
closest action in the LCSR Development model to a release.

```
git checkout master
git merge devel
git push origin master
```

To determine whether to increment `MAJOR`, `MINOR`, or `PATCH`, you need to review the changes since the last tag. 

> **NOTE:** Due to limited resources, it might not be possible to guarantee
> whether backwards-incompatible changes have been introduced. In this case, it
> is better to be conservative, and to increment the `MAJOR` version element.

In order to list all of the commits since the last tag, you can run the
following after merging locally:

```
git log `git describe --tags --abbrev=0`..HEAD --oneline --decorate --color
```

Merging of `fix-*` branches necessitates incrementing the `PATCH` version,
merging of `feature-*` branches necessitates incrementing the `MINOR` version,
and merging of `breaker-*` branches necessitates incrementing the `MAJOR`
version.

```
git tag X.Y.Z
git push orign --tags
```

## Continuous Integration Testing

TBD

## Maintainer Responsibilities

TBD

## Code Reviews

TBD
