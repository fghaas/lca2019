<!-- .slide: data-background-image="images/openstack-logo.svg" data-background-size="contain" -->

<!-- Note -->
Up to this point, we’ve only looked at OpenStack as one of many
platforms we can run Open edX _on,_ but that’s really only one half of
how Open edX can integrate with OpenStack. The other half is
interactive lab environments.

Now why and how is this useful? Allow me a quick detour for that.


## New technology, new challenges

<!-- Note -->
Consider that almost every new technology developed in recent years
has been complex, distributed, and built with scaleability in mind:
Think for example of [Kubernetes](https://kubernetes.io/),
[Ceph](https://ceph.com), and [OpenStack](https://openstack.org/)
itself. These systems are quite different from the ones we saw just a
few years ago. Loose coupling, asynchronicity, and elasticity: these
are properties that were mostly absent from systems we built 5 years
ago, but they’re pretty much table stakes in contemporary systems.

Now, people comfortable with building and operating such complex
systems are rare and in very high demand.  This presents a problem for
organizations who are struggling to adopt new technologies precisely
because they lack people who understand them. Meaning, we are dealing
with a skills gap, not a technology gap.

This means that as someone who builds learning systems that we must
provide learners — that is, potentially, **you** — with complex,
distributed systems to use as realistic learning environments. We must
enable you to learn from anywhere, at any time, and on your own
pace.

Now suppose you’re a Ceph novice. Subjecting you to the joy of downloading
Gigabytes’ worth of virtual images — just so you can spin up a
reference Ceph cluster — won’t make for a stellar learning experience:
you’re likely not in possession of a laptop that comes close to the
memory and CPU capacity of a modern server, much less of at least 10
of them to simulate a decent-sized Ceph cluster.

But we **can** do that by using cloud technology.


<!-- .slide: data-background-image="images/heat-logo.svg" data-background-size="contain" -->

<!-- Note -->
Again, going back to OpenStack Heat, we can define an exactly
reproducible, self-contained environment consisting of, say, 10
servers in 3 networks connected with 2 routers, and then toss arbitrarily
involved configurations at each of those 10 servers.

And if we use that as the definition of a learner’s lab environment,
we can give every learner an exactly defined working lab for whatever
technology a course aims to teach, however complex that
infrastructure needs might be.


<!-- .slide: data-background-image="images/openedx-logo.svg" data-background-size="contain" -->

<!-- Note -->
Now how do we make all of that play nicely with Open edX? To explain
that, let me introduce you to XBlocks. 


## XBlocks
A flexible, extensible plugin system

<!-- Note -->
XBlocks are Open edX’s plugin system that allows XBlocks authors to
expand the LMS functionality. XBlocks exist for very diverse learning
purposes: for example, building molecular models in chemistry, or
solving mathematical equations.

The XBlock SDK and API are, in contrast to the AGPL’d Open edX
platform, [Apache
licensed](https://tldrlegal.com/license/apache-license-2.0-(apache-2.0)).
That means that XBlock authors can, in principle, write XBlocks that
interface with systems that do not use open source licenses
themselves. However, in practice, [most available
XBlocks](https://openedx.atlassian.net/wiki/spaces/COMM/pages/43385346/XBlocks+Directory)
do use OSI-approved open source licenses.

For learning interactively on OpenStack-hosted resources, [an
XBlock](https://github.com/hastexo/hastexo-xblock) is available that
spins up a course author defined Heat stack on demand, and presents
either a terminal session or an
[RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) session
right in the learner’s browser, via [Apache
Guacamole](https://guacamole.apache.org/).

_(Cut to lab demo)_


<!-- .slide: data-background-image="images/celery-logo.svg" data-background-size="contain" -->

<!-- Note -->
Now in case you’re wondering how exactly we’re running rather complex
and long-running tasks from within the processing of an HTTP request, 
let me introduce you to [Celery](http://www.celeryproject.org/).

Celery is an asynchronous task-queue and processing
framework. Basically it’s a facility that allows an application to
say, “go do X”, and then continue doing whatever it was doing while X
is asychronously being completed in the background.

[This is frequently used in Django
applications](http://docs.celeryproject.org/en/latest/django/first-steps-with-django.html),
which can use Celery to offload asynchronous tasks and thus not hold
up the (synchronous) request processing queue. In the XBlock, we use
this for asynchronously firing off any API calls to OpenStack,
including stack creation and deletion.

We essentially wrap `python-heatclient` (the Python client library for
OpenStack Heat) in a Celery task, and then report back the task’s
outcome.


<!-- .slide: data-background-image="images/openstack-logo.svg" data-background-size="contain" -->

<!-- Note -->
Now you may be wondering why is he talking only about OpenStack? Heat
has workalikes on other cloud platforms, like [AWS
CloudFormation](https://aws.amazon.com/cloudformation/) or [Google
Cloud Deployment
Manager](https://cloud.google.com/deployment-manager/). Can’t we use
those?

At least for the time being, the answer is no. And that’s not the
XBlock’s fault.


# $$$

<!-- Note -->
Heat is the only cloud orchestration component that has the ability to
suspend an entire stack, however complex, in-place – to then resume it
at a much later date and return it to the exact same state as it
previously was.

And this comes in super handy in the training lab use case: in
self-paced training, learners typically spend 30-45 minutes on each
lesson, and might do one such lesson either every day, or every other
day. Over the course of a month, learners may thus use their lab for
as little as perhaps 10 hours in total.

Now Keeping that same lab running over the entire month, at a cost of
perhaps in excess of $1,000, would be entirely
cost-prohibitive. However, making the lab available with surgical
precision only when needed can drive this price point down into just
double digits, and making the whole endeavor entirely affordable.

Now how does **that** work?


### Celery, Heat, & JavaScript

<!-- Note -->
Here’s what we do: when we spin up a stack, we record its creation
timestamp in a database.

And then we have an HTTP endpoint on the LMS that listens to keepalive
`POST`s from the client. And there’s some JavaScript running in the
user’s browser that sends a `POST` request every thirty
seconds. Whenever we receive such a keepalive message, we update the
stack timestamp (we actually don’t update in-place, we append to a
stack log, but that’s a technicality).

If the learner now closes their browser tab, or suspends their laptop,
or loses their network connectivity so they can no longer interact
with their lab, the stack timestamps don’t get updated.

Server-side, we then have a “suspender” job that runs every minute
(configurable) and scans the database for any stacks whose most recent
keepalive timestamp is more than two minutes (configurable) old, and
fires off a Heat stack suspend task (again, via Celery).

When the user then returns to their lab, we resume the stack, instead
of firing up a new one.


### But wait...

<!-- Note -->
You may ask, hang on, if Guacamole has an SSH session open to the
stack, and the stack suspends, and then resumes, surely the SSH
session will hit some timeout and drop, in the interim. So how do you
ensure a smooth learning experience?

With a clever little `screen` hack.


```bash
# ~/.profile

exec /usr/bin/screen -xRR
```

<!-- Note -->
`screen -xRR` means “re-attached to the most recently attached
`screen` session, and if there isn’t any running, then just fire up a
new one.

And by `exec`-ing that from the learner’s profile, we’re just forcing
any SSH client (including `guacd`) to reconnect to the previous
session.

(For RDP-based sessions, we don’t need any such magic, as RDP is a
protocol that always reconnects to an existing desktop that is
preserved in the same state as it was left.)
