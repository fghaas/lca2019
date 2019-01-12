<!-- .slide: data-background-image="images/openstack-logo.svg" data-background-size="contain" -->

<!-- Note -->
So what do we need in order to run Open edX on OpenStack?


<!-- .slide: data-background-image="images/cluster.svg" data-background-size="contain" -->

<!-- Note -->
In other words, how do we build this architecture, in an automated
fashion, on an OpenStack private or public cloud?

OpenStack does give us everything we need:

* VM management with Nova,
* a facility to maintain master images with Glance,
* persistent storage with Cinder,
* load balancing with Octavia and Neutron.

So how do we put it all together?


<!-- .slide: data-background-image="images/heat-logo.svg" data-background-size="contain" -->

<!-- Note -->
First, we’ll need OpenStack Heat. That, again, is not the *only* way
we can get Open edX going on OpenStack
([Terraform](https://www.terraform.io/), with its [OpenStack
provider](https://www.terraform.io/docs/providers/openstack/), would
be another), but it’s a good and effective way to spin up an Open edX
environment quickly.


<!-- .slide: data-background-image="images/glance-logo.svg" data-background-size="contain" -->

<!-- Note -->
We can also make heavy use of OpenStack Glance for the app
servers. The principle goes like this:

1. Spin up an app server from a baseline Ubuntu Xenial image.
2. Configure that “master” server with the Ansible playbooks.
3. Take a snapshot of the server, thus creating a master image.
4. Spin up as many app servers as we want.

Conveniently, we can use this process for both initial deployment and
rolling minor upgrades, thanks to the stateless nature of the app
servers, as I’m about to demonstrate:

*(Cut to upgrade demo)*
