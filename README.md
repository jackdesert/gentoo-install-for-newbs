Gentoo Install for Newbs
========================

A guide for using the [oddlama's menu-based base installer](https://github.com/oddlama/gentoo-install)


Steps
-----

### 1. Create an archlinux bootable usb

https://archlinux.org/download/

### 2. Boot into the archlinux bootable usb using UEFI

(Make sure you boot from UEFI, otherwise the menu seems to assume uefi is not supported)

Note this will land you in a fully-functional linux terminal. You can look around,
install packages, or even mount additional thumb drives if you want to save your config files.

### 3. (Optional) Start screen so you can scroll

If you want to be able to scroll through the STDOUT that is generated, start screen before continuing.


    screen

(Press ENTER after you get the wall of text)


### 4. Connect to WiFi

Run this command, substituting your actual network name:

    iwctl station wlan0 connect 'Name of your Network'
    # (It will ask for your network password)

### 5. Verify network

Make sure the network works, since we will need it for subsequent steps.

    curl example.com
    # (Make sure you get some html back)


### 6. Clone the [oddlama/gentoo-install](https://github.com/oddlama/gentoo-install) repo

Earlier I said you could install packages. `pacman` is the package manager
available to us from the archlinux live USB, so we'll use that.

    # Install git (note the `-y` flag also updates pacman databases)
    pacman -Sy git
    # (It will ask for confirmation)

    # Clone the oddlama/gentoo git repo
    git clone https://github.com/oddlama/gentoo-install


### 7. (Optional) Download the config file from this repo

If you would rather enter all values yourself, skip to Step 8 (Menu Config).

Otherwise you can run these commands to download the config file provided
with this repo. You will still need to at least set `Partitioning scheme -> Device`
since yours will be different from mine.

    # Clone this repo, which contains gentoo.conf
    git clone https://github.com/oddlama/gentoo-install-for-newbs

    # copy `gentoo.conf` into the gentoo-install directory
    cp gentoo-install-for-newbs/gentoo.conf gentoo-install



### 8. Menu Config


    # cd into the repo
    cd gentoo-install

    ./configure
    # (It will ask to install missing programs...Yes!)
    # (It will also ask for confirmation...Yes!)
    # (After that the menu will open)


### 9. Choose Menu Options

If you downloaded the config file from this repo, you still need to at least change
the value of `Partitioning scheme -> Device` to reference the drive you want to install to.

If you are starting from no config, you will need/want to make several changes.
Here is what I did, and what I recommend:

UNCHECK:
    "Use Swap"             # Because we have plenty of RAM
    "LUKS Encryption"      # Because simplicity
    "Enable Bleeding Edge" # For reliability
    "Enable sshd"          # Because we don't need other people logging in here

CHANGE:
    "Device" to be the actual drive you want to install to

CHANGE:
    "Timeozne" to where you live

CHANGE:
    "Interface Name" to `en* wlan*`
    (This makes it so both ethernet and WiFi can both use DHCP)
    (You can edit /etc/systemd/network/20-wired.network in your newly installed system to change this later)

CHANGE:
    "Additional Packages" to `app-editors/neovim  net-wireless/iwd`
    (This installs neovim and `iwctl` so you can at least edit files and connect to WiFi on your new system)

CHANGE:
    "Gentoo Mirror" to one that is close to me. YMMV

### 10. Save Config File

I saved to the default filename (gentoo.conf)

### 11. Exit the menu

### 12. Install

    ./install
    # (It will ask you if you want to install missing programs. Yes!)
    # (It will ask for confirmation for the amount of downloads for installing missing programs. Yes!)

Next it will ask "Do you really want to apply this disk configuration"
Note it will display in grey above which partitions it intends to create.

The speed of downloading the stage3 tarball varies depending on your chosen mirror and geography.
Once that step is passed, expect about an hour for the remaining compilation and setup.

Finally, it will ask if you want to set a password for root...Yes!

Installation complete!


Booting Your New System
-----------------------

### Boot and Login

Remove the archlinux thumb drive.

Reboot your computer.

Once it prints some stuff to the screen and then stops, press ENTER to see the prompt
for your username/password.

Enter `root` for the user and the password you chose above.


### Connect WiFi

This assumes that you installed `iwd` as one of the "additional packages"


    # Enable and start iwd so you can use iwctl
    systemctl enable --now iwd

    # Connect to wifi (same as you did from the archlinux Live USB)
    iwctl station wlan0 connect 'Name of your Network'
    # (It will ask for your network password)

    # Verify network
    curl example.com
    # (Expect some html in response)
