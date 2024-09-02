### CoreOS - prepare ignition file for basic config

To continue in the installation of CoreOS we need to prepare a so-called 'ignition' file (find a version *without* key content in folder 'ignition').
Then the *public* key must be added to the ignition file (copy the full file content in [ ] at "sshAuthorizedKeys" )

```

{
  "ignition": {
    "version": "3.2.0"
  },
  "passwd": {
    "users": [
      {
        "groups": [
          "docker",
          "wheel",
          "sudo"
        ],
        "name": "core",
        "sshAuthorizedKeys": [
          "ssh-rsa AAAAB3NzaC1y...your-public-key-in-this-line...er7Q7/7 coreos"
        ]
      }
    ]
  }
}

```

### CoreOS - put ignition file onto a webserver

The CoreOS installation procedure will need to find the ignition file in the network - on a webserver. This can be any webserver either
in your local network or in the Internet. You may want to check with a browser that this file is delivered from an URL like:

`https://anyserver.org/assets/config.ign`

**TIPP**

If you don't have a webserver you can use github.com for this purpose.

To do this ... create a GitHub account on github.com for free if you do not have one already. 

Then go this community workspace

`https://github.com/ths-community/podman`

and use the 'fork' function to get 1:1 copy - then you can change the ignition file in the 'config' folder and add your public key.
After the file is committed (saved) you will get the full ignition file with URL

`https://raw.githubusercontent.com/YOUNAME/main/ignition/config.ign`

where YOURNAME is the name of your github account.

Note: You may feel uncomfortable to put a public key to a public webserver - but this is what public keys are made for.
However, there is a (very small) risk that if a hacker comes in possession of private(!) keys they may use those to test which public key will fit
and then use this knowledge to make it part of an attack plan. But if a hacker has your private keys already then publicly visible public keys
are your least problem then.


