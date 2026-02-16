---
Author: J.M. Porup
Full Title: How to make Linux more trustworthy
Category: #articles
Publication date: 2016-12-15
URL: http://arstechnica.co.uk/information-technology/2016/12/how-to-make-linux-more-trustworthy/
Summary: Reproducible builds are a good step, but Red Hat and Ubuntu don't want to join in.
---

![rw-book-cover](https://cdn.arstechnica.net/wp-content/uploads/2016/12/trustworthy-linux-penguin-silhouette-760x380.jpg)

# Notes

[[J.M. Porup - How to make Linux more trustworthy]]

***

#### Reproducible builds are a good step, but Red Hat and Ubuntu don't want to join in.

![How to make Linux more trustworthy](https://cdn.arstechnica.net/wp-content/uploads/2016/12/trustworthy-linux-penguin-silhouette-800x441.jpg)Enlarge
While the fight against government-mandated software backdoors raged for most of 2016—including [the showdown between Apple and the FBI](http://arstechnica.co.uk/gadgets/2016/02/tim-cook-says-apple-will-fight-us-govt-over-court-ordered-iphone-backdoor/) over the San Bernardino shooter's iPhone, and the UK's new [Investigatory Powers Act](http://arstechnica.co.uk/tech-policy/2016/11/investigatory-powers-act-imminent-peers-clear-path-for-uk-super-snoop-law/), which gives the government the power to demand UK companies backdoor their software to enable mass surveillance—the [Core Infrastructure Initiative](https://www.coreinfrastructure.org/) (CII) has been quietly working to prevent an even more insidious form of backdoor: malicious code inserted during the software build process without a developer’s knowledge or consent.

Such attacks are by no means theoretical. Documents from Snowden's trove make clear that intelligence agencies are actively working to compromise the software build process on individual developer's computers. The [CIA created a bogus version of XCode](https://theintercept.com/2015/03/10/ispy-cia-campaign-steal-apples-secrets/), the software used by developers to package applications for Apple devices. Such attacks have been difficult to detect—until now.

Led by the Debian Project, other Linux distros and software projects, including the [Tor Browser](https://blog.torproject.org/category/tags/deterministic-builds) and [Bitcoin](https://github.com/bitcoin/bitcoin/blob/master/doc/gitian-building.md), are also working to make their build processes more trustworthy, and the CII has thrown their financial weight behind the project. In 2015 the CII funded the [Reproducible Builds Project](https://reproducible-builds.org/) to the tune of $200,000 (£160,000), and in November of this year they announced an additional $225,000 (£180,000) of funding.

Reproducible builds, also known as deterministic builds, ensure a byte-for-byte equivalent binary given identical source code. Reproducible builds make it possible for anyone to reproduce the build process and gain improved confidence that a software package is what the developer intended.

#### Debian's push for reproducible builds

Alarmed at the prospect of being unable to trust all 24,000-plus Debian packages, most of which are built on package maintainers’ laptops, the [Debian Project has pushed hard over the last three years](https://motherboard.vice.com/read/how-debian-is-trying-to-shut-down-the-cia-and-make-software-trustworthy-again) to make as many packages reproducibly buildable as possible. As of this writing, 91 percent have achieved that goal, and the project hopes to hit 100 percent in 2017.

[![91.8% of Debian testing/amd64 packages are reproducibly buildable as of this writing.](https://cdn.arstechnica.net/wp-content/uploads/sites/3/2016/12/stats_pkg_state-980x490.png)](https://cdn.arstechnica.net/wp-content/uploads/sites/3/2016/12/stats_pkg_state.png)91.8% of Debian testing/amd64 packages are reproducibly buildable as of this writing.
The biggest challenges in making Debian packages reproducibly buildable have been surprisingly mundane. Small details like differences in timestamps or build directories result in different binaries. Updating the build toolchain to account for such differences has been one of the notable successes of the Reproducible Builds Project.

Two key innovations over the last year have made a big difference. The first, [diffoscope](https://diffoscope.org/), makes it possible to diff two Debian packages, or ISOs, or even tarballs, and drill down into what makes the software bundles different. The second is a patch to the gcc compiler which, Nicko van Someren, CTO of the Linux Foundation, which organises the CII, told Ars, "allows you to specify the prefix of the build directory that you're looking at." The gcc patch makes it possible to reproducibly build software source in different directories, which previously would have resulted in slightly different binaries.

A big part of the CII funding is about creating a new toolchain that will serve other Linux distributions, not just Debian, van Someren emphasised.

"Ensuring that no flaws are introduced during the build process greatly improves software security and control," Debian developer Chris Lamb said. "Our work has already made significant progress in Debian GNU/Linux, and we are making our tools available for Fedora, Guix, Ubuntu, OpenWrt and other distributions."

#### So what about Red Hat?

Despite the CII's push to deploy reproducible builds beyond just Debian, Red Hat told Ars they had no plans to move to reproducible builds.

"No, Red Hat does not have any plans to do this [make Red Hat reproducibly buildable]," Red Hat's director of corporate communications, Stephanie Wonderlick, told Ars, adding, "Josh Bressers, a security strategist at Red Hat, [wrote a blog post](http://sobersecurity.blogspot.co.uk/2016/05/trusting-trusting-trust.html) on this that may be of interest."

[Fedora](http://arstechnica.co.uk/information-technology/2016/08/fedora-24-review/) project leader Matthew Millar told Ars that he thinks reproducible builds are interesting, but "it hasn't been a big initiative." He noted that, unlike Debian packages, which are typically built on individual developer's laptops, "Fedora requires all packages to be built in our central system, from source, using scripts in a version-controlled repository. And, that central build system is all itself open source and we distribute the Ansible playbooks to deploy it in production."

"That means we already have a transparent, traceable pathway from binary packages back to source code," he added.

Transparent to the Fedora Project, perhaps, but not to end users. A central build system is also a central point of failure. How does the Fedora build system ensure trustworthy binaries in the face of intelligence agencies who wish to insert compile-time backdoors? Millar did not respond to this question, or further e-mails.

Given that Red Hat is a US-based company, and thus subject to secret court orders from the US government, can we be sure that Red Hat has not been compelled to insert compile-time backdoors?

"Red Hat has never intentionally inserted a back door in our products, and we have never received a court order to do so," Wonderlick told Ars.
