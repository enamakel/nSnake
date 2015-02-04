nSnake - tweaked
================

This is a modified version of the terminal snake game, tweaked to run an external program when the snake eat the fruit. It is made with C++ and ncurses.

`nsnake` is a clone of the classic snake game that we all used to play on our cellphones. You can play this game on the terminal, with textual interface.

Install
-------

### 1. Install Dependencies
Make sure you first have the nCurses dependencies installed, before you begin to compile the game

| Distro         | Installation command              |
| -------------- | --------------------------------- |
| Ubuntu/Debian  | `apt-get install libncurses5-dev` |
| Fedora         | `yum install ncurses-devel`       |
| Arch Linux     | _comes by default_                |


### 2. Setup source code
Download the source code from the git repo. Then compile and install.

    git clone --depth 1 https://github.com/enamakel/nSnake.git
    cd nSnake
    make
    sudo make install


### 3. Disabling current display manager
We want to disable the display manager from automatically loading during startup. This is because the display manager will get loaded by the snake game, which we will configure to load during startup.


#### 3.1 Finding your current display manager
You can figure out your current display manager by running the following command.

    cat /etc/X11/default-display-manager

#### 3.2 Disabling the display manager
Find your display manager's init script and disable it by appending `.dis` to it's filename. You will find the script inside the `/etc/init` folder.

So if your display manager was `/usr/sbin/lightdm` then look for `lightdm.conf` in the `/etc/init/` folder.

    sudo mv /etc/init/lightdm.conf /etc/init/lightdm.conf.dis

This effectively disables it from starting up during boot.

### 4. Enable TTY1 to load the snake game

We do this by modifying `tty1`'s startup script. So stay in the `/etc/init`folder and first make a backup of the `tty1.conf` file, if you have one.

    sudo mv /etc/init/tty1.conf /etc/init/tty1.conf.backup

Then create a new `/etc/init/tty1.conf` file with the following lines. Here I'm assuming that `/usr/sbin/lightdm` was my display manager. You should replace it with your own.

    start on stopped rc RUNLEVEL=[2345] and (
        not-container or
         container CONTAINER=lxc or
         container CONTAINER=lxc-libvirt)

    stop on runlevel [!2345]

    respawn

    exec /usr/bin/nsnake -e /usr/sbin/lightdm </dev/tty1 >/dev/tty1


### 5. Set automatic login in your display manager (Optional)
We want to do this, cause loading your desktop straight from a snake game is just too damn cool. Of course, this also removes the security of your password but that is something for you to think about.

I have listed a few links. They are instruction for the two most popular display managers. If there are more that you would like to add, please contribute to this page.

* GDM users, instructions [here](https://wiki.archlinux.org/index.php/GDM#Automatic_login)
* LightDM users, instructions [here](https://wiki.archlinux.org/index.php/LightDM#Enabling_autologin)


Contribute
----------
The modifications I have made with the code is a bit hacky. So if anyone would like to improve on that or add more, please do!

Also there are tons of cool things that can be done with this! If there is something that you have in mind, feel free to fork, implement and submit your pull request.


Credits
-------
This game is a fork of Alexandre Dantas's nSnake which can be found [here](https://github.com/alexdantas/nsnake). Credits go to him for generously open-sourcing his game so that we can do make like this.

We should all send him some thank you candy in his mail.