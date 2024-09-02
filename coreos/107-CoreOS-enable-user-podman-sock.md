### where podman is similar to docker

even fundamental concepts are different podman was developed as "drop in" replacement for docker - so as regular user you can use podman in `set podman=docker` mode.

Well known commands like `podman run`, `podman ps`, `podman stop` etc will work exactly like their respective counterparts in docker - also for regular users.
There is no need for such a user to be added to a docker group

### where podman is different

* resources like networks or namespaces are no longer shared on machine level - two regular users can run container workloads using the same names without any interference.
* there is no docker.sock - this may turn out as larger issue if you copy docker scripts or recipes to run under podman. Any access to docker.sock will fail.
There is an options to start a service which provides such a socket for compatibility reasons - usually it is not needed
* docker workloads are started automatically at boot time - for podman we will use systemd in user mode(!) to do the same (see examples in this repo)
* podman can bundle multiple container workloads in a "pod" - a concept well-known in kubernetes but unknown in docker - "pods" are described in YAML files similar
to kubernetes and are very useful in use-cases in which
an application like gICS or keycloak need a database to run - pods have an *internal* network in which any2any port can communicate 
* when a user logs off all containers will stop - use "loginctl enable-linger username" to avoid this and allow containers to continue running

## how to use (user) podman.sock ?

...
