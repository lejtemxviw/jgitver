# jgitver: git versioning library [![Build Status](https://travis-ci.org/jgitver/jgitver.svg)](https://travis-ci.org/jgitver/jgitver)

The goal of `jgitver` is to provide a common way, via a library, to calculate a project [semver](http://semver.org) compatible version from a git repository and the tags it contains.
By doing so, it will then be _easy_ to integrate it into build systems like maven, gradle or ant.

## How it works

`jgitver` uses annotated tags, lightweight tags & branches names to deduce the version of a particular git commit. 
From a given commit, a little bit like `git describe` command, `jgitver` walks thru the commit tree to retrieve the latest tag on an ancestor commit. From this tag and depending on the configuration a version will be deducted/calculated.

## Simplicity & power

`jgitver` comes with default modes that follow best practices & conventions or you can configure it to provide much more.

### versions, identifier & qualifiers

When computing versions, `jgitver` focuses on providing [semver](http://semver.org) compatible versions.

- version: as defined by [semver](http://semver.org), a serie of X.Y.Z where X, Y & Z are non-negative integers
- identifier: a textual information following the version, build from alphanumeric characters & hyphen
- qualifiers: qualifiers are textual information that can be combined to build a [semver](http://semver.org) identifier 

## Versions

### no calculation for commits with annotated tag

When the HEAD is on a git commit which contains an annotated tag that matches a version definition, this annotated tag is used to extract the version.

### use annotated tags version by default

Without any configuration, `jgitver` will retrieve the ancestor commit (starting from HEAD) that has an annotated tag on it, and will use this a a base version name.
It will append to that found version, any distance or branch name where needed.
If no tag with a version can be found ``0.0.0` will be used.     

### auto increasing version

If asked, `jgitver` will do a `+1` to the version found on the latest annotated tag found starting from the current commit.

To activate, call method `GitVersionCalculator#setAutoIncrementPatch(true)`

### lightweight tags

By looking for a version on an annotated tag, `jgitver` will also lookup lightweight tags. If one defining a version is found on a parent commit, it will be used.
Lightweight tags have the precedence on annotated tags, so that starting from the exact same commit as the one defining the previous release, you can defined the next version pattern to use and thus _override_ the default mechanism or the `+1` one.

## Identifier & Qualifiers

`jgitver` will also generate qualifiers and thus an identifier using:

- existing qualifiers on found tag (including SNAPSHOT)
- the tag distance from the HEAD if > 0 (active by default)
- the branch name if not omit by a call to `GitVersionCalculator#setNonQualifierBranches( ... )` ; by default only `master` branch is omit for branch qualification (active by default)
- the git commit id (inactive by default)

## Examples

Here is a serie of different settings supported by `jgitver`. Please look also at the junit tests cases that are aimed to cover all possibilities. 

### Example 1

![Example 1](src/doc/images/s1_linear_with_only_annotated_tags.gif?raw=true "Example 1")

### Example 2

![Example 2](src/doc/images/s1_linear_with_only_annotated_tags_autoIncrement.gif?raw=true "Example 2")

### Example 3

![Example 3](src/doc/images/s2_linear_with_both_tags_autoIncrement.gif?raw=true "Example 3")

### Example 4

![Example 4](src/doc/images/s7_linear_with_SNAPSHOT_tags_and_branch.gif?raw=true "Example 4")
