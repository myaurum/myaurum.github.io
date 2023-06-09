# joshjohanning.github.io

## Overview

A DevOps Blog  - Blogging about Azure DevOps and GitHub practices, tips, and my continuous improvement DevOps journey.

[**josh-ops.com →**](https://josh-ops.com)

[![Build and Deploy](https://github.com/joshjohanning/joshjohanning.github.io/actions/workflows/pages-deploy.yml/badge.svg?branch=main)](https://github.com/joshjohanning/joshjohanning.github.io/actions/workflows/pages-deploy.yml)

## Theme Source

Chirpy:
* [GitHub](https://github.com/cotes2020/jekyll-theme-chirpy)
* [Example and tips/best practices](https://chirpy.cotes.info/)
* Upgrading - copy in the new source while taking care to keep existing configuration/customizations in place? 

## Comment System

Utterances:
* [GitHub](https://github.com/utterance/utterances)
* [Example/configuration](https://utteranc.es/)

The configuration lives in `/_layouts/post.html`

I added a section for dynamically selecting whether to use the light or dark utterances comment theme. I loosely documented this in an [issue in the utterances repository](https://github.com/utterance/utterances/issues/549#issuecomment-917091550).

See commit: [c432906](https://github.com/joshjohanning/joshjohanning.github.io/commit/c432906dcb3f5f66c1b9dee9dd2bde41c50f8332)

## Deviations from Chirpy

### Reverting to pre-4.3.0 sidebar background color for light mode

See: [#8](https://github.com/joshjohanning/joshjohanning.github.io/pull/8)

### Adding Speaking tab

Used an icon from [fontawesome](https://fontawesome.com/v4/icons/)

### Reverting 'preview image ratio to 1.91 : 1'

- Upstream commit: [4b6ccbc](https://github.com/cotes2020/jekyll-theme-chirpy/commit/4b6ccbcbccce27b9fcb035812efefe4eb69301cf)
- My changes so that preview image still shimmers before loading, but no image cropping: [b282712^..bb1dc1f](https://github.com/joshjohanning/joshjohanning.github.io/compare/b282712087028da95e292e3159d20cdf63d59feb^..bb1dc1f1bdbba4ee7d62858d834e0ca19f7745db)

## Upgrading the Theme

Since we aren't using the theme gem (so we can do customizations), we have to do it the old-fashioned way: 

1. Ensure chirpy is set as a remote: `git remote add chirpy https://github.com/cotes2020/jekyll-theme-chirpy.git`
2. . Ensure you have the latest upstream commit: `git fetch chirpy`
3. Compare the upstream [releases](https://github.com/cotes2020/jekyll-theme-chirpy/releases) and [commits](https://github.com/cotes2020/jekyll-theme-chirpy/commits/master) to find the first and last commit in the range you want to update. 
    - Recommendation is to use release tag milestones instead of loose commits that aren't contained in a release yet
    - For example, updating from 4.3.0 to 4.3.4 was referencing [a887f1d](https://github.com/cotes2020/jekyll-theme-chirpy/commit/a887f1d57d9ac8e08c789c6201147bf68c459573) (one right after [945e8d1](https://github.com/cotes2020/jekyll-theme-chirpy/commit/945e8d195393f73f38c4782cb31b808f09acc6f5)) and [602e984](https://github.com/cotes2020/jekyll-theme-chirpy/commit/602e98448d419e9c5710cb0c8a002a6538562150) (the merge commit for 4.3.4)
    - You can use this [link](https://github.com/cotes2020/jekyll-theme-chirpy/compare/a887f1d^..602e984) to compare the changes between two commits in GitHub
4. Start the `git cherry-pick`:
    - To cherry-pick a range of commits (more common): `git cherry-pick "a887f1d^..602e984" -m 1`
    - The `^` is important otherwise you will not be including the first commit - but the commit might get picked up in the maintainer's merge commit for the release anyways - and the quotes are necessary around the commits in `zsh` b/c of the `^`
    - To cherry-pick a single commit (probably not as common): `git cherry-pick a887f1d -m 1`
5. Review merge conflicts - use a combination of `git cherry-pick --skip` (for when readme/starter posts are updated) and `cherry-pick --continue` (to continue after you resolve real merge conflicts)
6. Starting in 5.6.0, run: `npm run build` and `git add assets/js/dist -f`
7. Rebase the number of commits you just brought in (you should see icon in VS Code): `git rebase -i HEAD~16`
    - Leave the top commit as `pick` but change the rest to `squash`
    - Update the commit message as appropriate
8. Update author and commit time: `git commit --amend --author "Josh Johanning <joshjohanning@github.com>" --date=now`
9. [Test changes locally before pushing](#building--testing-locally) 

## Building / Testing Locally

On Ubuntu / Intel-based Mac:

```sh
bundle install
npm i && npm run build
bundle exec jekyll s
```

On Apple M1 chip:

```sh
arch -arch x86_64 bundle install
arch -arch x86_64 bundle exec jekyll s

# if still having issues with ffi, also run:
arch -x86_64 sudo gem install ffi
```
