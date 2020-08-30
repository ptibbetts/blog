---
title: Conventional Commits
date: 2020-08-23
published: true
tags: ['git']
canonical_url: false
description: "Writing better Git commit messages"
---

I've been coding for eight years or so now and along the way two things have had a major impact on the way I write software. 

The first was using Git. 

The second was using it properly.

## Bad commit messages

```git
commit dedbb15124d61a6b4ffc3e3ec75ce71cb81c184b (origin/epic/phase-1, epic/phase-1)
Author: ******** ******* <*********.******@********.***>
Date:   Sat Jul 11 18:16:26 2020 +0100

    updated

commit 0c1db5c71f2c2e89d047f5d42f3c638015c67c5f
Author: ******** ******* <*********.******@********.***>
Date:   Fri Jul 10 23:11:29 2020 +0100

    customer view with orders and addresses

commit cc19334ed6ab93b2c5c4aa386ec1704fb854bf07
Author: ******** ******* <*********.******@********.***>
Date:   Thu Jul 9 20:52:19 2020 +0100

    updated

commit 5c273087700768cb7630aabcade4a32z19271ab6 (origin/fix/category-unique, fix/category-unique)
Author: Paul Tibbetts <code@paultibbettsdotuk>
Date:   Wed Jul 8 00:32:54 2020 +0100

    fix: category name in factory needs to be unique to match migration
```

It's ok \*\*\*\*\*\*\*\*, we've all done it.

At one point or another we've all written something useless  as our commit message and now everyone can see we wrote a commit that:

- doesn't explain what it does
- doesn't say if it breaks anything
- means I need to inspect the commit to see what's been `updated`**!**

## Good commit messages

The good news is I have found the solution:

