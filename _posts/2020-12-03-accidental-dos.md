---
layout: post
title: 'How we nearly introduced a hilarious DoS vulnerability'
tags: [programming, software engineering, software design]
---

Denial of Service (DoS) attacks are pretty common. They usually involve a number
of machines flooding a target server with requests. But that requires a lot of
processing power. What if we could perform a DoS attack with only few requests?
That would be fun, right?

Those who are familiar with web security probably remember the infamous [Slowloris
attack](https://en.wikipedia.org/wiki/Slowloris_(computer_security)). However,
this post isn't about Slowloris, it's about a new vulnerability which we nearly
introduced in our data service.

## The Data Service
Before we go over the vulnerability, we need to first introduce said "data service".
It is a service meant to serve the data needs of our user-facing interfaces. This
included primarily our user portals and email notifications.And one of its main
features is searching.

Its search operation follows the following steps:
1. Create an Elasticsearch query
2. Call Elasticsearch and get only the IDs of matching documents
3. Call the services which own those documents to get the full document
4. Perform any other needed data processing

The question you might ask here is "why doesn't it just use the Elasticsearch
documents directly? Why call other services?". There are two reasons behind this
decision:
1. Some services have complex authorization check to verify that a caller should
be allowed to view a particular document.
2. What is persisted in an Elasticsearch index might not match the representation
a service has in the response.

Although the second reason is still important to maintain consistency, it's the
lesser of the two.

### Failed Calls
If a call to the owning service resulted in a non-2xx HTTP response, that document
is removed from the final results. So if we matched 20 documents, but got 403
response for one of them, we return back 19.

## The "Feature" that Nearly Caused a Big Problem
At some point, we didn't like the fact that a user portal might request 20 documents
but get back 19 or less. So there was an idea, what if we made the service get
more than 20, and even if it couldn't get 20 because of failed calls, cycle through more documents. Keep
doing that until we have 20 documents.

Sounds like a very innocent feature at first but in reality it introduces a very
specific DoS vulnerability. A malicious user of our portal could search for
something they have no access to with a regular expression which matches
everything. That would cause the data service to run through the entire index and
make a call to the owning service for each one. If the index had 10,000 documents,
then there will be 10,000 requests costing the attacker only a single request.

Just like that, we nearly turned our data service into a DoS requests amplifier,
turing one request into potentially tens of thousands. We could have limited the
cycle to only one extra round, but that would result in the same business problem
where they want exactly _n_ documents. Eventually we just decided that we cannot
guarantee that the service will return _n_ documents even if _n_ or more documents
match the search criteria.
