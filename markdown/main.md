## Now what’s this all about?

<!-- Note -->

Now the learning platform that I’m talking about is primarily
comprised of two things:


<!-- .slide: data-background-image="images/openedx-logo.svg" data-background-size="contain" -->

<!-- Note -->
First, Open edX. This is a learning management platform that
originally started at [Harvard University](https://www.harvard.edu/)
and [MIT](https://www.mit.edu/) in 2012, with subsequent collaboration
from [Stanford University](https://www.stanford.edu/). Released under
the
[AGPL](https://tldrlegal.com/license/gnu-affero-general-public-license-v3-(agpl-3.0))
in 2013, it rapidly expanded to other universities and organizations,
with [Microsoft](https://www.microsoft.com/) being a then-unlikely
early adopter in 2015. Today, the non-profit [edX
Inc.](https://www.edx.org/) drives the majority of Open edX’s
development, but the entire learning platform is open source software
and welcomes community contributions.

In principle, Open edX is platform agnostic. You can run it directly
on baremetal servers running Ubuntu, you can run it in VMware or KVM,
on AWS (this is what edx.org uses), or in Docker containers (the
recently-renamed [Tutor](http://docs.tutor.overhang.io/en/latest/)
project, formerly called `openedx-docker`). In this talk however, I’ll
be focusing on running Open edX on OpenStack.


<!-- .slide: data-background-image="images/openstack-logo.svg" data-background-size="contain" -->

<!-- Note -->
In fact, we’ll be looking at OpenStack, in an Open edX context, from
two angles. One, running the platform itself on OpenStack. Two, using
OpenStack from Open edX to provide on-demand interactive lab
environments. I’ll start with the first part. So what do we need in
order to run Open edX on OpenStack?


<!-- .slide: data-background-image="images/heat-logo.svg" data-background-size="contain" -->

<!-- Note -->
First, we’ll need OpenStack Heat. That, again, is not the *only* way
we can get Open edX going on OpenStack
([Terraform](https://www.terraform.io/), with its [OpenStack
provider](https://www.terraform.io/docs/providers/openstack/), would
be another), but it’s a good and effective way to spin up an Open edX
environment quickly.


<!-- .slide: data-background-image="images/cluster.svg" data-background-size="contain" -->

<!-- Note -->
What we need to deploy with Heat is two things: one, we need a backend
cluster. This is always an exactly 3-node MySQL and MongoDB cluster
that uses persistent volumes. This contains all the data that we need
to preserve: courseware content, enrollment data, authentication
credentials (if we’re not using an external authentication backend
like OAuth or SAML), that sort of data.

Two, we need an application server cluster. This is a scaleable
frontend that consists of one or more front-end nodes. Small Open edX
installations might deploy one one. Others, 2 to 3. Still larger ones,
dozens. These can scale in and out as needed. To maintain one
consistent frontend, we plug all of those into an OpenStack managed
load balancer — either [Neutron LBaaSv2 (now
deprecated)](https://wiki.openstack.org/wiki/Neutron/LBaaS), or
[Octavia](https://docs.openstack.org/octavia/latest/). Heat manages
all of that for us — scale-out (cluster expansion), scale-in (cluster
contraction), and LBaaS pool management.
