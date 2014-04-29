git-local
=========

Git-local uses a separate bare Git repository to track files that the main
repository should not track.
Especially useful for versioning configuration files.
Most git commands are just passed through, and a few are special:

 * git local init
 * git local status
 * git local add


## Example

Inside a git repository,

```sh
$ git status --short
?? src/config/local.yml
```

create an overlapping, but purely local repository,

```sh
$ git local init
Initialized empty Git repository in /home/me/project/.git-local/
$ git local status --short
## Tracked and modified files:

## Untracked files that upstream ignores or does not track:
src/config/local.yml
```

where you can add files, commit, and use any git command:

```sh
$ git local add src/config/local.yml
$ git local commit -m "local config"
[master (root-commit) 81aeaf5] 1
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 other
$ git local log --format=oneline
81aeaf5dd5b86fce443cc06cbb6e237aa7432241 local config
```

The default behavior for the locally tracked files is to
ignore them in the main repository.
Here, the YAML file was added to the main `.gitignore`:

```sh
$ git status
?? .gitignore

$ git local status -v
## Tracked files:
src/config/local.yml
 
## Tracked and modified files:
nothing to commit (use -u to show untracked files)

## Untracked files that upstream ignores or does not track:
.gitignore
```


## Commands

### init

Initialize a local repository in `.git-local`, next to `.git`.

`--no-gitignore`
:   Do not create/update the `.gitignore` file to hide the local
    repository.

### status

With no option, lists files locally commited, or ignored upstream, or
untracked upstream. To set a list of patterns to ignore, modify
`.git-local/info/exclude`.

`--verbose` `-v`
:   Display also the tracked and unchanged files.

### add

The files tracked by the upstream repository will be ignored.

`--no-gitignore`
:   Do not add lines in the upstream `.gitignore` about the files
    tracked bu git-local.

