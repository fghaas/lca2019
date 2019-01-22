## Now what’s this all about?

<!-- Note -->

So with that, let’s get started! I’m talking about a sophisticated
learning platform today, one that is being used by over 30 million
learners world-wide — strongly trending upward —, that is completely
open-source and that *you* can work with.

Indeed, you can **learn** on such a platform, you can **teach and
create learning content** with it, and you can **run, deploy, and
operate it** yourself, and today I’m going to show you how to do all
that.

The platform that I’m talking about is primarily comprised of
two things:


<!-- .slide: data-background-image="images/openedx-logo.svg" data-background-size="contain" -->

<!-- Note -->
Open edX...


<!-- .slide: data-background-image="images/openstack-logo.svg" data-background-size="contain" -->

<!-- Note -->
... and OpenStack.


<!-- .slide: data-background-image="images/openedx-logo.svg" data-background-size="contain" -->

<!-- Note -->
And I’m going to start with Open edX. This is a learning management platform that
originally started at


<!-- .slide: data-background-image="images/Harvard_shield_wreath.svg" data-background-size="contain" -->

<!-- Note -->
[Harvard University](https://www.harvard.edu/) and


<!-- .slide: data-background-image="images/MIT_Seal.svg" data-background-size="contain" -->

<!-- Note -->
[MIT](https://www.mit.edu/) in 2012, with subsequent collaboration
from


<!-- .slide: data-background-image="images/Stanford_University_seal_2003.svg" data-background-size="contain" -->

<!-- Note -->
[Stanford University](https://www.stanford.edu/). This platform was
originally an in-house development of these three universities, but
was then released as open source software in 2013 under the


# AGPL

<!-- Note -->
[AGPL](https://tldrlegal.com/license/gnu-affero-general-public-license-v3-(agpl-3.0)),
and under the name Open edX. Since 2013, it has rapidly expanded to
other universities and organizations, with
[Microsoft](https://www.microsoft.com/) being a then-unlikely early
adopter in 2015.


<!-- .slide: data-background-image="https://upload.wikimedia.org/wikipedia/commons/8/8f/EdX.svg" data-background-size="contain" -->

<!-- Note -->
Now in order to clarify, **edX** (without the “Open”) is — depending
on context — either 

* *edx.org*, a learning website serving approximately 16 million
  learners world-wide, with content developed in the aforementioned
  universities and over 60 member organizations, or

* *edX Inc.*, the non-profit entity that *runs* edx.org and drives
  most of the development of the platform.


<!-- .slide: data-background-image="images/openedx-logo.svg" data-background-size="contain" -->

<!-- Note -->
**Open edX** is that technology platform itself — which I’m going into
great detail about in a bit — which runs both edx.org and many, many
learning platforms that are not affiliated with edX Inc. Anyone can
deploy, run, and operate an Open edX site provided they comply with
the terms of the AGPL. In 2018, the estimate was that 18 million
learners were learning on unaffiliated Open edX platforms, *on top of*
the 16 million learning on edx.org.

So let’s take a look at the technical details of this platform.
