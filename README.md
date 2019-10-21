# Homebrew Asterisk

[![Build Status](https://travis-ci.org/leedm777/homebrew-asterisk.svg?branch=master)](https://travis-ci.org/leedm777/homebrew-asterisk)

This repo contains the Homebrew formulas I use for my [Asterisk][ast] dev
machine. This is pretty much a few packages which, for one reason or
another, aren't the best fit for going into Homebrew itself.

## Installation

    xcode-select --install
    brew tap preetsethi/homebrew-asterisk (originally: brew tap leedm777/homebrew-asterisk)
    brew install asterisk

## Installation options

 * `--with-clang` - Compile with clang instead of gcc
   * This is a new-ish option in Asterisk, and might be a bit crashy.
 * `--with-dev-mode` - Enable dev mode in Asterisk
   * Disable optimizations, turns up build warnings, and enables the test
     framework.
 * `--devel` - Install development version 13
   * Build the latest code from the 13 branch.
 * `--HEAD` - Install HEAD version
   * Build the latest code from the master branch.

## Configuration

Configuration files are in `/usr/local/etc/asterisk`. Detailed configuration
docs can be found on the [Asterisk wiki][config-docs].

If you have problems after an upgrade, it may be because of bad path information
that ended up in `asterisk.conf`. Check that the directories section looks like:

    [directories](!)
    astetcdir => /usr/local/etc/asterisk
    astmoddir => /usr/local/opt/asterisk/lib/asterisk/modules
    astvarlibdir => /usr/local/var/lib/asterisk
    astdbdir => /usr/local/var/lib/asterisk
    astkeydir => /usr/local/var/lib/asterisk
    astdatadir => /usr/local/var/lib/asterisk
    astagidir => /usr/local/var/lib/asterisk/agi-bin
    astspooldir => /usr/local/var/spool/asterisk
    astrundir => /usr/local/var/run/asterisk
    astlogdir => /usr/local/var/log/asterisk
    astsbindir => /usr/local/opt/asterisk/sbin

## Running asterisk

If you want to just run Asterisk occasionally, just start it up using
`/usr/local/sbin/asterisk -c`. It is recommended to *not* run Asterisk as root.

## Running as a service

To have launchd start asterisk now and restart at login:

    $ brew services start leedm777/asterisk/asterisk

To stop asterisk:

    $ brew services stop leedm777/asterisk/asterisk

To reload asterisk after an upgrade:

    $ brew services restart leedm777/asterisk/asterisk

To connect to Asterisk running as a service:

    $ /usr/local/sbin/asterisk -r

## Uninstall

To uninstall Asterisk, run `brew rm asterisk`. To get rid of all local state and
configuration data:

    $ rm -rf /usr/local/etc/asterisk /usr/local/var/lib/asterisk \
        /usr/local/var/log/asterisk /usr/local/var/run/asterisk \
        /usr/local/var/spool/asterisk

## Upgrading from older versions

I used to have notes in the README for creating an `asterisk` user for running
Asterisk. This is not very homebrew-y, so I dropped it.

I also had a custom plist that I recommended instead of homebrew's built-in
plist feature. If you had followed those instructions, you may need to remove
`/Library/LaunchDaemons/org.asterisk.asterisk.plist` before installing
[the new plist above](#Running as a service).

## Common problems

**Compilation fails with `error: /usr/lib/bundle1.o: No such file or directory`**

This happens when you try to build Asterisk without the XCode CLT installed.
This also [affects how GCC is built](Homebrew/homebrew#34461), so you will also
want to reinstall GCC. The good news is that with the CLT installed, GCC should
install via bottle, which is tons faster than building it from source.

    xcode-select --install
    brew rm gcc
    brew install asterisk

**Configure fails with `The Asterisk menuselect tool requires the 'libxml2' development package.`**

This happens because of [a bug](libxml2-bug) with Xcode 8 on OSX 10.11 (El
Capitan). Right now, the only solution is to upgrade to macOS 10.12 (Sierra),
downgrade to Xcode 7.3, or do some pretty [terrifying edits](http://stackoverflow.com/q/39536144/115478)
to your system libraries.

 [ast]: http://asterisk.org/
 [config-docs]: https://wiki.asterisk.org/wiki/x/cYXAAQ
 [libxml2-bug]: https://github.com/leedm777/homebrew-asterisk/issues/23
