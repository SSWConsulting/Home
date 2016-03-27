Engineering guidelines
======================

* [Basics](#basics)
* [Source code management](#source-code-management)
* [Coding guidelines](#coding-guidelines)
* [Product planning and issue tracking](#product-planning-and-issue-tracking)
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


### Unit tests

We use xUnit.net for all unit testing.


### Repo-specific Samples

Some repos will have their own sample projects that are used for testing purposes and experimentation. Please ensure that these go in a `samples/` sub-folder in the repo.

To have a sample project reference a project in `src` you'll need a `global.json` file in the root of your repo. By default project-to-project references must be sibling folders. Using a `global.json` file allows a solution to specify non-standard locations to locate references. The format of `global.json` is as follows:

```json
{
    "projects": ["src"]
}
```


## Coding guidelines

:grey_exclamation: The *content* of the code that we write.


### Coding style guidelines – general

The most general guideline is that we use all the VS default settings in terms of code formatting, except that we put `System` namespaces before other namespaces (this used to be the default in VS, but it changed in a more recent version of VS).

1. Use four spaces of indentation (no tabs)
2. Use `_camelCase` for private fields
3. Avoid `this.` unless absolutely necessary
4. Always specify member visiblity, even if it's the default (i.e. `private string _foo;` not `string _foo;`)


### Usage of the var keyword

The `var` keyword is to be used as much as the compiler will allow. For example, these are correct:

```c#
var fruit = "Lychee";
var fruits = new List<Fruit>();
var flavor = fruit.GetFlavor();
string fruit = null; // can't use "var" because the type isn't known (though you could do (string)null, don't!)
const string expectedName = "name"; // can't use "var" with const
```

The following are incorrect:

```c#
string fruit = "Lychee";
List<Fruit> fruits = new List<Fruit>();
FruitFlavor flavor = fruit.GetFlavor();
```


### Use C# type keywords in favor of .NET type names

When using a type that has a C# keyword the keyword is used in favor of the .NET type name. For example, these are correct:

```c#
public string TrimString(string s) {
    return string.IsNullOrEmpty(s)
        ? null
        : s.Trim();
}

var intTypeName = nameof(Int32); // can't use C# type keywords with nameof
```

The following are incorrect:

```c#
public String TrimString(String s) {
    return String.IsNullOrEmpty(s)
        ? null
        : s.Trim();
}
```

### Cross-platform coding

Our frameworks should work on CoreCLR, which supports multiple operating systems. Don't assume we only run (and develop) on Windows. Code should be sensitive to the differences between OS's. Here are some specifics to consider.

#### Line breaks
Windows uses `\r\n`, OS X and Linux uses `\n`. When it is important, use `Environment.NewLine` instead of hard-coding the line break.

Note: this may not always be possible or necessary. 

Be aware that these line-endings may cause problems in code when using `@""` text blocks with line breaks.

#### Environment Variables

OS's use different variable names to represent similar settings. Code should consider these differences.

For example, when looking for the user's home directory, on Windows the variable is `USERPROFILE` but on most Linux systems it is `HOME`.

```c#
var homeDir = Environment.GetEnvironmentVariable("USERPROFILE") 
                  ?? Environment.GetEnvironmentVariable("HOME");
```

#### File path separators

Windows uses `\` and OS X and Linux use `/` to separate directories. Instead of hard-coding either type of slash, use [`Path.Combine()`](https://msdn.microsoft.com/en-us/library/system.io.path.combine(v=vs.110).aspx) or [`Path.DirectorySeparatorChar`](https://msdn.microsoft.com/en-us/library/system.io.path.directoryseparatorchar(v=vs.110).aspx).

If this is not possible (such as in scripting), use a forward slash. Windows is more forgiving than Linux in this regard.

### When to use internals vs. public and when to use InternalsVisibleTo

As a modern set of frameworks, usage of internal types and members is allowed, but discouraged.

`InternalsVisibleTo` is used only to allow a unit test to test internal types and members of its runtime assembly. We do not use `InternalsVisibleTo` between two runtime assemblies.

If two runtime assemblies need to share common helpers then we will use a "shared source" solution with build-time only packages. Check out the https://github.com/aspnet/Mvc/tree/dev/src/Microsoft.AspNet.Mvc.Common project and how it is referenced from the `project.json` files of sibling projects.

If two runtime assemblies need to call each other's APIs, the APIs must be public. If we need it, it is likely that our customers need it.


### Argument null checking

**Note:** The `[NotNull]` feature is not being implemented as a compile-time feature. To throw a runtime exception, add an explicit null check and throw an `ArgumentNullException`. Some repos still use the `[NotNull]` attribute, but it is for static analysis only and does not affect runtime behavior.

Null checking is required for parameters that cannot be null (big surprise!). To add null checking to your code, declare this attribute in your assembly in any namespace (use the JetBrains namespace to have ReSharper work):

```c#
using System;

namespace JetBrains.Annotations
{
    [AttributeUsage(
        AttributeTargets.Method | AttributeTargets.Parameter |
        AttributeTargets.Property | AttributeTargets.Delegate |
        AttributeTargets.Field, AllowMultiple = false, Inherited = true)]
    internal sealed class NotNullAttribute : Attribute
    {
    }
}
```

And then annotate parameters of methods or property setters:

```c#
public void GetBanana([NotNull] string variety)
{
    // do not do explicit null check in the method body!
    ...
}

public string Variety
{
    get;
    [param: NotNull]
    set;
}
```

The null checking code will be code-gen'ed at compile time into the method body.


#### Argument null checking in interface member definitions and abstract/virtual methods

If an interface member or abstract/virtual member contractually disallows nulls in its parameters, annotate them with `[NotNull]`.
The implementing method does *not* need the annotations - the code-gen will automatically emit the null checking code on the implementing method.


#### Argument null checking in chained constructors/methods

Null checks should be used on any public entry point where null is not allowed. This ensures the contract of not-nullable is seen by all callers.

```c#
public class Banana
{
    // Even though all this ctor does is chain the next ctor, it still must have NotNull annotations
    public Banana([NotNull] string name, [NotNull] string variety)
        : this(name, variety, string.Empty)
    {
    }

    public Banana([NotNull] string name, [NotNull] string variety, [NotNull] string color)
    {
        ...
    }
}
```


### Async method patterns

By default all async methods must have the `Async` suffix. There are some exceptional circumstances where a method name from a previous framework will be grandfathered in.

Passing cancellation tokens is done with an optional parameter with a value of `default(CancellationToken)`, which is equivalent to `CancellationToken.None` (one of the few places that we use optional parameters). The main exception to this is in web scenarios where there is already an `HttpContext` being passed around, in which case the context has its own cancellation token that can be used when needed.

Sample async method:

```c#
public Task GetDataAsync(
    QueryParams query,
    int maxData,
    CancellationToken cancellationToken = default(CancellationToken))
{
    ...
}
```


### Extension method patterns

The general rule is: if a regular static method would suffice, avoid extension methods.

Extension methods are often useful to create chainable method calls, for example, when constructing complex objects, or creating queries.

Internal extension methods are allowed, but bear in mind the previous guideline: ask yourself if an extension method is truly the most appropriate pattern.

The namespace of the extension method class should generally be the namespace that represents the functionality of the extension method, as opposed to the namespace of the target type. One common exception to this is that the namespace for middleware extension methods is normally always the same is the namespace of `IAppBuilder`.

The class name of an extension method container (also known as a "sponsor type") should generally follow the pattern of `<Feature>Extensions`, `<Target><Feature>Extensions`, or `<Feature><Target>Extensions`. For example:

```c#
namespace Food {
    class Fruit { ... }
}
namespace Fruit.Eating {
    class FruitExtensions { public static void Eat(this Fruit fruit); }
  OR
    class FruitEatingExtensions { public static void Eat(this Fruit fruit); }
  OR
    class EatingFruitExtensions { public static void Eat(this Fruit fruit); }
}
```

When writing extension methods for an interface the sponsor type name must not start with an `I`.


### Doc comments

The person writing the code will write the doc comments. Public APIs only. No need for doc comments on non-public types.

Note: Public means callable by a customer, so it includes protected APIs. However, some public APIs might still be "for internal use only" but need to be public for technical reasons. We will still have doc comments for these APIs but they will be documented as appropriate.


### Assertions

Use `Debug.Assert()` to assert a condition in the code. Do not use Code Contracts (e.g. `Contract.Assert`).

Please note that assertions are only for our own internal debugging purposes. They do not end up in the released code, so to alert a developer of a condition use an exception.


### Unit tests and functional tests


#### Assembly naming

The unit tests for the `Microsoft.Fruit` assembly live in the `Microsoft.Fruit.Tests` assembly.

The functional tests for the `Microsoft.Fruit` assembly live in the `Microsoft.Fruit.FunctionalTests` assembly.

In general there should be exactly one unit test assembly for each product runtime assembly. In general there should be one functional test assembly per repo. Exceptions can be made for both.

TODO: Should we update the SSW Rule to reflect the ASP.NET team guildelines ?


#### Unit test class naming

Test class names end with `Test` and live in the same namespace as the class being tested. For example, the unit tests for the `Microsoft.Fruit.Banana` class would be in a `Microsoft.Fruit.BananaTest` class in the test assembly.


#### Unit test method naming

Unit test method names must be descriptive about *what is being tested*, *under what conditions*, and *what the expectations are*. Pascal casing and underscores can be used to improve readability. The following test names are correct:

```
PublicApiArgumentsShouldHaveNotNullAnnotation
Public_api_arguments_should_have_not_null_annotation
```

The following test names are incorrect:

```
Test1
Constructor
FormatString
GetData
```


#### Unit test structure

The contents of every unit test should be split into three distinct stages, optionally separated by these comments:

```c#
// Arrange  
// Act  
// Assert 
```

The crucial thing here is that the `Act` stage is exactly one statement. That one statement is nothing more than a call to the one method that you are trying to test. Keeping that one statement as simple as possible is also very important. For example, this is not ideal:

```c#
int result = myObj.CallSomeMethod(GetComplexParam1(), GetComplexParam2(), GetComplexParam3());
```

This style is not recommended because way too many things can go wrong in this one statement. All the `GetComplexParamN()` calls can throw for a variety of reasons unrelated to the test itself. It is thus unclear to someone running into a problem why the failure occurred.

The ideal pattern is to move the complex parameter building into the `Arrange` section:

```c#
// Arrange
P1 p1 = GetComplexParam1();
P2 p2 = GetComplexParam2();
P3 p3 = GetComplexParam3();

// Act
int result = myObj.CallSomeMethod(p1, p2, p3);

// Assert
Assert.AreEqual(1234, result);
```

Now the only reason the line with `CallSomeMethod()` can fail is if the method itself blew up. This is especially important when you're using helpers such as `ExceptionHelper`, where the delegate you pass into it must fail for exactly one reason.


### Testing exception messages

In general testing the specific exception message in a unit test is important. This ensures that the exact desired exception is what is being tested rather than a different exception of the same type. In order to verify the exact exception it is important to verify the message.

To make writing unit tests easier it is recommended to compare the error message to the RESX resource. However, comparing against a string literal is also permitted.

```c#
var ex = Assert.Throws<InvalidOperationException>(
    () => fruitBasket.GetBananaById(1234));
Assert.Equal(
    Strings.FormatInvalidBananaID(1234),
    ex.Message);
```


#### Use xUnit.net's plethora of built-in assertions

xUnit.net includes many kinds of assertions – please use the most appropriate one for your test. This will make the tests a lot more readable and also allow the test runner report the best possible errors (whether it's local or the CI machine). For example, these are bad:

```c#
Assert.Equal(true, someBool);

Assert.True("abc123" == someString);

Assert.True(list1.Length == list2.Length);

for (int i = 0; i < list1.Length; i++) {
    Assert.True(
        String.Equals
            list1[i],
            list2[i],
            StringComparison.OrdinalIgnoreCase));
}
```

These are good:

```c#
Assert.True(someBool);

Assert.Equal("abc123", someString);

// built-in collection assertions!
Assert.Equal(list1, list2, StringComparer.OrdinalIgnoreCase);
```


#### Parallel tests

By default all unit test assemblies should run in parallel mode, which is the default. Unit tests shouldn't depend on any shared state, and so should generally be runnable in parallel. If the tests fail in parallel, the first thing to do is to figure out *why*; do not just disable parallel tests!

For functional tests it is reasonable to disable parallel tests.


### Use only complete words or common/standard abbreviations in public APIs

Public namespaces, type names, member names, and parameter names must use complete words or common/standard abbreviations.

These are correct:
```c#
public void AddReference(AssemblyReference reference);
public EcmaScriptObject SomeObject { get; }
```

These are incorrect:
```c#
public void AddRef(AssemblyReference ref);
public EcmaScriptObject SomeObj { get; }
```


## Product planning and issue tracking

:grey_exclamation: How we track what work there is to do.


### Issue tracking

Bug management takes place in GitHub. Each repo has its own issue tracker. Bugs cannot be moved between repos so make sure you open a bug in the right repo. If a bug is opened in the wrong repo someone will have to manually copy it to the correct repo.

We use the HuBoard pattern for issue tags. Look at the numerical tags that SignalR uses for an idea: https://github.com/SignalR/SignalR/issues


## Tips and tricks

:grey_exclamation: The *structure* of the code that we write and the *tools* that we use to write the code.


### GitHub Flavored Markdown

GitHub supports Markdown in many places throughout the system (issues, comments, etc.). However, there are a few differences from regular Markdown that are described here:

	https://help.github.com/articles/github-flavored-markdown


### Including people in a GitHub discussion

To include another team member in a discussion on GitHub you can use an `@ mention` to cause a notification to be sent to that person. This will automatically send a notification email to that person (assuming they have not altered their GitHub account settings). For example, in a PR's discussion thread or in an issue tracker comment you can type `@username` to have them receive a notification. This is useful when you want to "include" someone in a code review in a PR, or if you want to get another opinion on an issue in the issue tracker.


