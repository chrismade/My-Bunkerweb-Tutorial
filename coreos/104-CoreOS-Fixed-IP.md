fixed IP in coreos

## tell installer to keep the network settings 

copy the install environment networking configuration into the installed system via coreos-installer --copy-network.

source: https://dustymabe.com/2020/11/18/coreos-install-via-live-iso--copy-network/


## configure static IP address on Fedora 31 step by step instructions

1.  Open up terminal and obtain the name or UUID of the network connection you with to configure with static IP address. Example:
    
    ```
    $ sudo nmcli connection show
    NAME                UUID                                  TYPE      DEVICE 
    Wired connection 1  91d78f79-c7cf-32fc-8a91-bc3d587a2461  ethernet  enp0s3 
    virbr0              b5402c70-eb3d-4a45-a7ad-43e1652798b1  bridge    virbr0
    ```
    
    The UUID of the network connection we wish to change is `91d78f79-c7cf-32fc-8a91-bc3d587a2461`.
    

* * *

* * *

1.  Use `nmcli` command to set the example static IP address `192.168.1.127/24`, DNS `8.8.8.8`, gateway `192.168.1.1` and configuration method as `manual`. Change the bellow settings to suit your needs and use the UUID you have retrieved in the previous step:
    
    ```
    $ sudo nmcli connection modify 91d78f79-c7cf-32fc-8a91-bc3d587a2461 IPv4.address 192.168.1.127/24
    $ sudo nmcli connection modify 91d78f79-c7cf-32fc-8a91-bc3d587a2461 IPv4.gateway 192.168.1.1
    $ sudo nmcli connection modify 91d78f79-c7cf-32fc-8a91-bc3d587a2461 IPv4.dns 8.8.8.8
    $ sudo nmcli connection modify 91d78f79-c7cf-32fc-8a91-bc3d587a2461 IPv4.method manual
    ```
    
2.  Restart your network to apply changes:
    
    ```
    $ sudo nmcli connection down 91d78f79-c7cf-32fc-8a91-bc3d587a2461
    $ sudo nmcli connection up 91d78f79-c7cf-32fc-8a91-bc3d587a2461
    
    ```
    
    All done.
    

source:

https://linuxconfig.org/how-to-configure-static-ip-address-on-fedora-31

* * *