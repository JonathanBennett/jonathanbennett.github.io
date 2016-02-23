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

I spent a lot of time looking into the various "roll your own cloud" services that are all trying to emulate the *fantastic* work by my very good friends over at the Heroku HQ, however no one seems to make it simple, easy to get started and frankly not cumbersome. From reviewing the work of the CloudFoundry guys, which is battle tested and very stable, the Kubernetes group, which is much newer, a little more open-standards orientated but in my honest opinion over complex, and the various Kubernetes based PaaS services such as Deis, OpenShift and others, I was left asking myself why there wasn't just an easy, quick, way to get started.

*Disclaimer: I know that the approach I outline below will feel over-simplified for many of the CloudFoundry/Kubernetes hardcore users out there, and I have to say although I haven't found anything I **can't** do with the approach below, I may come up against something in the near future for which I'll look back at this post and say "wasn't he stupid?"*

There are many major problems that I intend to tackle through a simple, unix-like approach of choose individual components which do the job at hand incredibly well:

1. Auto-discovery / DNS,
2. Container orchestration through scheduling,
3. External load balancing,
4. Central log aggregation and search,
5. Container metrics and monitoring.

And what I'm going to do is take each one in order and break down what decisions, investigation and methods I've found so far.

# Automation at the centre

At the core of everything we do now, we have prioritised automation, auto-scaling through said automation and elasticity at the heart of everything we do. Before your team makes the decision to automate everything there's always some hesitance to automate. It feels clunky and counter intuitive to teams who are so used to doing everything by hand, even when by said hand they make mistakes. From the beginning of this project I focussed on automating everything through Ansible. The reasons for choosing Ansible were simple:

1. It was simple enough to get started that I didn't need to send the entire team on a training course for them to get going
2. The agentless strategy makes me feel much more comfortable in understanding rather than the approach of, say for example, Chef.
