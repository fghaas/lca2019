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

TODO: Explain what we need in a learning platform to achieve this.


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

TODO: Explain how XBlocks work, how they tie in with Open edX, and
that everyone can write their own (or choose from the many that are
available).


## OpenStack
Arbitrarily complex lab environments for learners

<!-- Note -->

TODO: Explain what OpenStack Heat is, how we can use it to define
arbitrarily complex, but completely reproducible learning
environments, and that we can interface from Open edX both with
private and public OpenStack clouds.


## How to avoid breaking the bank

<!-- Note -->

TODO: Explain that OpenStack Heat allows Open edX operators, uniquely,
to suspend and resume full lab environments at will, and that this
enables them to offer learners very complex and realistic learning
environments at a fraction of their regular cost, with non-existant
learner-facing setup hassles.
