The combination of Open edX and OpenStack can enable information
technology professionals to acquire critical skills in complex
distributed systems technology.

## New technology, new challenges

Almost every new technology developed in recent years has been
complex, distributed, and built for scale:
[Kubernetes](https://kubernetes.io/), [Ceph](https://ceph.com), and
[OpenStack](https://openstack.org/) can serve as just a few
representative examples. These systems are quite different from the
ones we saw just a few years ago. Practically any nontrivial software
solution today comes with loose coupling, asynchronicity, and
elasticity – properties usually absent from the systems of the
past. 

People comfortable with building and operating such complex systems
are rare and in high demand.  This presents a problem for
organizations who are struggling to adopt new technologies precisely
because they lack people who understand them: we are dealing with a
skills gap, not a technology gap.

This means that we need novel ways to allow professionals to
familiarize themselves with these technologies. We must provide
professional learners with complex, distributed systems to use as
realistic learning environments. We must enable them to learn from
anywhere, at any time, and on their own pace.

Now subjecting, say, a Ceph novice to the joy of downloading dozens
of Gigabytes’ worth of virtual images — just so they can spin up a
reference Ceph cluster — won’t make for a stellar learning experience:
you’re likely not in possession of a laptop that comes close to the
memory and CPU capacity of a modern server, much less of at least 10
of them to simulate a decent-sized Ceph cluster.

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
in 2013, it rapidly expanded to other universities and also
corporations, with [Microsoft](https://www.microsoft.com/) being a
then-unlikely early adopter in 2015. Today, the non-profit [edX
Inc.](https://www.edx.org/) drives the majority of Open edX’s
development, and continues to host the [edx.org](https://www.edx.org)
learning site, but the entire platform is open source software and
welcomes community contributions.

[As of May 2018](https://blog.edx.org/furthering-the-edx-mission),
edx.org had served approximately 16 million learners through its own
and its official partners’ sites. At the same time, another 18 million
learners were estimated to use independent Open edX based platforms
world-wide.


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

TODO: Include LMS screen shot

Its cousin, the content management system (you guessed it, `cms`,
although there is a slightly snazzier alias available in _Open edX
Studio_), is for teachers, instructors, and course authors. Learners
do not interact with Studio, and even for authors, its use is
optional.  Those who prefer to, can manage course content in an
external content store or a revision control system like
[Git](https://git-scm.com/), and import from there. Like the LMS, the
CMS is also a Django application.

TODO: Include CMS screen shot

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

XBlocks are Open edX’s plugin system that allows their authors to
significantly expand Open edX’s functionality. XBlocks exist for
learning purposes as diverse as building molecular models in
chemistry, or solving mathematical equations.

The XBlock SDK and API are [Apache
licensed](https://tldrlegal.com/license/apache-license-2.0-(apache-2.0)),
so that XBlock authors can, in principle, write XBlocks that interface
with systems that do not use open source licenses themselves. In
practice, [most available
XBlocks](https://openedx.atlassian.net/wiki/spaces/COMM/pages/43385346/XBlocks+Directory)
do use OSI-approved licenses.

For learning interactively on OpenStack-hosted resources, [an
XBlock](https://github.com/hastexo/hastexo-xblock) is available that
spins up course author defined Heat stacks on demand, and presents
either a terminal session or and
[RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) session
right in the learner’s browser, via [Apache
Guacamole](https://guacamole.apache.org/).

TODO: Include screen shot of course page including terminal window


## OpenStack
Arbitrarily complex lab environments for learners

<!-- Note -->

OpenStack is an
[infrastructure-as-a-service](https://en.wikipedia.org/wiki/Infrastructure_as_a_service)
platform whose orchestration component, [OpenStack
Heat](https://docs.openstack.org/heat/), comes in handy in providing
arbitrarily complex lab environments to learners. Using a Heat
template, a course author can define an exactly reproducible,
self-contained environment consisting of, say, 10 servers in 3
networks connected with 2 routers and arbitrarily involved
configurations for each server.

Heat has some interesting features that set it apart from its
workalikes on other cloud platforms, like [AWS
CloudFormation](https://aws.amazon.com/cloudformation/) or [Google
Cloud Deployment
Manager](https://cloud.google.com/deployment-manager/). In particular,
Heat has the ability to suspend an entire stack, however complex,
in-place – to then resume it at a much later date and return it to the
exact same state as it previously was. This, of course, comes in quite
handy in the training lab use case: in self-paced training, learners
typically spend 30-45 minutes on each lesson, and might do one such
lesson either every day, or every other day. Over the course of a
month, learners may thus use their lab for as little as perhaps 10
hours in total. Keeping that same lab running over the entire month,
at a cost of perhaps in excess of $1,000, would be entirely
cost-prohibitive. However, making the lab available with surgical
precision only when needed can drive this price point down into just
double digits, and making the whole endeavor entirely affordable.

TODO: Include screen shot of “we think you’re busy elsewhere” message


## In summary

By combining the facilities of Open edX — in particular its XBlock
plugin system — with OpenStack, a learning provider can give learners
the opportunity to explore the inner workings of arbitrarily complex
distributed systems in a completely self-directed and very
cost-effective fashion. Such a system — comprised of exclusively
open-source components — can enable organizations and individuals to
quickly grow technology skills.

* * *

*The author of this article is to present [a technical
overview](https://linux.conf.au/schedule/presentation/121/) of the
Open edX platform as part of the
[linux.conf.au](https://linux.conf.au/) conference in Christchurch,
New Zealand later this month. This article will be updated with slides
and a video of the presentation, once they become available.*
