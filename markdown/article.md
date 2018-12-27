<!-- Note -->
Almost every new technology developed in recent years has been
complex, distributed, and built for scale:
[Kubernetes](https://kubernetes.io/), [Ceph](https://ceph.com), and
[OpenStack](https://openstack.org/) can serve as just a few
representative examples.

These system are quite different from the ones we saw just a few years
ago. Practically any nontrivial software solution today comes with
loose coupling, asynchronicity, and elasticity – properties usually
absent from the systems of the past. People comfortable with building
and operating such complex systems are rare and in high demand.  This
presents a problem for organizations who are struggling to adopt new
technologies precisely because they lack people who understand them:
we are dealing with a skills gap, not a technology gap.

This means that we need novel ways to educate professionals on these
technologies. We must provide professional learners with complex,
distributed systems to use as realistic learning environments. We must
enable them to learn from anywhere, at any time, and on their own
pace. 

Now subjecting, say, a Ceph novice to the joys of downloading dozens
of Gigabytes’ worth of virtual images won’t make for a stellar
learning experience: you’re likely not in possession of a laptop that
comes close to the memory and CPU capacity of a modern server, much
less of at least 10 of them to simulate a decent-sized Ceph cluster.

What’s the alternative? The alternative is a system that gives
learners access to arbitrarily complex, realistic distributed
environments on demand. As it turns out, the combination of two Opens,
Open edX and OpenStack, is an excellent option to provide exactly
that.


## Open edX

<!-- Note --> 

[Open edX](https://open.edx.org/) is a learning management platform
that originally started at [Harvard
University](https://www.harvard.edu/) and [MIT](https://www.mit.edu/)
in 2012, with subsequent collaboration from [Stanford
University](https://www.stanford.edu/). Released under the
[AGPL](https://tldrlegal.com/license/gnu-affero-general-public-license-v3-(agpl-3.0))
in 2013, it rapidly expanded to other universities and organizations,
with [Microsoft](https://www.microsoft.com/) being a then-unlikely
early adopter in 2015. Today, the non-profit [edX
Inc.](https://www.edx.org/) drives the majority of Open edX’s
development, but the entire learning platform is open source software
and welcomes community contributions.


### Django all the way
(almost)

<!-- Note -->

The core of the Open edX platform, its learning management system
(LMS) — which, in a bout of creativity, its creators called `lms` — is
essentially a fairly involved [Django](https://www.djangoproject.com/)
application. Or rather, a whole _collection_ of applications,
reflecting a theme that will no doubt be familiar to many Django
developers. Any learner and student interacts with the LMS to access
course content, watch videos, take labs and quizzes, and collaborate
with co-learners.

Its cousin, the content management system (you guessed it, `cms`,
although there is a slightly snazzier alias available in _Open edX
Studio_), is for teachers, instructors, and course authors. Learners
do not interact with Studio, and even for authors, its use is
optional.  Those who prefer to manage course content in an external
content store or a revision control system like
[Git](https://git-scm.com/), and import from there. Like the LMS, the
CMS is a Django application.

For interactions with other machines rather than humans, Open edX
heavily uses the [Django REST
Framework](https://www.django-rest-framework.org/) (DRF). For example,
external applications can use the
[REST](https://en.wikipedia.org/wiki/Representational_state_transfer)
API to invoke automated course enrollment based on, say, the purchase
of a course seat in a payment system.


### XBlocks
A flexible, extensible plugin system

<!-- Note -->

XBlocks are Open edX’s plugin system that allows XBlock authors to
significantly expand Open edX’s functionality. XBlocks exists for
learning purposes as diverse as building molecular models in
chemistry, or solving mathematical equations.

For learning interactively on OpenStack-hosted resources, an XBlock
comes in handy that spins up course author defined Heat stacks on
demand, and presents either a terminal session or and RDP session
right in the learner’s browser, via Apache Guacamole.


## OpenStack
Arbitrarily complex lab environments for learners

<!-- Note -->

OpenStack is an infrastructure-as-a-service platform whose
orchestration component, OpenStack Heat, comes in handy in providing
arbitrarily complex lab environments to learners. Using a Heat
template, a course author can define an exactly reproducible,
self-contained environment consisting of, say, 10 servers in 3
networks connected with 2 routers and arbitrarily involved
configurations for each server.

Heat has some interesting features that set it apart from its
workalikes on other cloud platforms, like AWS CloudFormation or Google
Cloud Platform Deployment Manager. In particular, Heat has the ability
to suspend an entire stack, however complex, in-place – to then resume
it at a much later date and return it to the exact same state as it
previously was. This, of course, comes in quite handy in the training
lab use case: in self-paced training, learners typically spend 30-45
minutes on each lesson, and might do one such lesson either every day,
or every other day. Over the course of a month, learners may thus use
their lab for as little as perhaps 10 hours in total. Keeping that
same lab running over the entire month, at a cost of perhaps in excess
of $1,000, would be entirely cost-prohibitive. However, making the lab
available with surgical precision only when needed can drive this
price point down into just double digits, and making the whole
endeavor entirely affordable.
