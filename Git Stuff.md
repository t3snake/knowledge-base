# Git Stuff

## Remove files/directory from history

```
pip install git-filter-repo
```

Installs git-filter-repo into PATH.
Uses git filter-path internally and provides convenient command.

```
git-filter-repo --path <path_to_dir_or_file> --invert-paths
```

Removes and rebases all affected commits automatically.
Also removes remote to add friction.

```
pip uninstall git-filter-repo
```