> The Conventional Commits specification is a lightweight convention on top of commit messages. 
[https://www.conventionalcommits.org/en/v1.0.0/#summary](https://www.conventionalcommits.org/en/v1.0.0/#summary)

and by following it:

- your colleagues can gain a better understanding of what's going on
- you can use it to follow [SEMVER](https://semver.org/) and create better releases of your software
- you can use tools to do things like generate the changelog automatically, close github issues and also mark them as released


```git
commit d036a899d51568d616ecff7fc11ffc483d5bfabf (HEAD -> master, origin/master, origin/HEAD)
Author: ylemkimon <y@ylemdotkim>
Date:   Tue Aug 18 08:26:13 2020 +0900

    ci(docs): use actions/checkout@v2 (#1620)

commit 9303d1dba0790fb765ae46625d8e7fdecbf6ffe6
Author: AbdelRahman Wahdan <abdelrahman.wahdan@outlookdotcom>
Date:   Fri Jul 31 02:17:36 2020 +0200

    docs(resources.md): added more semantic release article (#1610)

commit b72cdb331b6db057ec0f44cf4f6a281726075f3b
Author: kopal <50901044+kopal2212@users.noreply.github.com>
Date:   Thu Jul 30 03:55:32 2020 +0530

    docs(configuration.md): Updated documentation for dry-run feature of semantic Release (#1607)

commit 0f0c650b41764d1a3deb33631147c7ca0e39fe59 (tag: v17.1.1)
Author: Emmanuel Ogbizi <iamogbz+github@gmail.com>
Date:   Thu Jun 25 12:30:12 2020 -0400

    fix: use correct ci branch context (#1521)

commit a4658016d957a9a240051e51d77388f1345bd6ec (tag: v17.1.0)
Author: Sven Liebig <liebigsv@gmail.com>
Date:   Mon Jun 22 20:26:24 2020 +0200

    feat(bitbucket-basic-auth): support for bitbucket server basic auth (#1578)

commit 6d48663a77c0cafb6ea5adfe18ad4c4eda655413
Author: Daniel Wirtz <dcode@dcode.io>
Date:   Mon Jun 15 18:16:33 2020 +0200

    build(gitattributes): eol=lf for Windows users (#1575)

commit eed1d3c8cbab0ef05df39866c90ff74dff77dfa4 (tag: v17.0.8)
Author: Nicholas Shine <shine.nick@gmail.com>
Date:   Sun May 24 13:53:00 2020 -0500

    fix: prevent false positive secret replacement for Golang projects (#1562)
```

## Summary

The spec is used to describe `features`, `fixes` and `breaking changes` by writing your commit messages with the following convention:

- **fix:** patches a bug in the codebase (`PATCH`)
- **feat:** a new feature (`MINOR`)
- **BREAKING CHANGE:** contains a breaking API change  (`MAJOR`) - this is written in the footer of the commit message and can apply to any type of commit

These prefixes are the minimum you need to get started writing Conventional Commits however the default commitlint config[1] understands a few more:

- **docs:**  changes the documentation
- **style:** changes that do not alter the way the code behaves, only how it looks
- **refactor:**  changes that neither fix a bug or alter the way something behaves
- **perf:** changes that only improve performance
- **test:** adding missing tests or fixing existing ones
- **build:** changing the build system of the project
- **ci:** changing the CI config or scripts
- **chore:** any other change
- **revert:** reverting a previous commit

### Scopes

You can optionally pass in a `(scope)` to the prefix you are using if you want to highlight a specific area of the codebase that was changed or group related changes together.

For instance a change of 

```
docs(readme): adds badges
```

tells us the documentation was changed, specifically the `README.md` file.

## SEMVER

Starting your commit messages with these prefixes will make it clear exactly what has changed and what sort of release should follow.

If the release would contain only bug fixes, refactors or documentation changes then only a `PATCH` release is needed. Your users can update safely.

```diff
- v1.0.0
+ v1.0.1
```

If the release contains any new features then a `MINOR` release is needed. Users will be able to update to this release with no effort required and will benefit from the new features you've added.

```diff
- v1.0.0
+ v1.1.0
```

If the release however contains a `BREAKING CHANGE` then a `MAJOR` release is required. This tells the user that they will need to do something when they update to this release.

```diff
- v1.0.0
+ v2.0.0
```

## Getting started

To get started writing Conventional Commits I recommend installing the [Commitizen](https://www.npmjs.com/package/commitizen) CLI tool with npm:

```shell
npm install -g commitizen
```

It looks like this when you use it:

```
➜ git commit
husky > prepare-commit-msg (node v14.8.0)
cz-cli@4.1.2, cz-conventional-changelog@3.2.0

? Select the type of change that you're committing: 
❯ feat:     A new feature 
  fix:      A bug fix 
  docs:     Documentation only changes 
  style:    Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons
, etc) 
  refactor: A code change that neither fixes a bug nor adds a feature 
  perf:     A code change that improves performance 
(Move up and down to reveal more choices)
```

You will then have to setup your project to follow the Convention which can be done with:

```shell
npx commitizen init cz-conventional-changelog --save-dev --save-exact
```

I also use [Husky](https://github.com/typicode/husky) to run commitizen for me when I type `git commit` which involves adding the following hook:

```json
"prepare-commit-msg": "exec < /dev/tty && git cz --hook || true",
```

### As one command

You can run the following to set it all up in one go:

```shell
npx @ptibbetts/conventional-commits-starter
```

### Writing commits

Commitizen will take care of this for you but your commits will end up following this structure:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

An example would be:

```
feat(example): adds an example

to make it easier to understand Conventional commits

BREAKING CHANGE: it breaks bad habits writing useless git commits?

fixes github issue #123
```

Get started by running `git commit`.

## And then?

And then that's it! 

You're ready to start writing better commit messages.

[1] You could also use [`commitlint`](https://github.com/conventional-changelog/commitlint) to force others into following the Convention when working on your project.

In my next post I'll be writing about how I use Conventional Commits and [semantic-release](https://github.com/semantic-release/semantic-release) when releasing software on GitHub.