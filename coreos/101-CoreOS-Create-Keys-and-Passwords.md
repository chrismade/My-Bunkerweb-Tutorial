### CoreOS - prepare login keys

"security first" requires that login by default is key-based - not username/password (which is considered vastly insecure).
There is an option for username/password for the local console only - which is out of scope here

so the first thing to do is create a public/private keypair 

and then add the public key to a so-called ignition file which is used to automatically configure Fedora CoreOS during install.
Ignition file can be used to apply many different configuration settings - but we will only focus here for the login key as the bare minimum.

`ssh-keygen -f coreos`

which creates two files in the current directory

```
coreos
coreos.pub

```

First you may want to copy away the file "coreos" which contains the private key to a safe place.
