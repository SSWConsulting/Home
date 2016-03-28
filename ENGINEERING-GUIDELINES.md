Engineering guidelines
======================

* [Basics](#basics)
* [Source code management](#source-code-management)
* [Product planning and issue tracking](#product-planning-and-issue-tracking)
* [Coding guidelines](#coding-guidelines)
* [Testing](#testing)
* [Tips and tricks](#tips-and-tricks)


## Basics

### Copyright header and license notice

All source code files (mostly `src/**/*.cs` and `test/**/*.cs`) require this exact header (please do not make any changes to it):

```c#
// Copyright (c) SSW. All rights reserved.
// Licensed under the Apache License, Version 2.0. See License.txt in the project root for license information.
```

It is ok to skip it on generated files, such as `*.designer.cs`.

Every repo also needs the Apache 2.0 License in a file called LICENSE.txt in the root of the repo. Please use only identical copies to what we have in other repos.

### External dependencies

This refers to dependencies on projects (i.e. NuGet packages) outside of the `SSW` repo or outside of Microsoft.
Adding an external dependency requires approval from @ericphan or @adamstephensen.

The current approved dependencies are:
* Autofaq
* SeriLog
* Seq
* Json.NET


Dependencies that are used only in test projects and build tools are not nearly as rigid, but still require approval from @ericphan or @adamstephensen.

### Customer samples

Customer-facing sample projects will have their own repos.
For example, the "MVC Music Store" sample has its own repo with both the original MVC Music Store, and a copy of it that has been modified to use the new features we are building.
For repo-specific samples please see the Repo-specific Samples section.


### Code reviews and checkins

To help ensure that only the highest quality code makes its way into the project, please submit all your code changes to GitHub as PRs. This includes runtime code changes, unit test updates, and updates to official samples (e.g. Music Store). For example, sending a PR for just an update to a unit test might seem like a waste of time but the unit tests are just as important as the product code and as such, reviewing changes to them is also just as important.

The advantages are numerous: improving code quality, more visibility on changes and their potential impact, avoiding duplication of effort, and creating general awareness of progress being made in various areas.

In general a PR should be signed off (using the :shipit: `:shipit:` emoticon) by the Subject Matter Expert (SME) of that code. For example, a change to the Banana project should be signed off by `@MrMonkey`, and not by `@MrsGiraffe`. If you don't know the SME, please talk to one of the engineering leads and they will be happy to help you identify the SME. Of course, sometimes it's the SME who is making a change, in which case a secondary person will have to sign off on the change (e.g. `@JuniorMonkey`).

To commit the PR to the repo **do not use the Big Green Button**. Instead, do a typical push that you would use with Git (e.g. local pull, rebase, merge, push).


TODO: Rule - 'Do You Know How To Submit Pull Requests'


## Source code management

:grey_exclamation: The *structure* of the code that we write and the *tools* that we use to write the code.


### Repos

To create a new repo in the https://github.com/sswconsulting/ org, contact @ericphan or @adamstephensen.


### Branch strategy

TODO: To be updated to reflect GitHub flow

In general:

* `master` has the code for the latest release on NuGet.org (e.g. alpha, beta, RC, RTM)
* `dev` has the code that is being worked on but not yet released. This is the branch into which devs normally submit pull requests and merge changes into.
* `release` has the code that is being staged and stabilized for an upcoming release

Shortly before a release, the `release` branches are created from `dev`, and stabilization work happens there. Post-release work continus in the `dev` branch. Once the release takes place, the code is pushed from `release` to `master`.


### Solution and project folder structure and naming

Solution files go in the repo root.

Solution names match repo names (e.g. Mvc.sln in the Mvc repo).

Every project also needs a `project.json` and a matching `.xproj` file. This `project.json` is the source of truth for a project's dependencies and configuration options.

Solutions need to contain solution folders that match the physical folders (`src`, `test`, etc.).

For example, in the `Fruit` repo with the `Banana` and `Lychee` projects you would have these files checked in:

```
/Fruit.sln
/src
/src/Microsoft.AspNet.Banana
/src/Microsoft.AspNet.Banana/project.json
/src/Microsoft.AspNet.Banana/Banana.kproj
/src/Microsoft.AspNet.Banana/Banana.cs
/src/Microsoft.AspNet.Banana/Util/BananaUtil.cs
/src/Microsoft.AspNet.Lychee
/src/Microsoft.AspNet.Lychee/project.json
/src/Microsoft.AspNet.Lychee/Lychee.kproj
/src/Microsoft.AspNet.Lychee/Lychee.cs
/src/Microsoft.AspNet.Lychee/Util/LycheeUtil.cs
/test
/test/Microsoft.AspNet.Banana.Tests
/test/Microsoft.AspNet.Banana.Tests/project.json
/test/Microsoft.AspNet.Banana.Tests/BananaTest.kproj
/test/Microsoft.AspNet.Banana.Tests/BananaTest.cs
/test/Microsoft.AspNet.Banana.Tests/Util/BananaUtilTest.cs
```




### Conditional compilation for Desktop/CoreCLR

Almost all development is done for both CoreCLR and Desktop .NET. Some code will be CoreCLR-specific or Desktop-specific because of API changes or behavior differences. The build system has two conditional compilation statements to assist with this:

Desktop:

```c#
#ifdef DNX451
```

CoreCLR:

```c#
#ifdef DNXCORE50
```


### Assembly naming pattern

The general naming pattern is `SSW.<project>.<area>.<subarea>`.

This is as per [SSW Rules to Better Architecture - Do you review the solution and project names](https://rules.ssw.com.au/do-you-review-the-solution-and-project-names)





### Repo-specific Samples

Some repos will have their own sample projects that are used for testing purposes and experimentation. Please ensure that these go in a `samples/` sub-folder in the repo.

To have a sample project reference a project in `src` you'll need a `global.json` file in the root of your repo. By default project-to-project references must be sibling folders. Using a `global.json` file allows a solution to specify non-standard locations to locate references. The format of `global.json` is as follows:

```json
{
    "projects": ["src"]
}
```


## Product planning and issue tracking

:grey_exclamation: How we track what work there is to do.


### Issue tracking

Bug management takes place in GitHub. Each repo has its own issue tracker. Bugs cannot be moved between repos so make sure you open a bug in the right repo. If a bug is opened in the wrong repo someone will have to manually copy it to the correct repo.

We use the HuBoard pattern for issue tags. Look at the numerical tags that SignalR uses for an idea: https://github.com/SignalR/SignalR/issues

## Coding guidelines

Details for coding guidelines can be found in the [Coding Standards](ENGINEERING-GUIDELINES-CODING-STANDARDS.md)


## Testing

Details for testing guidelines can be found in the [Testing Engineering Guidelines](ENGINEERING-GUIDELINES-TESTING.md)

## Tips and tricks

:grey_exclamation: The *structure* of the code that we write and the *tools* that we use to write the code.


### GitHub Flavored Markdown

GitHub supports Markdown in many places throughout the system (issues, comments, etc.). However, there are a few differences from regular Markdown that are described here:

	https://help.github.com/articles/github-flavored-markdown


### Including people in a GitHub discussion

To include another team member in a discussion on GitHub you can use an `@ mention` to cause a notification to be sent to that person. This will automatically send a notification email to that person (assuming they have not altered their GitHub account settings). For example, in a PR's discussion thread or in an issue tracker comment you can type `@username` to have them receive a notification. This is useful when you want to "include" someone in a code review in a PR, or if you want to get another opinion on an issue in the issue tracker.
