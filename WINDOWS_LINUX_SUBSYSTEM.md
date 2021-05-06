_DISCLAIMER We heavily recommend a minimal hardware configuration to consider this Windows Linux Subsystem setup: A SSD drive and at least 8Go RAM are mandatory_


# Setup instructions

The following instructions will help you to get ready for [Le Wagon](http://www.lewagon.org) fullstack bootcamp:

- Grab a text editor, where you'll spend your day and nights
- Install a package manager
- Pimp your Terminal
- Setup git and GitHub
- Install Ruby


## Prerequisites

First you need to check your machine runs on Windows 10 build 1615 or later. Follow theses steps to [check your build](https://docs.microsoft.com/en-us/windows/wsl/troubleshooting#check-your-build-number). For earlier versions of Windows 10, follow [Le Wagon Windows setup](WINDOWS.md).

### Ubuntu on Windows

Press `Windows key`, then type `powershell`. Right click on `Windows PowerShell (x86)` then click on `Run as administrator`
A blue window will open, run the following command in it:
```bash
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
The terminal will offer you to restart your computer, type `y` and press `Enter`.

Download [Ubuntu on Windows](https://www.microsoft.com/fr-fr/store/p/ubuntu/9nblggh4msv6?rtc=1) from the Windows Store. Once download has completed, select _Launch_. This will open a console window. Wait for installation to complete then you will be prompted to create your LINUX user account.

⚠ WARNING: ⚠ Your username should be **one** word, **downcased** and **no special character**. This user account has no relationship to your Windows username and password and hence can be different.

You're now ready to use your Linux environment 👏.

To open a terminal, press `Windows key`, then type `Ubuntu` (it might show up earlier, though), then `Enter`

You will have to copy-paste a lot of commands in this guide. While copying to the clipboard works as everywhere else by selecting the text, and pressing `Ctrl` + `C`, pasting into the terminal is done by right-clicking anywhere within the terminal window.

Start with this command that installs a few useful utilities that will be needed later:
```bash
sudo apt update
sudo apt install -y apt-transport-https unzip gnome-terminal
```

:point_up: This command will ask for your password with: `[sudo] password for <username>:`. The Linux terminal won't give you any feedback, like `\*`, as you might be used to. You just have to type it correctly, and then press `Enter`. If you type it wrong, you will be asked again otherwise the terminal will remember it until it is closed.


### Xming X Server for Windows

You need a X server to run your text editor through your Linux console in the context of your working directory.
Go to https://sourceforge.net/projects/xming/ and download Xming. Once download has completed, just launch it. You will see a Xming icon in the taskbar.

Xming does not autostart by default. To autostart it when you log into Windows, press `Windows key` + `R`, type `shell:startup`, and press `Enter`. Now drag an Xming icon (e.g. from your Desktop) into the window that just opened.


## Git

To install `git`, first open a terminal. To open a terminal, you can click on the Ubuntu Start button in the sidebar and type `Terminal`. Then click on the terminal icon.

Then copy this line with `Ctrl` + `C`:

```bash
sudo apt install -y git
```

## Oh-my-zsh - Fancy your Terminal

We will use the shell named `zsh` instead of `bash`, the default one.

```bash
sudo apt install -y zsh curl vim imagemagick jq
```

We need to install the latest version of nodejs:
```bash
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt install -y nodejs
```

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
# it will ask for your session password
```

Be careful, those commands will ask you to type your password twice. At the end
your prompt should look like this:

![](images/ubuntu_oh_my_zsh.png)

If it doesn't, **ask a teacher**.

To make this change stick, close your terminal and open it again.

## Git -> Azure

We need to generate SSH keys which are going to be used by GitHub and Heroku
to authenticate you. Think of it as a way to log in, but different from the
well known username/password couple. If you already generated keys
that you already use with other services, you can skip this step.

Open a terminal and type this, replacing the email with **yours** (the
same one you used to create your GitHub account). It will prompt
for information. Just press enter until it asks for a **passphrase**.

```bash
mkdir -p ~/.ssh && ssh-keygen -t ed25519 -o -a 100 -f ~/.ssh/id_ed25519 -C "TYPE_YOUR_EMAIL@HERE.com"
```

**NB:** when asked for a passphrase, put something you want (and that you'll remember),
it's a password to protect your private key stored on your hard drive. You'll type,
nothing will show up on the screen, **that's normal**. Just type the passphrase,
and when you're done, press `Enter`.

Then you need to give your **public** key to GitHub. Run:

```bash
cat ~/.ssh/id_ed25519.pub
```

It will prompt on the screen the content of the `id_ed25519.pub` file. Copy that text,
then go to [github.com/settings/ssh](https://github.com/settings/ssh). Click on
**Add SSH key**, fill in the Title with your computer name, and paste the **Key**.
Finish by clicking on the **Add key** green button.

To check that this step is completed, in the terminal run this. You will be
prompted a warning, type `yes` then `Enter`.

```bash
ssh -T git@github.com
```

If you see something like this, you're done!

```bash
# Hi --------! You've successfully authenticated, but GitHub does not provide shell access
```

If it does not work, try running this before trying again the `ssh -T` command:

```bash
ssh-add ~/.ssh/id_ed25519
```

Don't be in a rush, take time to [read this article](http://sebastien.saunier.me/blog/2015/05/10/github-public-key-authentication.html) to get a better
understanding of what those keys are used for.


## Dotfiles (Standard configuration)

Hackers love to refine and polish their shell and tools. We'll start with a great default configuration provided by [Dotfiles](http://github.com/gbs0/dotfiles), stored on GitHub. As your configuration is personal, you need your own repository storing it, so you first need to fork it to your GitHub account.


Forking means that it will create a new repo in your GitHub account, identical to the original one. You'll have a new repository on your GitHub account, `your_github_username/dotfiles`. We need to fork because each of you will need to put specific information (e.g. your name) in those files.

Open your terminal. **Don't blindly copy paste this line**, replace `replace_this_with_your_github_username` with *your*
own github usernickname.

```bash
export GITHUB_USERNAME=replace_this_with_your_github_username

# Example:
#   export GITHUB_USERNAME=ssaunier
```

Now copy/paste this very long line in your terminal. Do **not** change this one.

```bash
mkdir -p ~/code/$GITHUB_USERNAME && cd $_ && git clone git@github.com:$GITHUB_USERNAME/dotfiles.git
```

Run the `dotfiles` installer.

```bash
cd ~/code/$GITHUB_USERNAME/dotfiles
zsh install.sh
```

Then run the git installer:

```bash
cd ~/code/$GITHUB_USERNAME/dotfiles
zsh git_setup.sh
```

:point_up: This will **prompt** you for your name (`Firstname Lastname`) and your email.

Be careful, you **need** to put the **same** email as the one you sign up with on GitHub.

You need to prepend commands that start applications in a graphical interface outside the command line with `DISPLAY=:0 `, e.g. `DISPLAY=:0 subl`, or set this variable by adding it to `~/.bashrc`, i.e.
```bash
echo "export DISPLAY=:0" >> ~/.bashrc
echo "export DISPLAY=:0" >> ~/.zshrc
```

We also need to install a graphical library:
```bash
sudo apt install libgtk2.0-0
```

Copy the following command in your terminal to open the `~/.zshrc` profile file in Sublime Text:
```bash
stt ~/.zshrc
```
Locate the line `# Actually load Oh-My-Zsh` and **above it** write the following line:
```bash
ZSH_DISABLE_COMPFIX=true
```
Save the file and close Sublime Text.


Restart your terminal.

### Sublime Text auto-configuration

:warning: Be sure you have a Xming server running or launch it.

Open a new terminal and type this:

```bash
stt
```

It will **open Sublime Text in the context of your current folder**. That's how we'll use it.

_NB: You can ignore this warning message "st_run: nice(5) failed: operation not permitted"_

**Close Sublime text** and open it again:

```bash
stt
```

**Wait 1 minute** for additional packages to be automatically installed (New tabs with text will automatically open, containing documentation for each new package installed). TO follow package installation, you can go to `View > Show console`.

To check if plugins are installed, open the Command Palette (`⌘` + `⇧` + `P` on OSX, `Ctrl` + `⇧` + `P` on Linux), type in `Packlist` and then `Enter`, you should see a couple of packages installed (like [Emmet](http://emmet.io/)).

If you don't, please install all of them manually. The list is referenced [here](https://github.com/lewagon/dotfiles/blob/master/Package%20Control.sublime-settings).

When it's done, you can close Sublime Text.

### Auto-start `ssh-agent`

You don't want to be asked for your passphrase every time you communicate with a distant repository. So you need to add the plugin `ssh-agent` to `oh my zsh`.
First open `.zshrc` file:

```bash
stt ~/.zshrc
```

Spot the line starting with `plugins=` Then add `ssh-agent` to the plugins list. The list should look like:

```
plugins=(gitfast last-working-dir common-aliases sublime zsh-syntax-highlighting history-substring-search ssh-agent)
```

Save the `.zshrc` file with `Ctrl` + `S` and you can close Sublime Text.


## Postgresql

In a few weeks, we'll talk about SQL and Databases and you'll need something called Postgresql,
an open-source robust and production-ready database. Let's install it now.


```bash
sudo apt install -y postgresql postgresql-contrib libpq-dev build-essential
sudo /etc/init.d/postgresql start
sudo -u postgres psql --command "CREATE ROLE `whoami` LOGIN createdb;"
```

You can configure PostgreSQL to autostart, so you don't have to execute `sudo /etc/init.d/postgresql start` each time you open a new terminal:

```bash
sudo echo "`whoami` ALL=NOPASSWD:/etc/init.d/postgresql start" | sudo tee /etc/sudoers.d/postgresql
sudo chmod 440 /etc/sudoers.d/postgresql
echo "sudo /etc/init.d/postgresql start" >> ~/.zshrc
```


## Check-up

Let's check if you successfully installed everything.

Quit all opened Terminal, open a new one and run the following commands:

```bash
curl -Ls https://raw.githubusercontent.com/lewagon/setup/master/check.rb > _.rb && ruby _.rb || rm _.rb
```

It should tell you if your workstation is ready :) If not, ask a teacher.

