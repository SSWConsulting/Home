# Contributing Process

## Commit/Pull Request Format

```
Summary of the changes (Less than 80 chars)
 - Detail 1
 - Detail 2

Addresses #bugnumber (in this specific format)
```


## Issue tracking

Bug management takes place in GitHub. Each repo has its own issue tracker. Bugs cannot be moved between repos so make sure you open a bug in the right repo. If a bug is opened in the wrong repo someone will have to manually copy it to the correct repo.

We use the HuBoard pattern for issue tags. Look at the numerical tags that SignalR uses for an idea: https://github.com/SignalR/SignalR/issues

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

### Code reviews and checkins

To help ensure that only the highest quality code makes its way into the project, please submit all your code changes to GitHub as PRs. This includes runtime code changes, unit test updates, and updates to official samples (e.g. Music Store). For example, sending a PR for just an update to a unit test might seem like a waste of time but the unit tests are just as important as the product code and as such, reviewing changes to them is also just as important.

The advantages are numerous: improving code quality, more visibility on changes and their potential impact, avoiding duplication of effort, and creating general awareness of progress being made in various areas.

In general a PR should be signed off (using the :shipit: `:shipit:` emoticon) by the Subject Matter Expert (SME) of that code. For example, a change to the Banana project should be signed off by `@MrMonkey`, and not by `@MrsGiraffe`. If you don't know the SME, please talk to one of the engineering leads and they will be happy to help you identify the SME. Of course, sometimes it's the SME who is making a change, in which case a secondary person will have to sign off on the change (e.g. `@JuniorMonkey`).

To commit the PR to the repo **do not use the Big Green Button**. Instead, do a typical push that you would use with Git (e.g. local pull, rebase, merge, push).

TODO: Rule - 'Do You Know How To Submit Pull Requests'



### Branch strategy

TODO: To be updated to reflect GitHub flow

In general:

* `master` has the code for the latest release on NuGet.org (e.g. alpha, beta, RC, RTM)
* `dev` has the code that is being worked on but not yet released. This is the branch into which devs normally submit pull requests and merge changes into.
* `release` has the code that is being staged and stabilized for an upcoming release

Shortly before a release, the `release` branches are created from `dev`, and stabilization work happens there. Post-release work continus in the `dev` branch. Once the release takes place, the code is pushed from `release` to `master`.

**Tests**
