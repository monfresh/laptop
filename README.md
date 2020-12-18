# Laptop

[![Build Status](https://travis-ci.org/monfresh/laptop.svg)](https://travis-ci.org/monfresh/laptop)

Laptop is a script to set up a macOS computer for web development, and to keep
it up to date.

It can be run multiple times on the same machine safely. It installs,
upgrades, or skips packages based on what is already installed on the machine.

This particular version of the script is geared toward beginners who want to
set up a Ruby environment on their Mac to be able to install gems such as Rails
or Jekyll. More advanced users can easily [customize](#customize-in-laptoplocal-and-brewfilelocal)
the script to install additional tools.

For more coding guides and tools, subscribe to my [newsletter](http://eepurl.com/1-dtn). It goes out once a week on Thursdays. I write about Ruby, Rails, Mac productivity and automation, static sites, and more on
my [site](https://www.moncefbelyamani.com).

## Requirements

**Macs with the M1 chip are not currently supported**

Supported operating systems:

- I'll be testing BigSur soon (on Intel Macs)
- macOS Catalina (10.15)
- macOS Mojave (10.14)
- macOS High Sierra (10.13)
- macOS Sierra (10.12)
- OS X El Capitan (10.11)
- OS X Yosemite (10.10)
- OS X Mavericks (10.9)

Supported shells:

- bash
- fish
- zsh

## Install
**READ THIS FIRST! PLEASE WAIT UNTIL DECEMBER 21 BEFORE RUNNING THIS SCRIPT.
THERE SEEMS TO BE A BUG WITH ONE OF THE TOOLS THAT JUST STARTED HAPPENING A FEW DAYS AGO.
I AM WORKING WITH THE DEVELOPER TO FIX IT, AND I AM ALSO LOOKING FOR ALTERNATIVES.
THANK YOU FOR YOUR PATIENCE!**

**IMPORTANT:** Before you run the script, make sure you have the latest Apple software updates installed. Check by going to System Preferences, then Software Update. If you're on Catalina, this does not mean upgrading to Big Sur, just the latest Catalina updates.

Begin by opening the Terminal application on your Mac. The easiest way to open
an application in macOS is to search for it via [Spotlight]. The default
keyboard shortcut for invoking Spotlight is `command-Space`. Once Spotlight
is up, just start typing the first few letters of the app you are looking for,
and once it appears, press `return` to launch it.

In your Terminal window, copy and paste the command below, then press `return`.

```sh
bash <(curl -s https://raw.githubusercontent.com/monfresh/laptop/master/laptop)
```

The [script](https://github.com/monfresh/laptop/blob/master/mac) itself is
available in this repo for you to review if you want to see what it does
and how it works.

Note that the script might ask you to enter your macOS password at various
points. This is the same password that you use to log in to your Mac. The
prompt comes from Homebrew, because it needs permissions to write to the
`/usr/local` directory.

If you
have `rbenv` or `RVM` installed, the script will ask you to uninstall them first
and run the script again. Look at the script output for uninstallation
instructions.

**Once the script is done, quit and relaunch Terminal.**

I recommend running the script regularly to keep your computer up
to date. Once the script has been installed, you'll be able to run it at your
convenience by typing `laptop` and pressing `return` in your Terminal.

[spotlight]: https://support.apple.com/en-us/HT204014

## How to tell if the script worked

If the last thing the script displayed was "All done!", then everything the script was meant to do worked.

To verify that the Ruby environment is properly configured, run one or more of these
commands:

```shell
ruby -v
```

This should show `ruby 2.7.2p137` or later. If not, try quitting and relaunching Terminal.

```shell
which ruby
```

This should point to the `.rubies` directory in your home folder. For example:

```
/Users/monfresh/.rubies/ruby-2.7.2/bin/ruby
```

## How to install gems such as Rails or Jekyll

Once you run the script, and have verified that it worked, then you can safely install gems using the `gem install` command. For example:

```shell
gem install jekyll
```

You should then be able to use the gems right away:

```shell
jekyll -v
```

## How to switch between Ruby versions and install different versions

By default, the script installs the latest version of Ruby. To install an older version,
run `ruby-install` followed by `ruby-` and the desired version. For example:

```shell
ruby-install ruby-2.6.6
```

To switch to this newly-installed version, run `chruby` followed by the version. For example:

```shell
chruby 2.6.6
```

Another way to automatically switch between versions is to add a `.ruby-version` file in your Ruby project with the version number prefixed with `ruby-`, such as `ruby-2.7.2`. To test that this works:

1. `cd` into a folder outside of your project
2. Run `chruby 2.6.6` (or some other version that is not the one specified in your `.ruby-version`)
3. Verify that you are using 2.6.6 with `ruby -v`
4. `cd` into your project
5. Verify that you are using the specified version with `ruby -v`

Note that gems only get installed in a specific version of Ruby. If you installed jekyll in 2.7.2,
and then you install 2.6.6 for example, you'll have to install jekyll again in 2.6.6.

## Why chruby and not RVM or rbenv?

This script used `RVM` at first, but it started causing problems, so I switched to
`chruby`, and haven't had any issues since. `chruby` is also is the simplest, most
reliable, and easiest to understand. I like that it does not do some of the things
that other Ruby managers do:

- Does not hook `cd`.
- Does not install executable shims.
- Does not require Rubies to be installed into your home directory.
- Does not automatically switch Rubies by default.
- Does not require write-access to the Ruby directory in order to install gems.

Other folks who prefer `chruby`:

- <https://kgrz.io/programmers-guide-to-choosing-ruby-version-manager.html>
- <https://stevemarshall.com/journal/why-i-use-chruby/>
- <https://linhmtran168.github.io/blog/2014/02/27/moving-from-rbenv-to-chruby/>

## Debugging

Your last Laptop run will be saved to a file called `laptop.log` in your home
folder. Read through it to see if you can debug the issue yourself. If not,
copy the entire contents of `laptop.log` into a
[new GitHub Issue](https://github.com/monfresh/laptop/issues/new) (or attach the whole log file to the issue) for me and I'll be glad to help you. If the script doesn't work for you, it's most likely a bug in my script, which I'd love to fix, so please don't hesitate to report any issues.

## What it sets up

- [Bundler] for managing Ruby gems
- [chruby] for managing [Ruby] versions (recommended over RVM and rbenv)
- [GitHub CLI] brings GitHub to your terminal.
- [Heroku Toolbelt] for deploying and managing Heroku apps
- [Homebrew] for managing operating system libraries
- [Homebrew Cask] for quickly installing Mac apps from the command line
- [Homebrew Services] so you can easily stop, start, and restart services
- [Postgres] for storing relational data
- [ruby-install] for installing different versions of Ruby

[bundler]: http://bundler.io/
[chruby]: https://github.com/postmodern/chruby
[github cli]: https://cli.github.com
[heroku toolbelt]: https://toolbelt.heroku.com/
[homebrew]: http://brew.sh/
[homebrew cask]: http://caskroom.io/
[homebrew services]: https://github.com/Homebrew/homebrew-services
[postgres]: http://www.postgresql.org/
[ruby]: https://www.ruby-lang.org/en/
[ruby-install]: https://github.com/postmodern/ruby-install

It should take less than 15 minutes to install (depends on your machine and
internet connection).

## Customize in `~/.laptop.local` and `~/Brewfile.local`

```sh
# Go to your macOS user's root directory
cd ~

# Download the sample files to your computer
curl --remote-name https://raw.githubusercontent.com/monfresh/laptop/master/.laptop.local
curl --remote-name https://raw.githubusercontent.com/monfresh/laptop/master/Brewfile.local

# open the files in your text editor
open .laptop.local
open Brewfile.local
```

Your `~/.laptop.local` is run at the end of the `mac` script.
Put your customizations there. If you want to install additional
tools or Mac apps with Homebrew, add them to your `~/Brewfile.local`.
You can use the `.laptop.local` and `Brewfile.local` you downloaded
above to get started. It lets you install the following tools and Mac apps:

- [Firefox] for testing your Rails app on a browser other than Chrome or Safari
- [Flux] for adjusting your Mac's display color so you can sleep better
- [GitHub Desktop] for working with your repos using a GUI
- [iTerm2] - an awesome replacement for the macOS Terminal
- [Nova] - Panic's new macOS native code editor
- [Redis] for storing key-value data
- [Sublime Text 3] - a solid and fast code editor
- [Visual Studio Code] - Microsoft's popular code editor

[firefox]: https://www.mozilla.org/en-US/firefox/new/
[flux]: https://justgetflux.com/
[github desktop]: https://desktop.github.com/
[iterm2]: https://iterm2.com/
[nova]: https://nova.app/
[redis]: https://redis.io/
[sublime text 3]: https://www.sublimetext.com/3
[visual studio code]: https://code.visualstudio.com/

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo`, and `gem_install_or_update` can be used
in your `~/.laptop.local`.

## How to manage background services (such as Postgres)

The script does not automatically launch these services after installation
because you might not need or want them to be running. With Homebrew Services,
starting, stopping, or restarting these services is as easy as:

```
brew services start|stop|restart [name of service]
```

For example:

```
brew services start postgresql
```

To see a list of all installed services:

```
brew services list
```

To start all services at once:

```
brew services start --all
```

## Credits

This laptop script is inspired by
[thoughbot's laptop](https://github.com/thoughtbot/laptop) script.

### Public domain

thoughtbot's original work remains covered under an [MIT License](https://github.com/thoughtbot/laptop/blob/c997c4fb5a986b22d6c53214d8f219600a4561ee/LICENSE).

My work on this project is in the worldwide [public domain](LICENSE.md), as are contributions to my project. As stated in [CONTRIBUTING](CONTRIBUTING.md):

> This project is in the public domain within the United States, and copyright and related rights in the work worldwide are waived through the [CC0 1.0 Universal public domain dedication](https://creativecommons.org/publicdomain/zero/1.0/).
>
> All contributions to this project will be released under the CC0 dedication. By submitting a pull request, you are agreeing to comply with this waiver of copyright interest.
