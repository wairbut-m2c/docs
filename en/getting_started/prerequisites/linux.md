# Configuration for development and test environments (GNU/Linux)

## Git

Git is officially maintained in Debian/Ubuntu:

```
sudo apt-get install git
```

## Ruby

Ruby versions packaged in official repositories are not suitable to work with consul (at least Debian 7 and 8), so we'll have to install it manually.

The preferred method is via rvm:

(only the multi user option installs all dependencies automatically, as we use 'sudo'.)

### As local user

```
curl -L https://get.rvm.io | bash -s stable
```

### For all system users

```
curl -L https://get.rvm.io | sudo bash -s stable
```

and then add your user to rvm group

```
sudo usermod -a -G rvm <user>
```

and finally, add rvm script source to user's bash (~/.bashrc) (this step it's only necessary if you still can't execute rvm command)

```
[[ -s /usr/local/rvm/scripts/rvm ]] && source /usr/local/rvm/scripts/rvm
```

with all this, you are suppose to be able to install a ruby version from rvm, as for example version 2.3.0:

```
sudo rvm install 2.3.0
```

## Bundler

with

```
gem install bundler
```

or there is more methods [here](https://rvm.io/integration/bundler) that should be better as:

```
gem install rubygems-bundler
```

## Node.js

To compile the assets, you'll need a JavaScript runtime. Node.js is the preferred option. As with Ruby, we don't recommend installing Node from your distro's repositories.

To install it, you can use [n](https://github.com/tj/n)

Run the following command on your terminal:

```
curl -L https://git.io/n-install | bash -s -- -y lts
```

And it will install the latest LTS (Long Term Support) Node version on your `$HOME` folder automatically (This makes use of [n-install](https://github.com/mklement0/n-install))

## PostgreSQL (>=9.4)

PostgreSQL version 9.4 is not official in debian 7 (wheezy), in 8 it seems to be officially maintained.

So you have to add a repository, the official postgresql works fine.

Add the repository to apt, for example creating file */etc/apt/sources.list.d/pgdg.list* with:

```
deb http://apt.postgresql.org/pub/repos/apt/ wheezy-pgdg main
```

afterwards you'll have to download the key, and install it, by:

```
wget https://www.postgresql.org/media/keys/ACCC4CF8.asc
apt-key add ACCC4CF8.asc
```

and install postgresql

```
apt-get update
apt-get install postgresql-9.4
```

## ChromeDriver

To run E2E integration tests, we use Selenium along with Headless Chrome.

On Debian-based distros, the process to get ChromeDriver up and running is not as straightforward as on Mac OS.

To get it working, first install the following packages:

```bash
sudo apt-get update && sudo apt-get install libxss1 libappindicator1 libindicator7 unzip
```

Then you need either Google Chrome or Chromium installed, both are valid.

You can download the former from [here](https://www.google.com/chrome/index.html), while the latter can be installed with the following command:

```bash
sudo apt-get install chromium
```

You can now proceed to install ChromeDriver. First, check out its latest version [here](https://sites.google.com/a/chromium.org/chromedriver/)

Download it the following way:

```bash
wget -N http://chromedriver.storage.googleapis.com/2.37/chromedriver_linux64.zip
```

Unzip it and make it executable like this:

```bash
unzip chromedriver_linux64.zip
chmod +x chromedriver
```

Finally, add the binary to your `$PATH`:

```bash
sudo mv -f chromedriver /usr/local/share/chromedriver
sudo ln -s /usr/local/share/chromedriver /usr/local/bin/chromedriver
sudo ln -s /usr/local/share/chromedriver /usr/bin/chromedriver
```

Make sure everything's working as expected by running the following command:

```bash
chromedriver --version
```

You should receive an output with the latest version of ChromeDriver. If that's the case, you're good to go!

If you happen to be on an Arch-based distro, installing `chromium` from the `extra` repo will do.

There's also the option to only install ChromeDriver from AUR. If you're using `pacaur`, this will do:

```bash
pacaur -S chromedriver
```

> Now you're ready to go get Consul [installed](../installation.html)!!
