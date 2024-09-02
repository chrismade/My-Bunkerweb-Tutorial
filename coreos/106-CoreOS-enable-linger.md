**login setting**

there is one more thing which we need to do in order to allow processes started by regular user 'core' to remain existent after logoff:

`sudo loginctl enable-linger core`

after this setting we will use 'sudo' or 'root' access rarely and most actions will be run under user 'core' even create/start/stop/remove of containers.

**reboot**

in order to test that everything is setup correctly please reboot again now with

`sudo reboot`

after successful reboot CoreOS is ready to use, has a separated data disk for easier disaster recovery and will automatically update at least once a month
for better security - and is ready to run our container workloads like gICS, keycloak or bunkerweb (WAF = web application firewall)


