# reproducing npm bugs with git-based dependencies

To reproduce the issues, always run the commands without a `node_modules` directory.

## `npm install` broken with `npm` @ `6.9 - 6.11`.

- `npm install` installs `caolan/async#f9ad468` (a.k.a. `latest`) instead of `caolan/async#4752a79` and updates the lockfile entry.
