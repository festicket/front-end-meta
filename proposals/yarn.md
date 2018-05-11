# Yarn Configuration
## Problem
By always running `pure-lockfile` locally through our `.yarnrc` configuration, we have lost the ability for Yarn to manage merge conflicts, because we are always disabling Yarn's ability to update the lockfile.


## Solution
According to this discussion (https://github.com/yarnpkg/website/issues/700) the recommended setup is to;

1. **Not use `pure-lockfile` locally.** The hypothesis that dependencies will be upgraded all the time without this is not correct.

> Yarn will continue to install the version specified in the lockfile, even if a new version comes out. That's the purpose of the lockfile - All of your dependencies are locked to a 'known good' version.

2. **Use `yarn install --frozen-lockfile` in CI.** This means that a CI build will fail if `package.json` is modified without running `yarn` afterwards. This ensures that the lockfile is always synced up with `package.json`.

> --frozen-lockfile will only fail a build if the package.json is inconsistent with the lockfile, so it's highly recommended and should never break a legitimate build. 
