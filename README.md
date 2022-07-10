### DBUS-ACTION
[![AUR](https://img.shields.io/aur/version/dbus-action)](https://aur.archlinux.org/packages/dbus-action/)

[dbus-action][REPO] is a program which listens to D-Bus and actions
configured commands on specified messages. A message is specifying using
bus + interface + member + response value, and this can be mapped to
trigger any arbitrary command. You can choose to [automatically
start](https://specifications.freedesktop.org/autostart-spec/autostart-spec-latest.html)
dbus-action in your desktop environment to action commands in response
to events in that environment.

The latest version and documentation is available at
https://github.com/bulletmark/dbus-action.

### INSTALLATION

NOTE: Arch users can just install [_dbus-action from the
AUR_][AUR]. Then skip to the next CONFIGURATION section.

You need python 3.6 or later, python2 is not supported. You also need
[PyGObject](https://pypi.org/project/PyGObject/),
[python3-dbus](https://pypi.org/project/dbus-python/),
and [python3-ruamel-yaml](https://pypi.org/project/ruamel.yaml/)
packages.

Install this software:

    git clone https://github.com/bulletmark/dbus-action.git
    cd dbus-action
    sudo make install (or sudo ./dbus-action-setup install)

### CONFIGURATION

The default configuration file is in `/etc/dbus-action.conf`. You will
want to create your own custom triggers and actions so copy that file to
`~/.config/dbus-action.conf` and edit it. Options and an example are
described in comments within that file. The example shows how to action
a command when your machine suspends, and then another command when it
resumes from suspend.

For other custom actions, most likely you will first need to determine
the interface, member, and responses you want to trigger on. To help
with this, you can run `dbus-action` in monitor mode to view all
messages. Run the following as your normal user in your normal session
(DO NOT use root/sudo):

    dbus-action -m all

Note, instead of `-m all`, you can choose `-m session` or `-m system` to
limit listening to those specific buses only. You can also add `-i
interface` to further limit output to a specific interface.

Then perform the action you would like to intercept and capture the
D-Bus message. In your `~/.config/dbus-action.conf`, configure the bus,
interface, member, and response values to commands which you would like
to trigger. Note that the response is a list of values (although often
only a list of 1) so you must set `value_index` in your configuration to
the index of the value in the returned list you want to compare. It
defaults to 0, i.e. the first value returned.

### STARTING AND STOPPING

You must choose between starting the application as a [systemd user
service](https://wiki.archlinux.org/index.php/Systemd/User), or as a
[desktop
application](https://specifications.freedesktop.org/autostart-spec/autostart-spec-latest.html)
(with an XDG compliant DE such as GNOME and KDE). The systemd user
service provides more robust management and better logging than the
desktop so is the preferred choice. Choose one of the two following
options:

1. To set up the application as a [systemd user
   service](https://wiki.archlinux.org/index.php/Systemd/User):

````
dbus-action-setup service
````

2. Or instead, to set up the application using your
   [DE](https://specifications.freedesktop.org/autostart-spec/autostart-spec-latest.html):

````
dbus-action-setup desktop
````

After *choosing one of the above*, you can use then run the following commands:

Enable the app to start automatically in the background when you
log in with:

    dbus-action-setup autostart

Disable the app from starting automatically with:

    dbus-action-setup autostop

Start the app immediately in the background:

    dbus-action-setup start

Stop the background app immediately with:

    dbus-action-setup stop

Restart the app, e.g. to reload the configuration file, with:

    dbus-action-setup restart

Check the status of the app with:

    dbus-action-setup status

Note if you are starting using the DE option and you are using some
uncommon systems then `dbus-action-setup start` may fail
to start the application returning you a message _Don't know how to
invoke dbus-action.desktop_. If you get this error message,
install the dex package, preferably from your system packages
repository, and try again.

### UPGRADE

    # cd to source dir, as above
    git pull
    sudo make install (or sudo ./dbus-action-setup install)
    dbus-action-setup restart

### REMOVAL

    dbus-action-setup stop
    dbus-action-setup autostop
    sudo dbus-action-setup uninstall

### COMMAND LINE USAGE

```
usage: dbus-action [-h] [-c CONFFILE] [-v] [-m MONITOR] [-i INTERFACE]

Watch D-Bus to action configured commands on specific events.

optional arguments:
  -h, --help            show this help message and exit
  -c CONFFILE, --conffile CONFFILE
                        alternative configuration file
  -v, --verbose         verbose output
  -m MONITOR, --monitor MONITOR
                        just monitor given bus, or "all" buses
                        (session,system)
  -i INTERFACE, --interface INTERFACE
                        limit monitor output to specific interface
```

### LICENSE

Copyright (C) 2020 Mark Blakeney. This program is distributed under the
terms of the GNU General Public License.
This program is free software: you can redistribute it and/or modify it
under the terms of the GNU General Public License as published by the
Free Software Foundation, either version 3 of the License, or any later
version.
This program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General
Public License at <https://www.gnu.org/licenses/> for more details.

[REPO]: https://github.com/bulletmark/dbus-action/
[AUR]: https://aur.archlinux.org/packages/dbus-action/

<!-- vim: se ai syn=markdown: -->
