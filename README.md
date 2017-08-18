# What to do after a fresh install of Xubuntu?

![How to secure your laptop](https://raw.githubusercontent.com/rpellerin/dotfiles/master/Pictures/secure-laptop.png)

1. Upgrade the bios by downloading the latest image from [Dell.com](http://www.dell.com/support/home/us/en/19/product-support/product/latitude-14-7480-laptop/drivers?os=biosa). Then:

    ```bash
    sudo mv Downloads/Latitude_7x80_1.4.6.exe /boot/efi
    ```


    Reboot, hit F12 to initiate the update. Once done, reboot and press F2 to enter BIOS setup. Set a password for the BIOS and the hard drive.
    Alternatively, you can try to download the image from [this website](https://secure-lvfs.rhcloud.com/lvfs/devicelist) and install it through "Software" (simply open the file).

2. Download Chrome .deb file and then:

    ```bash
    sudo dpgk -i google-chrome-stable.deb
    sudo apt install -f # To fix dependencies problems
    ```

    Also download Firefox Nightly and set it up correctly:
    
    - [Setting TLS minimum to 1.1 for example](https://support.mozilla.org/fr/questions/1103968)
    - [Extensions](https://blog.imirhil.fr/2015/12/08/extensions-vie-privee.html)
    - [More extensions](https://amiunique.org/tools)
    - [Even more extensions](http://sebsauvage.net/wiki/doku.php?id=firefox)

3. Install many things:

	```bash
	sudo apt-add-repository ppa:git-core/ppa

	sudo apt update
	sudo apt upgrade

	sudo apt install \
        git \
        xclip \
        autojump \
        ctags \
        tree \
        ntp \
        imagemagick \
        optipng \
        texlive-full \
        texlive-bibtex-extra \
        openjdk-8-jdk \
        openjdk-8-doc \
        inotify-tools \
        arandr \
        gcolor2 \
        vlc \
        gksu \
        gigolo \
        gedit \
        i3lock \
        p7zip-full \
        build-essential \
        gimp \
        curl \
        ffmpeg \
        vim-gtk \
        python3 \
        zsh \
        libreoffice \
        libreoffice-l10n-fr \
        libreoffice-help-fr \
        hyphen-fr \
        hunspell-en-gb \
        thunderbird-locale-en-gb \
        libreoffice-l10n-en-gb \
        libreoffice-help-en-gb \
        hyphen-en-gb

	# vim-gtk for clipboard support
    # ctags is for vim tag jumping (see .vimrc)
    # Install biber from http://biblatex-biber.sourceforge.net/ (sudo cp biber /usr/local/bin)
    # See https://bugs.launchpad.net/ubuntu/+source/biber/+bug/1565842

    # Tmux
    sudo apt install libevent-dev libncurses-dev pkg-config automake autoconf
    git clone https://github.com/tmux/tmux.git
    cd tmux
    sh autogen.sh
    ./configure && make
    sudo make install

    # NodeJS
	curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
	sudo apt install nodejs # And read https://docs.npmjs.com/getting-started/fixing-npm-permissions
    # Yarn (better alternative to npm)
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
    sudo apt update && sudo apt install yarn

    crontab -e
    */5 * * * * /usr/bin/node /home/romain/git/dotfiles/scripts/getWeather.js > /tmp/weather.txt

	sudo apt install -f # To fix problems

    # ZSH + Prezto
    zsh
    # Press the 'q' key
    git clone --recursive https://github.com/sorin-ionescu/prezto.git "${ZDOTDIR:-$HOME}/.zprezto"
    setopt EXTENDED_GLOB
    for rcfile in "${ZDOTDIR:-$HOME}"/.zprezto/runcoms/^README.md(.N); do
      ln -s "$rcfile" "${ZDOTDIR:-$HOME}/.${rcfile:t}"
    done
    chsh -s /bin/zsh # Might need rebooting to take effect

    cd dotfiles # cd to this git repo
    REPO_DIR=`pwd`
    
    # Install the firewall
    sudo cp -i $REPO_DIR/scripts/firewall.sh /etc/init.d/
    sudo chmod 700 /etc/init.d/firewall.sh
    sudo chown root:root /etc/init.d/firewall.sh
    sudo update-rc.d firewall.sh defaults

    # Custom settings
    ln -sf $REPO_DIR/.vimrc $HOME/
    mkdir -p $HOME/.vim
    for file in .vim/**/*.*; do
        ln -sf $REPO_DIR/`dirname $file` $HOME/.vim
    done
    echo "source $REPO_DIR/.rc" >> $HOME/.zshrc
    echo "source $REPO_DIR/.aliases" >> $HOME/.zshrc
    ln -sf $REPO_DIR/.tmux.conf $HOME/
    cp -i "$REPO_DIR/Pictures/pause.png" $HOME/Pictures/pause.png
    ln -sf "$REPO_DIR/.gitconfig" $HOME/
    ln -sf "$REPO_DIR/.git-templates" $HOME/
    ln -sf "$REPO_DIR/.gitignore_global" $HOME/
    ln -sf $REPO_DIR/.curlrc $HOME/
    ln -sf $REPO_DIR/.less $HOME/
    ln -sf $REPO_DIR/.lesskey $HOME/
    ln -s $REPO_DIR/.ycm_extra_conf.py $HOME/
    ln -s $REPO_DIR/.tern-project $HOME/
    ln -s $REPO_DIR/.eslintrc.js $HOME/
    diff $REPO_DIR/.zpreztorc $HOME/.zprezto/runcoms/zpreztorc
    mkdir -p $HOME/.gradle
    cp $REPO_DIR/.gradle/gradle.properties $HOME/.gradle/

    # Set up Vim
    ## Vundle (Vim package manager)
    git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
    ### Then go to https://github.com/VundleVim/Vundle.vim
    ## Install YouCompleteMe by reading https://github.com/Valloric/YouCompleteMe/blob/master/README.md#ubuntu-linux-x64 (no need to read the "Full Installation Guide" section; if you alreadt have Clang on your system, your might use the option `--system-libclang`)
    ## Finally, deactivate your firewall (just in case), launch Vim and run:
    :PluginInstall
	```

4. Check *Additional Drivers* in *Settings* to make sure all devices are being used.

5. Set up the xfce panel (top bar): show the battery indicator (if on a laptop), set the date, time and timezone, sync the time with the Internet. Add network, RAM and CPU monitor.

6. Go through all the settings, in the *Settings Manager*.

    - More importantly, set the keyboard shortcuts (*Tile window to the x*, *Show desktop*).
    - Also, change the DNS servers to those from FDN (http://blog.fdn.fr/?post/2014/12/07/Filtrer-The-Pirate-Bay-Ubu-roi-des-Internets).
    - Set up your `/etc/hosts`: [blocklists](https://github.com/jmdugan/blocklists) and [how to make the internet not suck (as much)](http://someonewhocares.org/hosts/)
    - Finally, in *Keyboard*, bind the command `i3lock -i /home/romain/Pictures/pause.png -n -t` with *Ctrl+Alt+Delete*.

7. Set up **Thunderbird**. Most of the time, you can import the directory *~/.thunderbird* (except the directory *Crash Reports*, inside, maybe) from another computer.

8. Install the [ownCloud client](https://software.opensuse.org/download/package?project=isv:ownCloud:desktop&package=owncloud-client).

9. You shoud install f.lux and launch it at startup (Menu>Settings>Session and startup): http://doc.ubuntu-fr.org/f.lux#installation_manuelle

10. Make sure nothing happens when you close the lid (in both plugged mode or battery mode): no sleep mode, no turning off.

11. [Finally, you might want to protect your privacy even further](http://blog.romainpellerin.eu/yes-privacy-matters.html) (read the section "Further Reading").

## Optional stuff

### Android

Pay attention while installing the Android SDK, it requires extra Debian packages, as stated on the download page. You'll also need to install `lib32stdc++6` and `lib32z1`.

Then:

    ```bash
    sudo ln -s /home/romain/android-studio/bin/studio.sh /usr/local/bin/studio
    sudo ln -s /home/romain/Android/Sdk/tools/android /usr/local/bin/android
    ```

### Python 3.5

If you download Python3 using your package manager (as seen above), you'll likely get a quite old version. If you want the latest one, here's how to do it:

1. Download it from [https://www.python.org/downloads/](https://www.python.org/downloads/)
2. Extract the tar ball and `cd` to the directory.
3. Then:

    ```bash
    sudo apt install libssl-dev openssl libsqlite3-dev
    ./configure --enable-loadable-sqlite-extensions --with-ensurepip=install
    make profile-opt
    make test
    sudo make install
    ```

### Optional Python packages

Installable with `pip install <package>`.

- `eg` useful examples of common commands
- `gitpython` an API for GitHub

### TeamViewer

```bash
wget http://download.teamviewer.com/download/teamviewer_linux.deb -O /tmp/teamviewer.deb
sudo dpkg -i /tmp/teamviewer.deb
# You might need to run dpkg --add-architecture i386 before the previous command
sudo apt update # Required to solve dependencies involving i386 packages
sudo apt install -f
rm /tmp/teamviewer.deb -f
```

### Haskell & Pandoc

```bash
sudo apt install haskell-platform
# http://pandoc.org/installing.html
cabal update
cabal install pandoc --enable-tests
cabal install pandoc-citeproc
```

### Optional Debian/Ubuntu packages

- `exiftool` for EXIF data
- `jhead` for EXIF data
- `filezilla`
- `atom (go to official website to download the .deb file)`
- `zenity` a simple interactive dialog
- `icoutils` to create Microsoft Windows(R) icon and cursor files
- `zathura` a PDF viewer
- `wireshark`
- `htop`
- `synaptic` see http://askubuntu.com/questions/76/whats-the-difference-between-package-managers
- `gtk-recordmydesktop`
- `pdf-presenter-console`
- `openvpn`
- `network-manager-openvpn-gnome` (then `sudo service network-manager restart`)
- `network-manager-vpnc`
- `codeblocks`

### Optional npm packages

Installable with `npm install -g <package>`.

- `eslint` Linter for JavaScript, read [http://eslint.org/docs/user-guide/getting-started](http://eslint.org/docs/user-guide/getting-started)
- `cloc` Count Lines of Code
- `sloc` Source Line Of Code (same stuff)
- `gnomon` Utility to annotate console logging statements with timestamps
- `livedown` Live preview of Markdown files
- `peerflix` Download torrent and watch them as they are being downloaded
- `castnow` Play media files on Chromecast (subtitles supported)
