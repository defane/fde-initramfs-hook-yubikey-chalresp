fde-initramfs-hook-yubikey-chalresp
===================================
The goal of the project is to bring YubiKey Challenge-Response support in full disk encryption on linux.
Tested on Debian 7 32bit and 64bit.

Features
--------
* Use YubiKey Challenge-Response to generate passphrase
* Fallback to enter directly a passphrase

Passphrase configuration
------------------------
By default, the script use the slot 2 for Challenge-Response.

Generate new passphrase:

    ykchalresp -2 "Your challenge"

Add the passphrase:

    cryptsetup luksAddKey /dev/sdXY

Installation
------------
Copy the files:

    cp bin/fdeyk_read_keyfile /usr/bin/fdeyk_read_keyfile
    cp initramfs-tools/hooks/yubikey-chalresp /etc/initramfs-tools/hooks/yubikey-chalresp
    cp initramfs-tools/scripts/local-top/yubikey-chalresp /etc/initramfs-tools/scripts/local-top/yubikey-chalresp

Ensure they are executable:

    chmod +x /usr/bin/fdeyk_read_keyfile
    chmod +x /etc/initramfs-tools/hooks/yubikey-chalresp
    chmod +x /etc/initramfs-tools/scripts/local-top/yubikey-chalresp

Modify the `/etc/crypttab` by adding `,keyscript=/usr/bin/fdeyk_read_keyfile,tries=1`:

    sdXY_crypt UUID=XXXXX none luks,keyscript=/usr/bin/fdeyk_read_keyfile,tries=1

Then, update the initramfs by the following command:

    update-initramfs -u

Reboot and enjoy!
