# Setup

## Show or Hide hidden files in finder using
```bash
defaults write com.apple.finder AppleShowAllFiles TRUE

defaults write com.apple.finder AppleShowAllFiles FALSE
```

## FTP File server

For read only access the finder works fine, but does not support write operations.

Use `Cyberduck`

## Mouse and Touchpad

Reverse behavior for non-apple mouse and touchpad

Use `Scroll Reverser`


## Terminal

Current: `Ghostty`

### Package Manager

Using `brew` at the moment.

### Update path always

To always update path `.zshrc` file needs to be updated to update the path.
`export PATH=...` only seems to work in current session.

### Live update terminal session with latest config

`source ~/.zshrc` to live update the session with latest changes in `.zshrc`