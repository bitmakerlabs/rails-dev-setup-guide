# Mac Setup for Web Development Intensive

This is a guide for the Ruby/Rails version of the WDI course (2013 - 2018).  If you're looking for the current version of this installation guide, [head over here instead](https://github.com/bitmakerlabs/python-dev-setup-guide-mac).

This is Bitmaker's guide to setting up a development environment on a Mac for the Web Development Intensive course.  If you are using Linux (or need to install Linux in place of Windows), please refer to [this guide](https://github.com/bitmakerlabs/dev_environment_setup) instead.

If you have the choice, always choose a Mac for your development environment. It's just much easier, especially when starting out as a new developer. Linux works too, but the [instructions](https://github.com/bitmakerlabs/dev_environment_setup) will be quite different from what's described here. Using Windows for the course is not recommended at all.

This process should take about 1 hour to 1.5 hours to complete, depending on the speed of your machine and your internet connection.

## Preflight

This guide is written assuming you are running OS X El Capitan 10.11 or later. These instructions may still work for OS X 10.9 or 10.10, but is not guaranteed. You should upgrade to 10.11 before proceeding. We'll be waiting for you right here.

If you're a new developer and you haven't installed any development tools before, you can skip the next two paragraphs and go straight to the next step: [XCode and Command Line Tools](#xcode-and-command-line-tools).

If you already have some development tools installed, you'll have to adjust accordingly. If you use RVM or MacPorts, you'll need to fully uninstall those before continuing as they're incompatible with rbenv and Homebrew, which are our preferred tools.

This guide assumes that you're using bash shell, which is the default shell for the OS X Terminal. We also assume that you use `.bash_profile` to setup `$PATH` and other environment variables. If you use a different bash config file, be sure to substitute it where appropriate below.

## XCode and Command Line Tools

We need to install the tools that allow us to compile programs specifically for your machine. These are provided by Apple and are pretty easy to install.

Open the **Terminal** program. You can find it in the *Other* folder in Launchpad.

Enter the following command on the terminal command line.

```bash
xcode-select --install
```

It will pop up a dialog box like the following.

![XCode](assets/xcode.png)

Click **Install** to proceed. It is going to take a few minutes to complete.

## Alias ls

By default, when you view a listing of your files with the `ls` command, hidden files won't show up (i.e. files beginning with a '.'), nor will the details about each file appear. More often than not, we want to see those details, so let's `alias ls` to show these details everytime we use `ls`.

Enter the following from within your terminal:

```bash
echo "alias ls='ls -al'" >> ~/.bash_profile
source ~/.bash_profile
```

## Homebrew

Next we'll install [Homebrew](http://brew.sh/), a package manager which we'll use to install most of our other required command-line tools. It's a much more convenient alternative to compiling the code ourselves from source.

To install Homebrew, copy, paste and run the following at the command line:

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

You'll have to enter your Mac password to install Homebrew.

Once it's installed, run:

```bash
brew doctor
```

to make sure Homebrew installed correctly.

If brew doctor says `Your system is ready to brew`, then everything worked properly and you can skip the following step *"Your system is not ready to brew"* and proceed straight to [Try out Homebrew](#try-out-homebrew). Lucky you.

### Your system is not ready to brew

You might see something like this:

![Running brew doctor](assets/homebrew.png)

This is a problem. Let's fix this by moving the bin directory that Homebrew sets up for us ahead of every other folder specified in PATH. Run the following:

```bash
echo export PATH='/usr/local/bin:$PATH' >> ~/.bash_profile
```

This will create a `.bash_profile` config file which is read and executed each time a new terminal is opened. To apply changes made to this file, you can either restart terminal (ghetto mode), or run `source ~/.bash_profile`.

If you see other issues, try reading the instructions carefully and doing what they suggest. If `brew doctor` continues to issue warnings you can contact an instructor for help. You can also post a question in the #askusanything channel on Slack.

### Try out Homebrew

Try Homebrew out by installing `wget`:

```bash
brew install wget
```

In the future, run `brew update` to get the latest Homebrew formulas, and `brew upgrade` to update to the latest versions of installed applications.

You don't need to run these commands right now because you've already just installed the latest and greatest!

### An Aside: Why PATH Order is Important

Command-line executables are searched by going through each folder in the `$PATH` variable, one by one in the order listed. As soon as an app with the same name is found, it stops searching the rest of the folders. OS X comes with built-in apps (and you might have your own apps installed prior to this), but we often want to use newer versions instead. To make sure the newer version gets 'picked up', we need to ensure that the symlinked Homebrew /bin folder comes before other system folders. To see the PATH directories, run

```bash
echo $PATH
```

Homebrew packages are downloaded and installed in `/usr/local/Cellar/` by default, and symlinked into `/usr/local/bin`. This folder will not be overridden the next time Apple releases an incremental OS X update.

If you're a new developer and this section didn't make much sense to you, don't worry about it; you'll pick it up in the fullness of time.

### Food for Thought

* How do I get a list of Homebrew packages that are installable?
* How do I get a list of currently installed Homebrew packages?
* How do I update an existing package?

(You'll probably have to read the [Homebrew documentation](https://github.com/Homebrew/homebrew) to answer these questions.)

## Atom Editor

### Install Atom

Download [Atom](https://atom.io). Remember to drag the app from your `Downloads` folder into your `Applications` folder to install it into your system. Double click to launch it and take a look around.

Atom is the text editor we recommend for the course. Other alternatives are [Sublime Text 3](http://www.sublimetext.com/3) and [Textmate 2](https://macromates.com/download) if you ever feel like a change of pace.

### Atom Command-Line Tool

Atom comes with a command-line app called `atom`. We can open up files and folders from the command line using:

```bash
atom name_of_file_or_folder
```

Now that you've installed Atom, you might also want to set it as your default system editor. Much easier to use than `vim`!

```bash
echo 'export EDITOR="atom -w"' >> ~/.bash_profile
```

## rbenv

```bash
brew install rbenv
```

This installs [rbenv](https://github.com/sstephenson/rbenv), a lightweight tool to manage different versions of Ruby. OS X comes with an old version of Ruby, but we'll generally want to have our own, newer versions of it.

Installing rbenv will automatically install [ruby-build](https://github.com/sstephenson/ruby-build), a plugin for rbenv to conveniently install different versions of Ruby.

Now we need to modify our bash config. Copy and paste the following in your terminal.

```bash
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

Then close terminal and re-open it (restart Terminal) to apply the changes.

If you look at your path:

```bash
echo $PATH
```

It should look something like:

```
/Users/username/.rbenv/shims:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```

Reloading your config has inserted `/Users/username/.rbenv/shims` to the beginning of your `$PATH` variable. This is necessary for rbenv to work its magic.

## Ruby

To install ruby, first visit the [Ruby website](https://www.ruby-lang.org/en/downloads) and check to see what the latest version number is.  The version number should follow the format `2.X.X`, such as `2.5.1`.

```bash
rbenv install version_number_goes_here
```


This may take a while. Feel free to get a coffee while you wait.

After this finishes, you can setup your global (default) Ruby:

```bash
rbenv global version_number_goes_here
```

Make sure the right version is running:

```bash
ruby --version
```

Note that you can override this global setting per project. See [Choosing the Ruby Version](https://github.com/sstephenson/rbenv#choosing-the-ruby-version) for more information.

You can install older versions of Ruby this way as well if you ever find yourself working on a project that relies on an older version.

## RubyGems

RubyGems is a package management framework for Ruby. We use it to install Ruby apps such as Rails.

Run the following to update to the latest version:

```bash
gem update --system
```

Next run the following to install [Bundler](http://bundler.io/), which is required by Rails for managing your app's gems:

```bash
gem install bundler
```

## Postgres

PostgreSQL is awesome. Prefer it over MySQL if you have the choice.

The easiest way to install this is via [Postgres.app](http://postgresapp.com/). Download it, drag it to the applications folder, and then double-click to launch. While it's open, the database server is running (it adds an elephant icon to your taskbar). When you close it, the database server shuts down.

### Postgres Command-Line Tools

Install the Postgres command-line tools as follows:

```bash
echo 'export PATH=/Applications/Postgres.app/Contents/Versions/latest/bin:$PATH' >> ~/.bash_profile
```

Close and reopen Terminal, and then run:

```bash
psql --version
```

It should say something like:

```
psql (PostgreSQL) 9.5.4
```

For more information: [Postgres Documentation](http://postgresapp.com/documentation/)

## Rails

You need to have Rails installed in order to create new Rails projects. After the project is created (or if you're working with an existing Rails project), you'll be using the bundled versions of Rails specific to your project.

```bash
gem install rails
```

**Never run sudo in front of these gem commands**, or it may install to the wrong folder.

Rails might take a while to install. When the installation is done, you can look at your installed gems by running

```bash
gem list
```

Verify that Rails is there and that the version is 4.2.7. Now run:

```bash
rails --version
```

It should say `Rails 4.2.7`.

### Making a New Rails Project

You should create a directory where you can put all of your work. Run:

```bash
mkdir ~/Documents/work
```

This will create a `work` directory inside the OS X `Documents` folder. The words 'directory' and 'folder' are interchangeable. 'Folder' is the word generally used by non-technical people and 'directory' is the word generally used by technical people. Now that you are being initiated as techies, we'll use the 'directory' term!

The `~` symbol refers to your [Home Directory](http://superuser.com/questions/158721/what-does-mean-in-terms-of-os-x-folders-directories). You should put all of your work for this course inside the work directory, or some other directory of your choosing if you have another preference.

Go inside the work directory:

```bash
cd ~/Documents/work
```

and then make a new Rails project:

```bash
rails new my_awesome_app
```

This step will probably take a few minutes the first time you create a new Rails project. The next time, it'll run much faster.

### Running a Rails project

Next, go into your new project directory

```bash
cd my_awesome_app
```

then run:

```bash
bin/rails server
```

Visit `http://localhost:3000` in your browser. If you see the **Welcome aboard** page, congrats – Rails works!

You can type `ctrl + c` into your terminal to stop the Rails application.

In this course, you're going to be running the `bin/rails server` and `ctrl + c` commands very, very often, so go ahead and memorize them now!

### Editing a Rails project

Finally, you can open up the project files in Atom with the following command:

```bash
atom .
```

The `.` symbol means 'this directory', so the command means 'Open Atom in this directory'.

Poke around the project files as much as you like. Soon we'll be learning all about what makes a Rails app tick. Isn't it exciting?

Now, close the editor and continue to the next step.

### Delete the new Rails project

To keep your work directory clean, let's delete the new project you just created.

You are currently in your project directory. Check that by running:

```bash
pwd
```

It should say something like `/Users/username/Documents/work/my_awesome_app`.

Let's go back up one directory, to your work directory.

```bash
cd ..
```

Double check where you are:

```bash
pwd
```

It should say something like `/Users/username/Documents/work`.

Double check the contents of the directory

```bash
ls
```

If you've followed all the directions so far, there should only be single item called `my_awesome_app`. Go ahead and delete this project, it has served its purpose.

```bash
rm -r my_awesome_app
```

Be careful running this `rm -r` command! It's a command you need to learn, but always double and triple check what you're deleting. [More information on the rm -r command](http://stackoverflow.com/questions/29363445/what-is-exactly-doing-rm-r)

## Git

### Register on Github

First you'll need to setup your GitHub account.

Go to [github.com](http://github.com) and register a free account with your usual email address.

### Set up Git

git comes with OS X but it's typically an older version. Let's get a newer one.

```bash
brew install git
```

Then run:

```bash
git --version
```

It should say something like `git version 2.9.2`. The version number should be >= 2.9.

Next, tell Git your name so that your commits will be properly labelled. Substitute your actual name for `YOUR NAME`, of course.

```bash
git config --global user.name "YOUR NAME"
```

Now tell Git the email address that will be associated with your Git commits. This email address should be the same one that you registered with Github. Subtitute it for `YOUR EMAIL ADDRESS` below.

```bash
git config --global user.email "YOUR EMAIL ADDRESS"
```

More information: [Github documentation on setting up Git](https://help.github.com/articles/set-up-git#set-up-git)

### Generate SSH Keys

SSH keys are definitely a better setup to use with Git than HTTPS. This is so you don't have to type your password every time you push. Don't worry if these terms don't mean anything to you right now, you'll eventually learn what they are during your journey as a developer.

First let's make sure you don't already have existing SSH keys on your computer.

```bash
ls -la ~/.ssh
```

It should say something like `ls: /Users/username/.ssh: No such file or directory`.

With Terminal still open, run the following. Be sure to subtitute your Github email address.

```bash
ssh-keygen -t rsa -b 4096 -C "YOUR EMAIL ADDRESS"
```

When you are prompted to `Enter file in which to save the key`, just press **Enter** to continue and it will use the default filename `id_rsa`.

You'll then be asked to `Enter passphrase (empty for no passphrase)`. Enter a very good, secure passphrase (be sure that it's something you can remember).

Go ahead and memorize that passphrase now because it'll be needed again soon, in the *Test the Connection* step.

Next you'll be asked to `Enter same passphrase again`. Do so. You sould then see something like:

```
Your identification has been saved in /Users/user/.ssh/id_rsa.
Your public key has been saved in /Users/user/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:blw174u+1MfrpxaacBOqP4DLHyxUqcMPoOQ3+ijXOic anonymous@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
|                 |
|         .       |
|  . .   o   o    |
| o . o o   ..o   |
|  o o *.S .. ..  |
|   o o.B..o oo.. |
|  .. ...Bo o.+o.o|
|. Eoo oo....o..oo|
| oo=.  ....o+o++.|
+----[SHA256]-----+
```

And it means we can move onto the next step.

For more information: [Github documentation on generating SSH Keys](https://help.github.com/articles/generating-ssh-keys)

### Add your SSH Key to your Github Account

Copy the SSH key to your clipboard.

```bash
pbcopy < ~/.ssh/id_rsa.pub
```

Follow these steps to add the copied key to your Github account.

* In the top right corner of Github, click your profile photo, then click Settings.
* In the user settings sidebar, click **SSH keys**.
* Click **Add SSH key**.
* In the **Title** field, add a descriptive label for the new key. For example, if you're using a personal Mac, you might call this key "Personal MacBook Air".
* Paste your key into the **Key** field. (`ctrl-v`)
* Click **Add key**.

For more information: [Github documentation on adding your SSH Key to your account](https://help.github.com/articles/generating-ssh-keys/#step-4-add-your-ssh-key-to-your-account)

### Test the Connection

Open Terminal and enter:

```bash
ssh -T git@github.com
```

You will see something like:

```
The authenticity of host 'github.com (192.30.252.131)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)?
```

Type `yes` and then press **Enter**.

Next, a window like the following should pop up:

![SSH Agent](assets/sshagent.png)

Make sure the **Remember password in my keychain** box is checked.

Then type in the passphrase that you used to generate the SSH Key, and press **OK**.


If you see the following, then your Github account has been set up properly!

```
Hi username! You've successfully authenticated, but GitHub does not
provide shell access.
```

If you receive a message about "access denied," please see an instructor for help.


## Congratulations

Whew, you're done installing a working Rails development environment! Now take a break, you deserve it!
