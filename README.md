[![CI](https://github.com/lpenz/urxvt-ext-notify-osc/actions/workflows/ci.yml/badge.svg)](https://github.com/lpenz/urxvt-ext-notify-osc/actions/workflows/ci.yml)

urxvt-ext-notify-osc
====================

urxvt-ext-notify-osc is a urxvt OSC extension for desktop
notifications.

In essence, this means that whenever a process prints a string like
the following:

```shell
printf '\e]777;notify;alert user;the tea is ready\a'
```

urxvt creates a desktop notification with *alert notification* as
summary and *the tea is ready* as the body. The terminal window also
get its urgency hint set, which makes the window manager call your
attention to it.

This works well even when a remote program in an ssh session prints
the string. There is no dependency on X forwarding.


## Installation

### Manual installation

#### Requirements

- *notify-send* is used to create the notifications. It lives
  in the *libnotify-bin* Debian package.

- *xseturgent* is used to set the urgent hint. You can find in source
  form in [github](https://github.com/lpenz/xseturgent), or as a
  Debian package/repository
  in [packagecloud](https://packagecloud.io/lpenz/lpenz).
  (disclaimer: I'm the author)

If one of these utilities is not present, the corresponding feature
doesn't work.


#### Installing

Copy the *notify-osc* file to `~/.urxvt/ext`.


### Debian package

You can find a debian package
in [my packagecloud repository](https://packagecloud.io/lpenz/lpenz).

This options gets the dependencies sorted automatically.


### Enabling

Add *notify-osc* to the list of enabled urxvt extensions in `~/.Xresources`:

```
URxvt.perl-ext-common: ...,notify-osc
```

Reload the resources after editing that file by running:

```shell
xrdb -merge ~/.Xresources
```

