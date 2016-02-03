---
layout: article
title: Enabling your microservice architecture
modified:
categories:
excerpt:
tags: []
image:
  feature:
  teaser:
  thumb:
date: 2016-02-03T22:37:12+00:00
comments: true
---

Anyone who knows me and speaks to me about technology knows I speak consistently about very many few things:

1. Make everything as stateless as possible.
2. Make everything a microservice at whatever cost.
3. Publish everything in containers.

Each of these would make a fairly good blog post, but I really wanted to focus on the journey we have been on over the last 6 months in enabling our microservice architecture.

We have a fairly complex system; we are comprised of many integrations to third party systems, each of which have different business logic. We deal with huge amounts of requests per second and we **aim** to ensure complete request acknowledgement, even if that acknowledgement may respond with a negative answer (sad face 500 times). By the microservice approach, we have been on a journey to abstract each payment provider into their own microservice, only tied together by a common API contract from our north calling services along with a HTTP request handling framework which expects to be called on certain endpoints. Because every processor is different, containing different business logic, response handling and persistence of core data services, microservices are perfect here.

One of the biggest stumbling blocks we came across, however, is how on Earth you host so many tiny little microservices. The things that plagued us:

* How are we going to deploy these services?
* How do we solve auto-discovery?
* What kind of load balancer would we need that could understand where a dynamic service was and route to it appropriately?

The list seemed endless...

I spent a lot of time looking into the various "roll your own cloud" services that are all trying to emulate the *fantastic* work by my good friends over at the Heroku HQ, however no one seems to make it
