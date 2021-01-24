# Laptop

[![Build Status](https://travis-ci.org/monfresh/laptop.svg)](https://travis-ci.org/monfresh/laptop)

Laptop is a script to set up a macOS computer for web development, and to keep
it up to date.

It can be run multiple times on the same machine safely. It installs,
upgrades, or skips packages based on what is already installed on the machine.

This particular version of the script is for those who want to
set up a complete Ruby web development environment on their Mac to build Rails apps and/or Jekyll sites for example. You can also easily [customize](#customize-in-laptoplocal-and-brewfilelocal)
the script to install additional tools.

For a more minimal script that only installs Ruby (with chruby and ruby-install), check out my [install-ruby-on-macos](https://github.com/monfresh/install-ruby-on-macos) script.

For more coding guides, scripts, and screencasts, subscribe to my [free weekly newsletter](https://www.moncefbelyamani.com/newsletter).

## Requirements

**I have not yet personally tested the script on Macs with the M1 chip, but others have said it worked for them both natively and with Rosetta. Let me know if you run into any issues.**

Supported operating systems:

- Big Sur (11.1)
- macOS Catalina (10.15)
- macOS Mojave (10.14)

Unsupported operating systems. Give it a shot, but I can't guarantee it will work.

- macOS High Sierra (10.13)
- macOS Sierra (10.12)
- OS X El Capitan (10.11)
- OS X Yosemite (10.10)
- OS X Mavericks (10.9)

Supported shells:

- bash
- zsh
- fish (see the note at the bottom of this README)

## Install
**IMPORTANT:** Before you run the script, make sure you have the latest Apple software updates installed. Check by going to System Preferences, then Software Update. If you're on Catalina, this does not mean upgrading to Big Sur, just the latest Catalina updates.

**Also, please make sure to read everything on this page for caveats, troubleshooting tips, and to make sure the script worked.**

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

## Debugging script failures

Your last `laptop` run will be saved to a file called `laptop.log` in your home
folder. Read through it to see if you can debug the issue yourself, with the help of the [Troubleshooting Errors](https://github.com/monfresh/laptop/wiki/Troubleshooting-Errors) Wiki article. If not,
copy the entire contents of `laptop.log` into a
[new GitHub Issue](https://github.com/monfresh/laptop/issues/new) (or attach the whole log file to the issue) for me and I'll be glad to help you. If the script doesn't work for you, it's most likely a bug in my script, which I'd love to fix, so please don't hesitate to report any issues.

## How to tell if the script worked

If the last thing the script displayed was "All done!", then everything the script was meant to do worked.

To verify that the Ruby environment is properly configured, quit and restart Terminal, then run these commands:

```shell
ruby -v
```

This should show `ruby 2.7.2` or `ruby 3.0.0`. If not, try switching manually to 2.7.2:

```shell
chruby 2.7.2
```

and check the version to double check:

```shell
ruby -v
```

Then check where Ruby is installed:

```shell
which ruby
```

This should point to the `.rubies` directory in your home folder. For example:

```
/Users/monfresh/.rubies/ruby-2.7.2/bin/ruby
```

## How to create a new Rails app

Once you've installed the script successfully and restarted your terminal, creating a new Rails app takes just 3 more steps:
https://www.moncefbelyamani.com/how-to-install-rails-and-create-a-new-rails-app-on-a-mac-the-easy-way/

## How to create a new Jekyll site

1. Create a folder to test with and navigate to it: 

```shell
mkdir ~/testing-jekyll && cd ~/testing-jekyll
```

2. Run `chruby 2.7.2`

3. Create a new Jekyll site:
```shell
jekyll new .
```
4. Run the server:
```shell
bundle exec jekyll serve
```

Read my guide for [creating a new Jekyll site and publishing it on GitHub Pages](https://www.moncefbelyamani.com/making-github-pages-work-with-latest-jekyll/) for more details.

## How to switch between Ruby versions and install different versions

The first time you run the script (assuming you didn't already have `chruby` installed), it will install Ruby 2.7.2, which is the version that is compatible with most gems at the moment. If you run the script again, it will check for newer versions, and it will install Ruby 3.0.0, which was released on December 25, 2020. You will still have Ruby 2.7.2. That's the advantage of using version managers like `chruby`. You can have many different versions installed at the same time and you can switch between them.

**Ruby 3.0 is still very new, so it's not yet fully compatible with gems like Rails or Jekyll. So, before you create a new Rails app or Jekyll site, make sure you're using Ruby 2.7.2. Keep reading for instructions.**

To check if you have Ruby 2.7.2 installed, run this command:

```shell
find "$HOME/.rubies" -maxdepth 1 -name 'ruby-2.7.2'
```
If nothing is returned, then you should install it:

```shell
ruby-install ruby-2.7.2
```

To switch to this newly-installed version, run `chruby` followed by the version. For example:

```shell
chruby 2.7.2
```
You should run `chruby 2.7.2` before you start any new project to make sure you are using the correct version of Ruby.

Another highly-recommended way to automatically switch between versions is to add a `.ruby-version` file in your Ruby project with the version number prefixed with `ruby-`, such as `ruby-2.7.2`. To test that this works:

1. `cd` into your Ruby project, such as your Rails app or Jekyll site
2. First, check to see if the file already exists: `cat .ruby-version`. If not, then create it in the next step.
2. Create a file called `.ruby-version` with `ruby-2.7.2` in it:
    ```shell
    echo 'ruby-2.7.2' >> .ruby-version
    ```
1. `cd` into a folder outside of your project, such as your home folder: `cd ~`
2. Run `ruby -v`. It will probably say `2.6.3p62`, which is the Ruby that came preinstalled on your Mac.
4. `cd` into your project
5. Verify that `ruby -v` shows `2.7.2p137`

Note that gems only get installed in a specific version of Ruby at a time. If you installed jekyll in 3.0.0,
and then you install 2.7.2 later, you'll have to install jekyll again in 2.7.2.

## Check the Node installation

To verify if Node was installed and configured:

```shell
node --version
```
You should see `v14.15.3` or later

```shell
nodenv help
```
You should see various commands you can run with `nodenv`.

## Next steps

The next thing you'll want to do after running the script is to [configure Git with your name, email, and preferred editor](https://www.moncefbelyamani.com/first-things-to-configure-before-using-git/).

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

## What it sets up

- [Bundler] for managing Ruby gems
- [chruby] for managing [Ruby] versions (recommended over RVM and rbenv)
- [GitHub CLI] brings GitHub to your terminal.
- [Heroku Toolbelt] for deploying and managing Heroku apps
- [Homebrew] for managing operating system libraries
- [Homebrew Cask] for quickly installing Mac apps from the command line
- [Homebrew Services] so you can easily stop, start, and restart services
- [Jekyll] for creating static sites
- [Nodenv] to easily install and manage Node versions
- [Rails] for creating modern web apps
- [Postgres] for storing relational data
- [ruby-install] for installing different versions of Ruby
- [Yarn] to manage JS dependencies

[bundler]: http://bundler.io/
[chruby]: https://github.com/postmodern/chruby
[github cli]: https://cli.github.com
[heroku toolbelt]: https://toolbelt.heroku.com/
[homebrew]: http://brew.sh/
[homebrew cask]: http://caskroom.io/
[homebrew services]: https://github.com/Homebrew/homebrew-services
[jekyll]: https://jekyllrb.com
[Nodenv]: https://github.com/nodenv/nodenv
[postgres]: http://www.postgresql.org/
[rails]: https://rubyonrails.org
[ruby]: https://www.ruby-lang.org/en/
[ruby-install]: https://github.com/postmodern/ruby-install
[yarn]: https://yarnpkg.com

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

If you want to skip running `.laptop.local`, you can set the `SKIP_LOCAL` environment variable to `true` before running `laptop`:

```shell
export SKIP_LOCAL=true
laptop
```

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

## Note about the fish shell

`chruby` does not support the fish shell out of the box. There is a `chruby-fish` tool, but it has bugs that manipulate the `PATH` in a way it shouldn't. This can prevent using Nodenv if Node is already installed with Homebrew. This is just one example of issues you might run into and not understand why things aren't working. Until the issues are fixed, here is a workaround you can use:

1. Uninstall chruby-fish if you have it: `brew uninstall chruby-fish`
1. Clone this fork of `chruby-fish` to your computer:
    ```shell
    git clone https://github.com/bouk/chruby-fish/tree/rewrite-fish
    ```
2. Check out the `rewrite-fish` branch:
    ```shell
    cd chruby-fish
    git checkout -b rewrite-fish origin/rewrite-fish
    ```
3. Run `make install`
4. Remove any lines from your fish config file that `source` chruby, such as:
    ```text
    source /usr/local/share/chruby/chruby.fish
    source /usr/local/share/chruby/auto.fish
    source /usr/local/share/chruby/chruby.sh
    source /usr/local/share/chruby/auto.sh
    ```
5. Quit and restart Terminal

See this pull request for more information: https://github.com/JeanMertz/chruby-fish/pull/39

## Credits

This laptop script is inspired by
[thoughbot's laptop](https://github.com/thoughtbot/laptop) script.

### Public domain

thoughtbot's original work remains covered under an [MIT License](https://github.com/thoughtbot/laptop/blob/c997c4fb5a986b22d6c53214d8f219600a4561ee/LICENSE).

My work on this project is in the worldwide [public domain](LICENSE.md), as are contributions to my project. As stated in [CONTRIBUTING](CONTRIBUTING.md):

> This project is in the public domain within the United States, and copyright and related rights in the work worldwide are waived through the [CC0 1.0 Universal public domain dedication](https://creativecommons.org/publicdomain/zero/1.0/).
>
> All contributions to this project will be released under the CC0 dedication. By submitting a pull request, you are agreeing to comply with this waiver of copyright interest.
