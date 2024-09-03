# bunkerweb on Fedora CoreOS

[Bunkerweb](https://www.bunkerweb.io/) comes in various shapes and forms - one is a set of (docker) 
containers. However - as of this writing - all scripts and documentation refer to docker as runtime - which is OK for workstations
but only a 2nd best choice when it comes to servers, 24/7 operations, small resource footprint (= costs) and attack surface and executable disaster recovery plans.

The purpose of this article is to demonstrate how bunkerweb can be operated in a more server-like environment optimized for better security and 
low resource footprint for 24/7 operations with a simple disaster recovery plan.

## disclaimer

As with all community contributions this is neither a fully supported product nor in any way associated with or endorsed by the Bunkerweb Team, my employer or any other company or legal entity.
It may contain bugs and may lack handling of certain situations and incidents. It may have been reduced and simplified for clarity to understand
the concepts rather than provide a bullet-proof solution.


## what is podman (in contract to 'docker')? 

There are plenty of much better - and more comprehensive - articles out there which explain WHO has initiated podman as a docker drop-in replacement - and WHY -
for the purpose of this community contribution, I would like to highlight first the *common ground* that both docker and podman both use container runtime
libraries based on the OCI standard - this provides a huge level of compatibilities between both options.

however, podman addresses and fixed a few by-design issues from docker like

- need of a root-privileged service
- potentially insecure communication between containers by docker.sock
- insecurities in multi-user setups

which is why especially Enterprise Linux distros like RedHat (incl Fedora) and SUSE decided to stop support for docker in favor of podman.

However, podman is available on all major Linux distros (caution it may come in *outdated* versions when installed by the *standard* package manager)
as well as docker (CE = community Edition) can still be installed on Enterprise Linux at your own risk / without vendor support

Frankly speaking many concepts for podman are rather a kind of backport from kubernetes - paired with the simplicity of docker

please refer to  

https://indico.cern.ch/event/757415/contributions/3421994/attachments/1855302/3047064/Podman_Rootless_Containers.pdf

for instance, as one out of many good articles regarding this topic - for more detailed docker/podman comparison and reasoning
