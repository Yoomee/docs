# Rails Development

An explanation of the main tools to setup a local Rails development environment for Rails 4 with Yoomee Gems and Engine Yard hosting.

This setup is sometimes referred to as **conan** to distinguish it from the Rails 3 environments that used shared code called **tramlines**.

## Install XCode

This is needed to install git, gcc and lots of other necessary
libraries etc.

1. Install from the Mac App Store.
2. Once installed open XCode, go to Preferences > Downloads and install Command Line Tools. Or try this on Mavericks:

```
$ xcode-select --install
```

## Install Homebrew

```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

Otherwise follow instructions at http://brew.sh/

## Install dependencies

Install these brew packages as they are dependencies for various dev tools and native gems further down the line:

```
brew install autoconf automake libtool pkg-config libyaml readline libxml2 libxslt libksba openssl qt
```

## Install RVM

Then install RVM:

```
curl -L https://get.rvm.io | bash -s stable --ruby
```

Then use RVM to install some other shit:

```
rvm get head
rvm autolibs enable
```

Now you can install some rubies. These are the ones that we use:

```
rvm install 1.8.7
rvm install 1.9.2
rvm install 1.9.3
```

We also need Ruby version 2.1.1 for newer projects:

```
rvm install ruby-2.1.1
```

If that complains, then try this:

```
rvm install ruby-2.1.1 --verify-downloads 1
```

You're almost done!

## Install MySQL

We use MySQL on Engine Yard, so install that now:

```
brew install mysql
```

```
mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp
```

Add to launchctl

```
mkdir -p ~/Library/LaunchAgents
```

```
cp /usr/local/Cellar/mysql/5.*/homebrew.mxcl.mysql.plist ~/Library/LaunchAgents/
```

```
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```

## Install ImageMagick

We use ImageMagick to mess with images:

```
brew install imagemagick
```

## Install Thinking Sphinx

Thinking Sphinx is a concise and easy-to-use Ruby library that connects ActiveRecord to the Sphinx search daemon, managing configuration and searching.

1. Download binaries from [here](https://gitlab.yoomee.com/yoomee/docs/raw/master/assets/binaries/sphinx_binaries.zip).
2. Unzip and move to /usr/local/bin

## Engine Yard

Add the following lines to .bash_login to make RVM work:

```
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*
source ~/.bashrc
```

Add the following lines to .bash_rc to make Engine Yard work:

```
PATH=$PATH:$HOME/.rvm/bin # Add RVM to PATH for scripting
export EYRC="./.eyrc"
```


Congrats! You're done.

## Other server tools

Some optional extras that you will probably need for other projects, so you may as well install now.

##### PostgreSQL

The easiest way to get started with PostgreSQL on the Mac is here: http://postgresapp.com/

## Other local tools

You might want some of these tools, or choose your own alternatives.

##### Sequel Pro

Use Sequel Pro for looking at your local MySQL database: http://www.sequelpro.com/

##### Atom Text Editor

Most of the dev team are using https://atom.io/ because David says so. It's very cool too.


##### GitHub

Most people seem to be using [GitHub Mac](https://mac.github.com/) for managing git commits.

##### Gitx

Some other people use Gitx for managing git commits.

1. Use _GitX-dev (rowanj fork)_ from http://rowanj.github.io/gitx/
2. Enable Terminal Usage from the GitX application menu.
