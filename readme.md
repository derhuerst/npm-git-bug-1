# reproducing npm bugs with git-based dependencies

To reproduce the issues, always run the commands without a `node_modules` directory.

## `npm install` broken with `npm` @ `6.9 - 6.11`.

- `npm install` installs `caolan/async#f9ad468` (a.k.a. `latest`) instead of `caolan/async#4752a79` and updates the lockfile entry.
- `npm install` doesn't install the nested dependency `lodash/lodash#ddfd9b1` of `derhuerst/npm-git-bug-2#52dce58` and removes it from the lockfile.

```shell
npm --version # 6.11.3
npm ls
```

```
@derhuerst/npm-git-bug-1@1.0.0 /Users/j/playground/npm-git-bug-1
├─┬ @derhuerst/npm-git-bug-2@1.1.0 (github:derhuerst/npm-git-bug-2#52dce58e43c064469c4262371c212f76beef2135)
│ └── UNMET DEPENDENCY lodash@github:lodash/lodash#semver:^4.17.15
└── async@3.1.0 (github:caolan/async#f9ad468f7b464f5ef88116def2399a5c6e59e21d)

npm ERR! missing: lodash@github:lodash/lodash#semver:^4.17.15, required by @derhuerst/npm-git-bug-2@1.1.0
```

```
npm info it worked if it ends with ok
npm verb cli [
npm verb cli   '/usr/local/Cellar/node/12.10.0/bin/node',
npm verb cli   '/usr/local/bin/npm',
npm verb cli   'i',
npm verb cli   '--verbose'
npm verb cli ]
npm info using npm@6.11.3
npm info using node@v12.10.0
npm verb npm-session 5f9696eaf8e0bf9a
npm info lifecycle @derhuerst/npm-git-bug-1@1.0.0~preinstall: @derhuerst/npm-git-bug-1@1.0.0
npm timing stage:loadCurrentTree Completed in 9ms
npm timing stage:loadIdealTree:cloneCurrentTree Completed in 0ms
npm timing stage:loadIdealTree:loadShrinkwrap Completed in 9ms
npm info lifecycle async@3.1.0~prepack: async@3.1.0
npm info lifecycle async@3.1.0~postpack: async@3.1.0
npm timing stage:loadIdealTree:loadAllDepsIntoIdealTree Completed in 22645ms
npm timing stage:loadIdealTree Completed in 22659ms
npm timing stage:generateActionsToTake Completed in 16ms
npm verb correctMkdir /Users/j/.npm/_locks correctMkdir not in flight; initializing
npm verb lock using /Users/j/.npm/_locks/staging-20f860054522167f.lock for /Users/j/playground/npm-git-bug-1/node_modules/.staging
npm timing audit submit Completed in 998ms
npm http fetch POST 200 https://registry.npmjs.org/-/npm/v1/security/audits/quick 999ms
npm timing audit body Completed in 2ms
npm info lifecycle @derhuerst/npm-git-bug-2@1.1.0~prepack: @derhuerst/npm-git-bug-2@1.1.0
npm info lifecycle @derhuerst/npm-git-bug-2@1.1.0~postpack: @derhuerst/npm-git-bug-2@1.1.0
npm info lifecycle async@3.1.0~prepack: async@3.1.0
npm info lifecycle async@3.1.0~postpack: async@3.1.0
npm timing action:extract Completed in 14379ms
npm timing action:finalize Completed in 3ms
npm timing action:refresh-package-json Completed in 9ms
npm info lifecycle @derhuerst/npm-git-bug-2@github:derhuerst/npm-git-bug-2#52dce58e43c064469c4262371c212f76beef2135~preinstall: @derhuerst/npm-git-bug-2@github:derhuerst/npm-git-bug-2#52dce58e43c064469c4262371c212f76beef2135
npm info lifecycle async@3.1.0~preinstall: async@3.1.0
npm timing action:preinstall Completed in 1ms
npm info linkStuff @derhuerst/npm-git-bug-2@github:derhuerst/npm-git-bug-2#52dce58e43c064469c4262371c212f76beef2135
npm info linkStuff async@3.1.0
npm timing action:build Completed in 2ms
npm info lifecycle @derhuerst/npm-git-bug-2@github:derhuerst/npm-git-bug-2#52dce58e43c064469c4262371c212f76beef2135~install: @derhuerst/npm-git-bug-2@github:derhuerst/npm-git-bug-2#52dce58e43c064469c4262371c212f76beef2135
npm info lifecycle async@3.1.0~install: async@3.1.0
npm timing action:install Completed in 1ms
npm info lifecycle @derhuerst/npm-git-bug-2@github:derhuerst/npm-git-bug-2#52dce58e43c064469c4262371c212f76beef2135~postinstall: @derhuerst/npm-git-bug-2@github:derhuerst/npm-git-bug-2#52dce58e43c064469c4262371c212f76beef2135
npm info lifecycle async@3.1.0~postinstall: async@3.1.0
npm timing action:postinstall Completed in 1ms
npm verb unlock done using /Users/j/.npm/_locks/staging-20f860054522167f.lock for /Users/j/playground/npm-git-bug-1/node_modules/.staging
npm timing stage:executeActions Completed in 14456ms
npm timing stage:rollbackFailedOptional Completed in 0ms
npm info linkStuff @derhuerst/npm-git-bug-1@1.0.0
npm info lifecycle @derhuerst/npm-git-bug-1@1.0.0~install: @derhuerst/npm-git-bug-1@1.0.0
npm info lifecycle @derhuerst/npm-git-bug-1@1.0.0~postinstall: @derhuerst/npm-git-bug-1@1.0.0
npm info lifecycle @derhuerst/npm-git-bug-1@1.0.0~prepublish: @derhuerst/npm-git-bug-1@1.0.0
npm info lifecycle @derhuerst/npm-git-bug-1@1.0.0~prepare: @derhuerst/npm-git-bug-1@1.0.0
npm timing stage:runTopLevelLifecycles Completed in 37212ms
npm verb saving []
npm verb shrinkwrap skipping write for package.json because there were no changes.
npm info lifecycle undefined~preshrinkwrap: undefined
npm info lifecycle undefined~shrinkwrap: undefined
npm info lifecycle undefined~postshrinkwrap: undefined
added 2 packages from 2 contributors and audited 2 packages in 37.267s
found 0 vulnerabilities

npm verb exit [ 0, true ]
npm timing npm Completed in 37567ms
npm info ok
```


