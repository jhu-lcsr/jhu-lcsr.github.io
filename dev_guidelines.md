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

The branching model used by LCSR projects based on a simplified version
of the [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/)
development model. All repositories hosted on LCSR GitHub organizations must
conform to this branching model.

Similarly to *Git Flow*, we use following standard branches and branch types
(described in more detail in the subsequent sections):

- **master** -- The main "operational" branch, which should never be broken.
- **devel** -- The "volatile" branch where the latest features and bugfixes get integrated for multiple users to test. 
- **feature-xxx** -- Branches for features pending merge into `devel`
- **hack-xxx** -- Branches for variants of functionality which are done quickly for prototyping purposes.

Due to limited resources, we do not maintain release branches. As such,
bugfixes do not get pushed back into older versions of the code, they are only
merged into `devel` before being merged into `master`.

### **master**

The `master` branch should always have an up-to-date `README.md` which
describes how to build it for the target platforms. This file should also
give a high-level overview of the design of the package.

This branch should be usable by people who expect the package to "just work"
as described.

### **devel**

The `devel` branch is used to test integration of features and
bugfixes before merging into `master`. This branch should only be used
by people who are ready to debug problems which may arise and who aren't worried
about having to re-test their code.

### **feature-xxx**

Features are either backwards-compatible or non-backwards compatible. When making
a pull request, it should be determined whether a feature will break anything
as described below.

Backwards-compatible features include changes such as:

* Adding functionality
* Marking functionality as "deprecated"
* Refactoring -- restructuring without changing public interfaces

A non-backwards-compatible feature is any change which has any of the following
effects:

* Removing functionality
* Changing the behavior of an algorithm
* Changing the behavior of an APIa
* Changing configuration properties

### **hack-xxx**

These branches are expected to be made when prototyping or pushing towards a
deadline. They're useful for archiving variants for experimental code which are
not intended to eventually be merged into `master`.

## Version Tagging Model

Git tags are used to track breaking changes in a repository via a three-number
versioning system based on [semantic versioning](http://semver.org/). In
semantic versioning, each tag is a 3-element version number
`MAJOR.MINOR.PATCH`. These elements are defined as follows:

- `MAJOR` version when you make incompatible API changes,
- `MINOR` version when you add functionality in a backwards-compatible manner, and
- `PATCH` version when you make backwards-compatible bug fixes.

Since we expect that research code developed by JHU LCSR will be more volatile
than that which is developed for end-users, we will use a different semantic
versioning system, called *proto versioning*. In this system, we also use a
3-element version number, but it corresponds to the following `MORTAL.MAJOR.MINOR`:

- `MORTAL` version when **everything** is changed, restructured, or otherwise broken
- `MAJOR` version when you make incompatible API changes
- `MINOR` version when you add functionality or fix bugs in a backwards-compatible manner

## User Pattern

For repositories which you don't contribute to, you should either use the
`master` branch, or always use a tagged version. This will allow you to rely on
the code not changing.

## Contributer Pattern

Generally, if you're not responsible for maintaining a repository, you should develop
in your own *fork* of the repository. This will allow you to have the freedom to store
and work on your own experiments without breaking someone else's code.

On either GitHub or GitLab, you can fork a repository into your own account, and once
you want to contribute back, you can do so with a pull request into the original
repository's `devel` branch.

If your code is out of date, you can merge or rebase changes from the original repository
by adding it as a remote to your local clone:

```
git checkout feature-my-cool-thing
git remote add upstream https://github.com/jhu-lcsr/repo-name.git
git fetch upstream
git merge upstream devel
```

## Maintainer Pattern

As the maintainer of a repository, you're responsible for deciding when to
merge changes from the `devel` branch into the `master` branch and tagging
versions according to the versioning scheme described above.

For new repositories or repositories which aren't intended to be used by more
than one person, you can avoid maintaining separate `devel` and `master` branches.
In order to make it clear that your repository is volatile, you should delete
the `master` branch, and do all work directly in `devel`.

For repositories where development has already been happening directly in
`master`, you can switch this with the following:

```
git checkout master               # make sure you're on "master"
git pull origin master            # make sure you have the latest version of "master"
git checkout -b devel             # create a new branch called "devel" identical to "master"
git push -u origin devel          # push the new "devel" branch and set "origin" as it's upstream
git branch -D master              # delete the local "master" branch
git push origin --delete master   # delete the remote "master" branch
```

### Merging into `master` and Tagging Versions

Occasionally, as features and bugfixes are incorporated `devel` will be merged
into `master` and `master` will be tagged with a new version. This is the
closest action in the LCSR Development model to a release.

```
git checkout master
git merge devel
git push origin master
```

To determine whether to increment `MORTAL`, `MAJOR` or `MINOR`, you need to
review the changes since the last tag.

> **NOTE:** Due to limited resources, it might not be possible to guarantee
> whether backwards-incompatible changes have been introduced. In this case, it
> is better to be conservative, and to increment the `MAJOR` version element.

In order to list all of the commits since the last tag, you can run the
following after merging locally:

```
git log `git describe --tags --abbrev=0`..HEAD --oneline --decorate --color
```

Merging of  `feature-xxx` branches which are backwards-compatible necessitate
incrementing the `MINOR` version, and merging of non-backwards compatible
`feature-xxx` branches necessitate incrementing the `MAJOR` version, and
re-setting the `MINOR` version to `0`.


If it's clear that there has been a complete redesign of a package, it's
necessary to increment the `MORTAL` version and re-set the `MAJOR` and `MINOR`
versions to `0`.

```
git tag $MORTAL.$MAJOR.$MINOR
git push orign --tags
```

## Continuous Integration Testing

Continuous integration testing can be used to guarantee that a repository
builds and passes any available tests. A simple way to do this which works well
with GitHub is with [Travis](http://www.travis-ci.org).

## Code Standards

The most important thing is to be consistent within a given project or
repository.

## Code Reviews

Code reviews should be performed for all pull requests.

