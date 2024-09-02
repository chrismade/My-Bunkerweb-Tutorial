## Fedora CoreOS download 

go to 

https://fedoraproject.org/

and scroll down on that page - Fedora CoreOS should appear in the list of various options and flavors - likely at the very end of the page

click "download now" and pay attention that the version is "stable" and your architecture is likely "x86_64" and "bare metal" - "Live DVD ISO"
is the correct version if you are operating on own hardware - and not in any of the cloud environments.

please remember in which folder exactly this ISO file was downloaded

## CoreOS comes with podman installed

The easiest way to install podman is to install a host operating system in which it is already included by default - like CoreOS (nowadays also referred as
FCOS = Fedora Core OS). As the name indicates it is now a variant of Fedora to provide a secure minimal Linux designed to run container workloads (with podman by default) -
CoreOS is also used by RedHat Openshift - https://www.redhat.com/en/technologies/cloud-computing/openshift/what-was-coreos

Key features are a minimized Linux with current kernel and fixes and hence low attack surface. After installation most parts are read-only and updates are
applied to a layered filesystem (like in containers itself) - this makes rollback easy in case of issues and ensures integrity of (most of the) code.
And the built-in updater service (zincati) installs updates automatically usually once or twice a month - and can be paused in case of issues.

You will probably miss a graphically user interface or a package manager which allows to install more packages - both are against the basic concepts.
E.g. if a python3 runtime is needed then a container containing python3 is spin up to do the processing - instead of installing python3 into the OS itself.  
Even for debugging in CoreOS itself it uses a special debug container. It is cleaner - and easier to get rid of things which are no longer needed.

## alternatives to Fedora CoreOS

There is a very similar alternative to Fedora (IBM RedHat) CoreOS which is "flatcar Linux" by "kinvolk.io" (now a Microsoft(!) company). Note that this guide has not been tested on any of these alternatives.

## to-do

check if there are more alternatives to CoreOS e.g. from SUSE

### CoreOS - install

CoreOS is made for *server* use-cases - here is a word of warning:

**CAUTION!!!** - the installation procedure will always grab the full hard disk and overwrite existing content without any further warning.
It is *NOT* made for co-existence with other operating systems like Windows, Linux or MacOS which you know from desktop setups.

There are two easily ways to deal with this "special feature"

* Use a type1 hypervisor like Proxmox, Hyper-V or ESXi - or at least VirtualBox or VMware player/workstation - to provide a virtual machine with separated virtual disks
* Use a seperate hardware PC or notebook which does NOT contain anything (anymore) that is still needed

In a hypervisor / virtual target machine you can configure to use the ISO as virtual DVD / CDROM -

In case of a real hardware (called: bare metal) you need to burn the ISO file onto a DVD (not recommended) or
make a raw copy to an USB device (recommended) - to make such a bootable USB stick use a tool like "rufus" under windows - or in Linux it is

`dd bs=100M if=filename.iso of=/dev/sdX`

where `/dev/sdX` is usually a device like `/dev/sde` - you can insert the USB stick into the machine and call `sudo dmesg` and at the end of the log you 
can easily see which device was last inserted into the system.
 
Next power on your virtual - or real hardware - machine. You should see the "Fedora CoreOS" boot screen 
which automatically boots after a few seconds. Wait for the boot to complete - which is when it allows you to enter commands like `ip a`
to check the network configuration.

## install podman on a different Host OS 

In case you decide not to install CoreOS (or "flatcar Linux") to get a podman runtime you can still install it into any major Operating Systems (OS).
Maybe your current host OS (debian, ubuntu, etc) have already a podman package to install via your standard package manager easily - but note that these packages may contain an older version -
"podman" is actively developed and new features and versions are issued frequently.
Please go to https://podman.io or use your favorite search engine to find recent installation packages.



### CoreOS - shutdown after install

As printed on the screen the installer to complete the installation requires to enter:

`sudo coreos-installer install /dev/sda --ignition-url https://YOUR.URL/config.ign`

if you use github.com as webserver then it would look like this:

`sudo coreos-installer install /dev/sda --ignition-url https://raw.githubusercontent.com/YOUNAME/main/ignition/config.ign`

after the installation is complete then shutdown the server with 

`init 0`

### CoreOS - first boot after install

remove the USB stick from the server - or remove CD/DVD ISO from the virtual machine config - and start the server again.

This boot should start as before but now shows a 'login' message - do not try to login here, it is useless because we have not configured a password.
Instead, use ssh to login via network - you should also see the IP address of this server on the console - then go to another machine which has the private(!) key
file 'coreos' and login over network via key:

`ssh -i coreos IP-ADDR`

where IP-ADDR is the IP address shown on the CoreOS boot screen e.g. 192.168.55.34