## `npm ls` broken with `npm@6.11.3`

- `npm ci` installs all dependencies as specified in the lockfile, but `npm ls` seems not to find them.

```shell
npm --version # 6.11.3
npm ls
```

```
@derhuerst/npm-git-bug-1@1.0.0 /Users/j/playground/npm-git-bug-1
├─┬ UNMET DEPENDENCY @derhuerst/npm-git-bug-2@github:derhuerst/npm-git-bug-2#52dce58e43c064469c4262371c212f76beef2135
│ └── UNMET DEPENDENCY lodash@github:lodash/lodash#ddfd9b11a0126db2302cb70ec9973b66baec0975
├── UNMET DEPENDENCY async@github:caolan/async#4752a792f5f9272e8db7f6035122ee9d7dd53539
└── UNMET DEPENDENCY lodash@github:lodash/lodash#ddfd9b11a0126db2302cb70ec9973b66baec0975

npm ERR! missing: async@github:caolan/async#v3.1.0, required by @derhuerst/npm-git-bug-1@1.0.0
npm ERR! missing: lodash@github:lodash/lodash#semver:^4.17.15, required by @derhuerst/npm-git-bug-1@1.0.0
npm ERR! missing: @derhuerst/npm-git-bug-2@github:derhuerst/npm-git-bug-2#semver:^1.1.0, required by @derhuerst/npm-git-bug-1@1.0.0
npm ERR! extraneous: async@github:caolan/async#4752a792f5f9272e8db7f6035122ee9d7dd53539 /Users/j/playground/npm-git-bug-1/node_modules/async
npm ERR! extraneous: lodash@github:lodash/lodash#ddfd9b11a0126db2302cb70ec9973b66baec0975 /Users/j/playground/npm-git-bug-1/node_modules/lodash
npm ERR! missing: lodash@github:lodash/lodash#ddfd9b11a0126db2302cb70ec9973b66baec0975, required by @derhuerst/npm-git-bug-2@github:derhuerst/npm-git-bug-2#52dce58e43c064469c4262371c212f76beef2135
```

## minor annoyances

- With `npm@6.11.3`, `npm ci` doesn't to a *shallow* `git clone`.
