Gentoo Install for Newbs
========================

A guide for using the menu-based base installer located at https://github.com/oddlama/gentoo-install


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


### 4. Connect to wifi

    iwctl station wlan0 connect 'Name of your Network'
    # (It will ask for your network password)

### 5. Verify network

Make sure the network works, since we will need it for subsequent steps.

    curl example.com
    # (Make sure you get some html back)


### 6. Clone the oddlama/gentoo-install repo

Earlier I said you could install packages. `pacman` is the package manager
available to us from the archlinux live USB, so we'll use that.

    # Install git (note the "y" also updates pacman databases)
    pacman -Sy git
    # (It will ask for confirmation)

    # Clone the oddlama/gentoo git repo
    git clone https://github.com/oddlama/gentoo-install


### 7. Decide whether to generate your own config

If you wish to generate your own config, CONTINUE FROM STEP 8, "Menu Config"

Or if you would rather use the config file provided by gentoo-install-for-newbs, run these
command and then CONTINUE FROM STEP 12, "Install"

    # Clone this repo
    git clone https://github.com/oddlama/gentoo-install-for-newbs

    # copy `gentoo.conf` into the gentoo-install directory
    cp gentoo-install-for-newbs/gentoo.conf gentoo-install

    # cd into the gentoo-install directory
    cd gentoo-install

    # CONTINUE FROM STEP 12, "Install"


### 8. Menu Config

If you are here, you have decided to generate your own config file using the menu config.

    # cd into the repo
    cd gentoo-install

    ./configure
    # (It will ask to install missing programs. Yes)
    # (It will also ask for confirmation. Yes)
    # (After that the menu will open)


### 9. Choose Menu Options

Here is what I changed from the default settings when creating the gentoo.conf provided in this repo:

UNchecked:
    LUKS Encryption      # Because simplicity
    Use Swap             # Because we have plenty of RAM
    Enable Bleeding Edge # For reliability
    Enable sshd          # Because we don't need other people logging in here

CHANGED:
    "Device" to be my actual SSD

CHANGED:
    "Network Device" to `en* wlan*` (without quotes)
    (This makes it so both ethernet and wifi can both use DHCP)
    (You can edit /etc/systemd/network/20-wired.network in your newly installed system to change this later)

CHANGED:
    "Additional Packages" to `app-editors/neovim  net-wireless/iwd` (without quotes)
    (This installs neovim and `iwctl` so you can at least edit files and connect to wifi on your new system)

CHANGED:
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


### Connect Wifi

This assumes that you installed `iwd` as one of the "additional packages"


    # Enable and start iwd so you can use iwctl
    systemctl enable --now iwd

    # Connect to wifi (same as you did from the archlinux Live USB)
    iwctl station wlan0 connect 'Name of your Network'
    # (It will ask for your network password)

    # Verify network
    curl example.com
    # (Expect some html in response)
