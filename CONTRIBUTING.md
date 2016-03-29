# Contributing

One of the easiest ways to contribute is to participate in discussions and discuss issues.

You can also contribute by submitting pull requests with code changes.

* [Bugs and feature requests](#bugs-and-feature-requests)
* [General feedback and discussions](#general-feedback-and-discussions)
* [Reporting security issues and bugs](#reporting security-issues and bugs)
* [Contributing code via a pull request](#contributing-code-via-a-pull-request)
* [Engineering guidelines](#engineering-guidelines)
* [GitHub Flavored Markdown](#github-flavored-markdown)

## Bugs and feature requests
For non-security related bugs please log a new issue in the appropriate GitHub repo. Here are some of the most common repos:

* [SSW Data Onion](https://github.com/SSWConsulting/SSW.DataOnion) - Data access layer for easy Onion Architecture implementation
* [SSW Data Onion 2](https://github.com/SSWConsulting/SSW.DataOnion2) - Data Onion + .NET Core support + Ambient Context implementation
* [Enterprise Angular 2 Music Store](https://github.com/SSWConsulting/enterprise-musicstore-ui-angular2) - The Enterprise Music Store Angular 2 sample demonstrates how we at SSW implement Angular 2 in our solutions.
* [Enterprise Music Store API](https://github.com/SSWConsulting/enterprise-musicstore-api-aspnet) - The MVC Music Store backend - built ASP.Net 5 and WebAPI

Or browse the full list of repos in the [SSWConsulting](https://github.com/SSWConsulting/) organization.

## General feedback and discussions
Please start a discussion on the Issue tracker for the relevant repo e.g. * [SSW Data Onion 2 Issues](https://github.com/SSWConsulting/SSW.DataOnion2/Issues) or  * [SSW Angular 2 Music Store Issues](https://github.com/SSWConsulting/enterprise-musicstore-ui-angular2/issues) .

## Reporting security issues and bugs
Security issues and bugs should be reported privately, via email, to SSW  sswdevelopers@ssw.com.au You should receive a response within 24 hours. If for some reason you do not, please follow up via email to ensure we received your original message.

## Issue tracking

Bug management takes place in GitHub. Each repo has its own issue tracker. Bugs cannot be moved between repos so make sure you open a bug in the right repo. If a bug is opened in the wrong repo someone will have to manually copy it to the correct repo.

We use the HuBoard pattern for issue tags. Look at the numerical tags that SignalR uses for an idea: https://github.com/SignalR/SignalR/issues


## Filing issues
When filing issues, we ask you to use the [bug filing templates](https://github.com/aspnet/Home/wiki/Functional-bug-template) (We use the one from the ASP.NET team).
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

If you've found and fixed a bug, or have a new feature you would like to add we would love you to chip in.

Here are a few tips
* To make sure your pull-request makes it into the project please ensure you carefully read the
[Engineering guidelines](ENGINEERING-GUIDELINES.md)
* We have a well defined workflow. Before you lodge a pull-request check out our guide to the [Contributing Process](CONTRIBUTING-PROCESS.md)
* Make sure you can build the code.
  (Even better, configure continuous deployment and provide a link to your branch deployed and working)
* If you don't know what a pull request is read this article: [Using pull requests](https://help.github.com/articles/using-pull-requests).
* Check out these two blogs posts on contributing code: [Open Source Contribution Etiquette](http://tirania.org/blog/archive/2010/Dec-31.html) by Miguel de Icaza and [Don't "Push" Your Pull Requests](https://www.igvita.com/2011/12/19/dont-push-your-pull-requests/) by Ilya Grigorik.
* Note that all code submissions will be rigorously reviewed and tested by the SSW team, and only those that meet an extremely high bar for both quality and design/roadmap appropriateness will be merged into the source.


## Engineering guidelines

The coding, style, and general engineering guidelines are published on the [Engineering guidelines](https://github.com/SSWConsulting/Home/blob/master/ENGINEERING-GUIDELINES.md) page.

## Contributing Process

We have a well defined workflow. Before you lodge a pull-request check out our guide to the [Contributing Process](CONTRIBUTING-PROCESS.md).

## GitHub Flavored Markdown

GitHub supports Markdown in many places throughout the system (issues, comments, etc.). However, there are a few differences from regular Markdown that are described here:
	https://help.github.com/articles/github-flavored-markdown
