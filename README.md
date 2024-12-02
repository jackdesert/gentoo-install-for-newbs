Gentoo Install for Newbs
========================

A guide for using the menu-based base installer located at https://github.com/oddlama/gentoo-install


Alternatives
------------

https://github.com/sormy/gentoo-quick-installer/blob/mainline/gentoo-quick-installer.sh





Steps
-----

### 1. Create an archlinux bootable usb

### 2. Boot the archlinux bootable usb as UEFI

(Make sure you boot from UEFI, otherwise the menu seems to assume uefi is not supported)

Note this will land you at a terminal.
It is actually a full-functional linux terminal.
Meaning you can install packages. (`pacman` is the available package manager, which we will use to install git)
You can also mount additional thumb drives if you want to save your config file



Note once you boot into the archlinux bootable

### 3. (Optional) Start screen so you can scroll

### 4. Connect to wifi

    iwctl station wlan0 connect 'Name of your Network'
    # (It will ask for your network password)

### 5. Verify network

    curl example.com
    # (Make sure you get some html back)


### 6. Clone the oddlama/gentoo-install repo


    # Install git (note the "y" also updates pacman databases)
    pacman -Sy git
    # (It will ask for confirmation)

    # Clone the git repo
    git clone https://github.com/oddlama/gentoo-install



### 7. Run ./configure (OR copy a config file from USB)

    cd gentoo-install

    ./configure
    # (It will ask to install missing programs. Yes)
    # (It will also ask for confirmation. Yes)
    # (After that the menu will open)


### 8. Choose Menu Options

I UNchecked:

    LUKS Encryption      # for simple test
    Use Swap             # Because we have plenty of RAM
    Enable Bleeding Edge # For reliability
    Enable sshd          # Because we don't need other people logging in here

I changed "Device" to the SSD

NEED to change the network device.
    USE THIS: `en* wlan*`
    It appears to only accept one, although it says wildcards are okay..
    See 2.1.1 on this page for what can be used for "Name": https://wiki.archlinux.org/title/Systemd-networkd
    (It says whitespace separated is okay!!!)(And globs are okay!!!)

    Or https://www.man7.org/linux/man-pages/man5/systemd.network.5.html  :
           Name=
           A whitespace-separated list of shell-style globs matching the
           device name, as exposed by the udev property "INTERFACE", or
           device's alternative names. If the list is prefixed with a
           "!", the test is inverted.


    (I wonder if *n* would work to activate both wlan0 and ethernet)
    OR just put wlan0 in there, and later you can copy /etc/systemd/network/20-wired.network
    to a separate filename and change wlan0 to the ethernet.


NEED/WANT to add some packages: neovim, iwd
    At a minimum, add "iwd" so we can connect to wifi without needing to resort to a wired connection.

NEED to find out which additional packages we will want to get X.org working....
(or could leave that till later since we probably need some config too)

### 9. Save Config File

I saved to the default (gentoo.conf)

### 10. Exit the menu

### 11. Install

    ./install
    # (It will ask you if you want to install missing programs)
    # (It will ask for confirmation for the amount of downloads for installing missing programs)

Next it will ask "Do you really want to apply this disk configuration"
Note it will display in grey above which partitions it intends to create.

### After this, the rest of the install is hands-off until it asks for a root password right before finishing.

Creates partitions
formats the partitions
downloads the stage3 tarball
verifies the tarball integrity
mounts /tmp/root
extracts the tarball
performs global updates
emerges 13 things
Syncs repository "gentoo" into "/var/db/repos/gentoo"
Emerges 16 things



### Warning I Saw

    sys-kernel/linux-firmware not found installed on your system
    This package provides various firmware files that may be needed
    for your hardware to work. If in doubt, it is recommended
    to pause or abort the build process and install it before resuming.

    If you decide to install the linux-firmware later, you can rebuild
    the initramfs via issuing a command equivalent to:
        emerge --config sys-kernel/gentoo-kernel-bin:6.6.62

Here is a link to read about that:

https://wiki.gentoo.org/wiki/Linux_firmware

### Optional chrot

    "You may now reboot your system or execute ./install --chroot /tmp/gentoo-install/root to enter you system in a chroot.
    "Chrooting in this way is always possible in case you need to fix something after rebooting"



Runtime
--------

I started the install about 2pm. Maybe 1:50 (watch time)
Finished at 2:50.
So one hour. (the last step is the root passwd)


Whoops I was wrong, it does ask a questionn later.
about 2:45pm it asked "do you want to assign a root password now"?


Booting
--------

I hit F12 to get the custom boot menu
Then chose "UEFI Boot -> gentoo"

Only need to wait about ten seconds....it leaves some garbage on the screen,
but if you press ENTER it will ask you for a username/password.

It allowed me to log in as "root" with the password I supplied.


I want to install something like xfce.



### iwd

If you did not already install iwd with the inital install,
you will need to connect to wired ethernet. Then you can
install iwd.

    # Install iwd (if not already installed above)
    emerge --ask net-wireless/iwd

    # Enable and start iwd so you can use iwctl
    systemctl enable --now iwd

    # Connect to wifi (same as from the archlinux bootable usb)
    iwctl station wlan0 connect 'Name of your Network'
    # (It will ask for your network password)



How to save log file?
----------------------



Networking portion of the systemd docs
---------------------------------------

https://wiki.gentoo.org/wiki/Systemd#Network




How about this guide
---------------------

https://wiki.gentoo.org/wiki/Systemd/systemd-networkd

    systemctl enable --now systemd-networkd.service

After installing that ^^, you can run `networkctl` to see which interfaces are configured


How about asking LLM how to use systemd-networkd
------------------------------------------------0



New Features to Make Pull Requests for
--------------------------------------

1. echo all output to a logfile (withOUT the color codes)
2. echo commands run to a logfile
