# LMS

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
results).


# CMS

Its cousin, the course management system (you guessed it, `cms`,
although there is a slightly snazzier alias available in _Open edX
Studio_), is for teachers, instructors, and course authors. Learners
do not interact with Studio, and even for authors, its use is
optional.  Those who prefer to, can manage course content in an
external content store or a revision control system like
[Git](https://git-scm.com/), and import from there. Like the LMS, the
CMS is also a Django application.
