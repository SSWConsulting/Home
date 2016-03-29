# Engineering guidelines

* [Basics](#basics)
* [Source code management](#source-code-management)
* [Product planning and issue tracking](#product-planning-and-issue-tracking)
* [Coding guidelines](#coding-guidelines)
* [Testing](#testing)



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




## Code reviews and checkins




## Source code management

:grey_exclamation: The *structure* of the code that we write and the *tools* that we use to write the code.


### Repos

To create a new repo in the https://github.com/sswconsulting/ org, contact @ericphan or @adamstephensen.


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

## Product planning and issue tracking

Details for coding guidelines can be found in the [Contributing Guide](CONTRIBUTING.md)


### Branch strategy, Code Review & Checkins

Details for Branch strategy, Code Review & Checkins can be found in the [Contributing Guide](CONTRIBUTING.md)


## Coding guidelines

Details for coding guidelines can be found in the [Coding Standards](ENGINEERING-GUIDELINES-CODING-STANDARDS.md)


## Testing

Details for testing guidelines can be found in the [Testing Engineering Guidelines](ENGINEERING-GUIDELINES-TESTING.md)

## Sample Applications

### Customer samples

Customer-facing sample projects will have their own repos.

For example, the ["Angular 2.0 Music Store"](https://github.com/sswconsulting/angularmusicstore) sample has its own repo.

### Repo-specific Samples

Some repos will have their own sample projects that are used for testing purposes and experimentation. e.g. [SSW Data Onion](https://github.com/SSWConsulting/SSW.DataOnion)
Please ensure that these go in a `samples/` sub-folder in the repo.

For .Net Core projects - To have a sample project reference a project in `src` you'll need a `global.json` file in the root of your repo. By default project-to-project references must be sibling folders. Using a `global.json` file allows a solution to specify non-standard locations to locate references. The format of `global.json` is as follows:

```json
{
    "projects": ["src"]
}
```
