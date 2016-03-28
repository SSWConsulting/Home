Engineering guidelines - Coding Standards
======================

:grey_exclamation: The *content* of the code that we write.


## Coding style guidelines – general

The most general guideline is that we use all the VS default settings in terms of code formatting, except that we put `System` namespaces before other namespaces (this used to be the default in VS, but it changed in a more recent version of VS).

1. Use four spaces of indentation (no tabs)
2. Use `_camelCase` for private fields
3. Avoid `this.` unless absolutely necessary
4. Always specify member visiblity, even if it's the default (i.e. `private string _foo;` not `string _foo;`)


## Usage of the var keyword

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


## Use C# type keywords in favor of .NET type names

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

## Cross-platform coding

Our frameworks should work on CoreCLR, which supports multiple operating systems. Don't assume we only run (and develop) on Windows. Code should be sensitive to the differences between OS's. Here are some specifics to consider.

### Line breaks
Windows uses `\r\n`, OS X and Linux uses `\n`. When it is important, use `Environment.NewLine` instead of hard-coding the line break.

Note: this may not always be possible or necessary.

Be aware that these line-endings may cause problems in code when using `@""` text blocks with line breaks.

### Environment Variables

OS's use different variable names to represent similar settings. Code should consider these differences.

For example, when looking for the user's home directory, on Windows the variable is `USERPROFILE` but on most Linux systems it is `HOME`.

```c#
var homeDir = Environment.GetEnvironmentVariable("USERPROFILE")
                  ?? Environment.GetEnvironmentVariable("HOME");
```

### File path separators

Windows uses `\` and OS X and Linux use `/` to separate directories. Instead of hard-coding either type of slash, use [`Path.Combine()`](https://msdn.microsoft.com/en-us/library/system.io.path.combine(v=vs.110).aspx) or [`Path.DirectorySeparatorChar`](https://msdn.microsoft.com/en-us/library/system.io.path.directoryseparatorchar(v=vs.110).aspx).

If this is not possible (such as in scripting), use a forward slash. Windows is more forgiving than Linux in this regard.

## When to use internals vs. public and when to use InternalsVisibleTo

As a modern set of frameworks, usage of internal types and members is allowed, but discouraged.

`InternalsVisibleTo` is used only to allow a unit test to test internal types and members of its runtime assembly. We do not use `InternalsVisibleTo` between two runtime assemblies.

If two runtime assemblies need to share common helpers then we will use a "shared source" solution with build-time only packages. Check out the https://github.com/aspnet/Mvc/tree/dev/src/Microsoft.AspNet.Mvc.Common project and how it is referenced from the `project.json` files of sibling projects.

If two runtime assemblies need to call each other's APIs, the APIs must be public. If we need it, it is likely that our customers need it.


## Argument null checking

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


### Argument null checking in interface member definitions and abstract/virtual methods

If an interface member or abstract/virtual member contractually disallows nulls in its parameters, annotate them with `[NotNull]`.
The implementing method does *not* need the annotations - the code-gen will automatically emit the null checking code on the implementing method.


### Argument null checking in chained constructors/methods

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


## Async method patterns

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


## Extension method patterns

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


## Doc comments

The person writing the code will write the doc comments. Public APIs only. No need for doc comments on non-public types.

Note: Public means callable by a customer, so it includes protected APIs. However, some public APIs might still be "for internal use only" but need to be public for technical reasons. We will still have doc comments for these APIs but they will be documented as appropriate.


## Assertions

Use `Debug.Assert()` to assert a condition in the code. Do not use Code Contracts (e.g. `Contract.Assert`).

Please note that assertions are only for our own internal debugging purposes. They do not end up in the released code, so to alert a developer of a condition use an exception.



## Use only complete words or common/standard abbreviations in public APIs

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
