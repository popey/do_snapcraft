# do_snapcraft
Script to use Digital Ocean droplets to remotely build snaps using snapcraft. I made this so I can build in 'the cloud' and have my laptop CPU/IO freed up for watching important cat videos.

do_snapcraft uses [doctl](https://www.digitalocean.com/community/tutorials/how-to-use-doctl-the-official-digitalocean-command-line-client) to start a clean droplet. It installs snapcraft and uploads your snapcraft configuration and then runs snapcraft on the droplet. Once complete do_snapcraft triggers a notitifcation so you know it's finished.

# Pre-requisites

* A Digital Ocean account
 * Sign up using https://m.do.co/c/f9f96ea43bd3 to get $10 free credit
* An SSH key [configured](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets) in the digital ocean control panel
* [doctl](https://www.digitalocean.com/community/tutorials/how-to-use-doctl-the-official-digitalocean-command-line-client) command line tool in your path and correctly configured and authenticated to digital ocean

# Installation

* Copy do_snapcraft to your path such as ~/bin or /usr/local/bin

# Configuration

 * Edit do_snapcraft to set SSHKEY correctly for the key deployed to DO

# Using do_snapcraft

do_snapcraft requires only one parameter which is the name of the application, which it assumes is in a subdirectory from the current working directory.

So if ./myapp contains snapcraft.yaml and other associated snap configuration, source, plugins etc, then simply from the directory above myapp, run:-

    do_snapcraft myapp

Once finished, do_snapcraft leaves the droplet running. This is to enable the developer to ssh in to the droplet to debug the build. Leaving the droplet up will incur costs, so do_snapcraft echos the doctl command required to shut down the droplet.

The droplet name is randomly generated based on the app name and date and time. So if multiple people are using this in a digital ocean "team" then in theory there shouldn't be collisions of names of droplets.

# TODO

Suggestions for improvements are most welcome as this script was just hacked together quickly to service a personal need. However there's some specific things that could do with improving, including:-

* Only tested on Ubuntu 16.04 x86-64 running Unity. Some things may behave differently on other platforms. Testing/patches/issues welcome! Things to examine include:-
 * Perhaps using a shell other than bash
 * Notify-send (or equivalent) on non-Unity
 * Use of different file manager (nautilus command line is given as example)
* Detect that doctl isn't in the path, or not setup correctly
* If successfully built, copy the snap to the local machine automatically, rather than relying on the user to do it
* Perhaps add command line options for region and size of droplet to override the defaults of London and 512mb
* Perhaps point the droplet at a remote repo rather than upload from local machine
* Expand or fork to use other 'cloud' providers such as Amazon AWS
