How to Fully Uninstall the Official Docker OS X Installation
============================================================

No offense against Docker. I like the concept and the software!

– This guide is based on V1.3.0 of the installer –

But I absolutely do not like the official Docker OS X installer (install manual) which you can download here.

The reason for this are:

* It installs Virtualbox and has downgraded my existing Virtualbox Host software!
* It installs an app and several binaries.
* Everything packaged up into a .pkg and no uninstall app or instructions anywhere!

In summary it tries to do too much. Many developers use tools like Vagrant & Homebrew. Why not go that way?

### Uninstall steps for boot2docker / Docker

Be sure you’ve only used the official installer. This uninstall guide is not the right one if you have installed Docker with e.g. Homebrew or other methods.

First stop boot2docker and delete the VBox image:

    boot2docker stop
    boot2docker delete

Remove the environmental variable DOCKER\_HOST in case you have fixed it somewhere like e.g. in .bash\_profile

Drag and Drop the boot2docker app logo from applications into the trash can of OS X.

Remove Docker and boot2docker command line tools:

    sudo rm /usr/local/bin/docker
    sudo rm /usr/local/bin/boot2docker

Remove boot2docker VBox image:

    sudo rm /usr/local/share/boot2docker/boot2docker.iso
    sudo rmdir /usr/local/share/boot2docker

    rm -rf ~/.boot2docker

Remove boot2docker ssh keys:

    rm ~/.ssh/id_boot2docker*

Remove additional boot2docker files in /private folder:

    sudo rm -f /private/var/db/receipts/io.boot2docker.*
    sudo rm -f /private/var/db/receipts/io.boot2dockeriso.*

### (Optional) Uninstall steps for Virtualbox

You can also delete Virtualbox of course. But if you are a developer you probably need it anyway. In case your VBox got also downgraded: Reinstall Virtualbox.

If you really want to uninstall Virtualbox:

* Download the latest official Virtualbox installer.
* Open the DMG file and execute the uninstaller. Simple!
