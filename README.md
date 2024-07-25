# `gogitest`

> *`gogitest` (/go-gee-test/), originally from "Go Git Test" (due to the melding of `git` and `go` commands).*

***This tool tries to solve one problem: make it easier to test local go changes without having to "test the world".***

In large projects with many dependencies, changing a single file can result in many compilation and test failures in other local dependent packages. `gogitest` detects your changes with git, looks for packages that may depend on those changes, and tests them all for you. No more hunting down dependencies manually, stop playing games of ping pong against your CI test suite, don't wait for 30 minutes while the full test suite runs locally and then still fails due to unrelated environment issues.

Just run `gogitest`.

## Install

Clone this repo, then add the `./bin` folder to your `$PATH`:

```bash
cd ~/git
git clone git@github.com:fisherevans/gogitest.git
export PATH="$PATH:$HOME/git/gogitest/bin"
```

You will need to have `go` and `git` installed.

## Examples

Check out `gogitest -help` for all available options and arguments. The command will not make any change to your filesystem.

- **`gogitest`**
  - Detect all changes (staged and unstaged) compared to the merge base of the main development branch.
  - Find all packages that might depend on the changed (by naively looking for references to the fully qualified package import name).
  - `go test` all discovered packages.
- **`gogitest -match http -- -args integration`**
  - Only test and inspect packages with `http` within the fully qualified name.
  - Pass in `-args integration` to `go test`.
- **`gogitest -local`**
  - When detecting changes, compare your current filesystem to the `origin` of the current branch.
- **`gogitest -nodeps -base some-non/prod-branch`**
  - Skip package dependency detection, just test the modified packages.
  - Detect the merge base of `some-non/prod-branch` instead of the main development branch.

- **`gogitest -build -ignore kafka 'native$'`**
  - Only build the impacted packages and their tests without running them.
  - Ignore any packages with `kafka` anywhere inside the fully qualified import reference.
  - Ignore any packages with `native` at the end of the fully qualified import reference.
