# LMS
Learning Management System

<!-- Note -->

The core of the Open edX platform is its learning management system
(LMS) — which, in a bout of creativity, its creators called
`lms`. Any learner and student interacts with the LMS to access
course content, watch videos, take labs and quizzes, and collaborate
with co-learners.

This is what the LMS on an Open edX platform looks like:

_Cut to LMS demo:_

* Log in
* Find course
* Enroll in course
* Open course overview
* (optional) open discussion forum


<!-- .slide: data-background-image="images/Django_logo.svg" data-background-size="contain" -->

<!-- Note -->

The `lms` is essentially a fairly involved
[Django](https://www.djangoproject.com/) application. Or rather, a
whole _collection_ of applications, reflecting a theme that will no
doubt be familiar to many Django developers.

And there are many other things about the LMS that will feel very
familiar to people comfortable with Django applications: it runs in
its own venv, it relies on pip packages to provide additional
functionality, it puts its data into a MySQL database (relational data
includes things like user details, course enrollments, or exercise
results), it makes use of the django-storage abstraction layer.


# CMS
Course Management System

<!-- Note -->

And then there’s its cousin, the course management system (you guessed
it, `cms`, although there is a slightly snazzier alias available in
_Open edX Studio_), which is for teachers, instructors, and course
authors.

_Cut to CMS demo:_

* Open course overview
* Course setting
* (optional) some course content

Learners do not interact with Studio, and even for authors, its use is
optional.  Those who prefer to, can manage course content in an
external content store or a revision control system like
[Git](https://git-scm.com/), and import from there. Like the LMS, the
CMS is also a Django application.


## OLX
Open Learning XML

<!-- Note -->

Courseware content is internally stored in an XML flavor called OLX,
and lives in a content store called


## GridFS

<!-- Note -->

... which is admittedly a _little bit_ quirky in that it’s something
that more-or-less emulates a filesystem, but in MongoDB. Bluntly
speaking this a really odd way to store file-like content — it’s
basically a kluge, for Open edX’s use case — , but its introduction
predated the ready availability of stable distributed filesystems like
CephFS (and evidently, NFS wasn’t an option), and it seems to be
really hard to get rid of.


<!-- .slide: data-background-image="images/cluster.svg" data-background-size="contain" -->

<!-- Note -->

So that _application_ architecture then brings us so a _systems_
architecture that looks roughly like this:

* A persistent backend holding MongoDB and MySQL data (of which we
  need a 3-node cluster to ensure high availability)
* An arbitrarily large or small set of essentially stateless frontend
  application servers.

Since all communication with the frontend servers is either
straight-up web content or REST, and thus uses HTTP/HTTPS everywhere,
we can easily stick all the frontend servers behind a load balancer
and be done with it.


<!-- .slide: data-background-image="images/ansible-logo.svg" data-background-size="contain" -->

<!-- Note -->

And what do we use to deploy all this? Ansible, of course. Open edX
includes, as part of its AGPL-licensed stack, [the `edx/configuration`
GitHub repo](https://github.com/edx/configuration/), which contains a
bunch of Ansible roles and playbooks to fully automate the deployment
of an edX environment.

In principle, Open edX is platform agnostic. You can run it directly
on baremetal servers running Ubuntu, you can run it in VMware or KVM,
on AWS (this is what edx.org uses), or in Docker containers (the
recently-renamed [Tutor](http://docs.tutor.overhang.io/en/latest/)
project, formerly called `openedx-docker`). However, for the rest of
this talk, I’ll be focusing on running Open edX on OpenStack.
