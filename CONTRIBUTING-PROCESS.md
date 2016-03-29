# Contributing Process

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

## Contributing Overview

1. Discuss your your proposed change

2. For the repo and implement your change / fix / feature

3. Get a code review & feedback

4. Submit a pull request

## Discuss your proposed change

* **For suggested features or substantial code contributions** please discuss it with the team and ensure it follows the product roadmap. We don't want you investing a lot of time developing something that may not be included in the project because it doesn't fix with the core contributors plans for the project.

* **For small changes / bug fixes ** open a GitHub issue, include your problem or suggestion and how you intend to implement it. Getting feedback will help ensure the quickest path to a successful pull request.

* May of our projects have **Community Standups** where you can join a real-time video discussion of the core teams plans for the upcoming sprint. The details for your projects community standup will be on the Readme.md for that project.

## Code reviews and checkins

To help ensure that only the highest quality code makes its way into the project, please submit all your code changes to GitHub as PRs. This includes runtime code changes, unit test updates, and updates to official samples (e.g. Music Store). For example, sending a PR for just an update to a unit test might seem like a waste of time but the unit tests are just as important as the product code and as such, reviewing changes to them is also just as important.

The advantages are numerous: improving code quality, more visibility on changes and their potential impact, avoiding duplication of effort, and creating general awareness of progress being made in various areas.

In general a PR should be signed off (using the :shipit: `:shipit:` emoticon) by the Subject Matter Expert (SME) of that code. For example, a change to the Banana project should be signed off by `@MrMonkey`, and not by `@MrsGiraffe`. If you don't know the SME, please talk to one of the engineering leads and they will be happy to help you identify the SME. Of course, sometimes it's the SME who is making a change, in which case a secondary person will have to sign off on the change (e.g. `@JuniorMonkey`).

To commit the PR to the repo **do not use the Big Green Button**. Instead, do a typical push that you would use with Git (e.g. local pull, rebase, merge, push).

TODO: Rule - 'Do You Know How To Submit Pull Requests'

## Branch strategy

TODO: To be updated to reflect GitHub flow

In general:

* `master` has the code for the latest release on NuGet.org (e.g. alpha, beta, RC, RTM)
* `dev` has the code that is being worked on but not yet released. This is the branch into which devs normally submit pull requests and merge changes into.
* `release` has the code that is being staged and stabilized for an upcoming release

Shortly before a release, the `release` branches are created from `dev`, and stabilization work happens there. Post-release work continus in the `dev` branch. Once the release takes place, the code is pushed from `release` to `master`.

## Commit/Pull Request Format

```
Summary of the changes (Less than 80 chars)
 - Detail 1
 - Detail 2

Addresses #bugnumber (in this specific format)
```
