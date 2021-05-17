---
layout: post
title: 'Microservices: The Forest and The Trees'
tags: [programming, software engineering, software design]
---

> "Microservices solve a people problem more than a technology problem",
Someone from a podcast I can't recall, 2019

There's really no need to introduce microservice architecture at this point; it's been talked
about to death over the past decade. We don't even need to its appeal of: smaller teams,
smaller services, each is easily managed and maintained...etc, etc. You can find
hunderds of articles and videos online talking about that. What we're going to talk
about here is one of the pitfalls, and one which is often ignored and rarely talked about,
even though a lot of teams, including us, fell victim to it. It's
"*service tunnel vision*", for the lack of a better term.

> Service tunnel vision is when teams working on different services of the same
product, focus too much on their parts and ignore the overall product and business
needs.

Let's imagine that we're working for the same company, along with 50 other engineers.
We're all working on that big product called Behemolith. Of course, we're not
building it as a monolith, despite what the name suggests, we're doing *microservices*
.. *microservices* [fainting echo]. I'm working on services A and B with my team,
we know exactly what we need and we're designing it well. You're doing the same
thing with services X and Y, and so is everyone across all teams. Everything is going
well, and then a simple business requirement comes along, we realize now that we
can't satisfy that business without jumping and hopping across services. What
came as a basic feature, turned into a full project. I'm not exaggerating here,
it took us nearly 3 months to provide a "simple feature", which on paper felt like
two weeks worth of work... maximum.

So why does that happen? And, perhaps, most importantly, how can we avoid it? The
reason it happened in the first place is fairly straightfoward. When you and I
were working on our services, we were each focusing on a tree, but we both forgot
that our trees are still part of the larger forest, and we forgot how the forest
, as a whole, should look like after all. We focused on the technology, design,
and implementation of each service, we treated every other service as an "external component",
sometimes even immutable like it was provided by a third party. We ended up
sidelining the fact that all our services are part of the same system, at least
from the business side of things.

I know, at this point, some people are already disagreeing with me, and defending
that way of doing microservices; *a small price to pay for salvation*, they're
currently thinking. Fair enough, but can't we do better? I think we can. We need
to stay accountable to the end goals of the business. I know.. I know, easier
said than done. Nevertheless, I think the core issue here is us being constantly
sidetracked by technology, and teams working entirely in isolation.

How that can be fixed exactly, will probably depend on your own organization.
After all, I think this is a management and work culture issue, neither of which
I can help with.
