## Requirements

Hardware requirements to install and operate CoreOS are pretty low - the OS itself has a minimal RAM and CPU footprint so most of the resources are let to run containers.

This makes it ideal for bare metal installations on low-power hardware like thin client for inexpensive 24/7 operations with 2GB, 4GB, 8GB, 16GB or even 32GB with 2 oder 4 CPUs.

low power CPUs like Intel J1900, N3150/N3160, J41x5, J5005 and N6005 are fully supported.

There is a very wide support for x86 hardware and network adapters - it usually deploys the latest production ready linux kernel to inportate latest device drivers and bug fixes.

long story short - it should run on almost every x86 hardware 

## a few more things you need to know

CoreOS comes in various installable version - one of which is the well know ISO to boot from CD or USB (or even iPXE).

However there is no interactive install wizzard to finish the installation - instead we need to prepare a (so-called "ignition") file to contain the minimal config settings for download via https - e.g.  key for SSH login for the standard user "core" - which do NOT have a standard password.

It is perfectly make to *automatically* spin up many bare metal or virtual machines in almost no time - just choose the suitable ignition file for your specific use-case - for bunkerweb we will use the minimal version but in general the ignition is very powerful.

CoreOS follows the "cattle" concept (see pets versus cattle) and always grabs the complete disk for its installation. It does NOT respect existing partition tables. The is no "co-existance" with other operating systems. It is made to provide one OS to bare metal (or virtual machines).

CoreOS does NOT come with a package manager - whatever software module or package is NOT included in the OS already should be in a container - and hence clearly seperated from OS files.

If a data partition is needed ... this has to go on a second / separate hard disk - luckily this can be a USB stick or drive, too. You will likely have one boot disk and one data disk.  

Every new OS version (usually monthly updates) is installed alongside already existing versions - so you can still boot an older version in case the last update is not working as expected

However, this method of always adding new thing on top of existing will sooner or later result in out-of-disk-space if no additional clean-up or purge of older versions is done.

## support

CoreOS looks like a simple (free) give-away from Fedora for which you can't expect much support - but I noticed when I filed by first bug report that it seems to have some importance and impact for RedHat and its professional enterprise services - just an observation by the number of people who jumped on that one bug report / without having any support contract.