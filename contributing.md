# How to contribute

One of the easiest ways to contribute is to participate in discussions and discuss issues. You can also contribute by submitting pull requests with code changes.


## Bugs and feature requests?
For non-security related bugs please log a new issue in the appropriate GitHub repo. Here are some of the most common repos:

* [SSW Data Onion](https://github.com/SSWConsulting/SSW.DataOnion) - Data access layer for easy Onion Architecture implementation
* [SSW Data Onion 2](https://github.com/SSWConsulting/SSW.DataOnion2) - Data Onion + .NET Core support + Ambient Context implementation
* [Enterprise Angular 2 Music Store](https://github.com/SSWConsulting/enterprise-musicstore-ui-angular2) - The Enterprise Music Store Angular 2 sample demonstrates how we at SSW implement Angular 2 in our solutions.
* [Enterprise Music Store API](https://github.com/SSWConsulting/enterprise-musicstore-api-aspnet) - The MVC Music Store backend - built ASP.Net 5 and WebAPI 

Or browse the full list of repos in the [aspnet](https://github.com/aspnet/) organization.

## General feedback and discussions?
Please start a discussion on the Issue tracker for the relevant repo e.g. * [SSW Data Onion 2 Issues](https://github.com/SSWConsulting/SSW.DataOnion2/Issues)   .

## Reporting security issues and bugs
Security issues and bugs should be reported privately, via email, to SSW  sswdevelopers@ssw.com.au You should receive a response within 24 hours. If for some reason you do not, please follow up via email to ensure we received your original message. 

## Filing issues
When filing issues, we ask you to use the [bug filing templates](https://github.com/aspnet/Home/wiki/Functional-bug-template) from the ASP.NET team.
The best way to get your bug fixed is to be as detailed as you can be about the problem.
Providing a minimal project with steps to reproduce the problem is ideal.
Here are questions you can answer before you file a bug to make sure you're not missing any important information.

1. Did you read the documentation for the project?
2. Did you include the snippet of broken code in the issue?
3. What are the *EXACT* steps to reproduce this problem?
4. What package versions are you using (you can see these in the `project.json` file)?
5. What operating system are you using?
6. What version of IIS are you using?

GitHub supports [markdown](https://help.github.com/articles/github-flavored-markdown/), so when filing bugs make sure you check the formatting before clicking submit.


## Contributing code and content
Make sure you can build the code. Familiarize yourself with the project workflow and our coding conventions. If you don't know what a pull request is read this article: https://help.github.com/articles/using-pull-requests.

Before submitting a feature or substantial code contribution please discuss it with the team and ensure it follows the product roadmap. You might also read these two blogs posts on contributing code: [Open Source Contribution Etiquette](http://tirania.org/blog/archive/2010/Dec-31.html) by Miguel de Icaza and [Don't "Push" Your Pull Requests](https://www.igvita.com/2011/12/19/dont-push-your-pull-requests/) by Ilya Grigorik. Note that all code submissions will be rigorously reviewed and tested by the SSW team, and only those that meet an extremely high bar for both quality and design/roadmap appropriateness will be merged into the source.

Here's a few things you should always do when making changes to the code base:

**Engineering guidelines**

The coding, style, and general engineering guidelines are published on the [Engineering guidelines](https://github.com/sswconsulting/Home/wiki/Engineering-guidelines) page.

**Commit/Pull Request Format**

```
Summary of the changes (Less than 80 chars)
 - Detail 1
 - Detail 2

Addresses #bugnumber (in this specific format)
```

**Tests**

-  Tests need to be provided for every bug/feature that is completed.
-  Tests only need to be present for issues that need to be verified by QA (e.g. not tasks)
-  If there is a scenario that is far too hard to test there does not need to be a test for it.
  - "Too hard" is determined by the team as a whole.
