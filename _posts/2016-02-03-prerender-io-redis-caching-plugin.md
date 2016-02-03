---
layout: article
title: Prerender.io Redis Caching Plugin
modified:
categories:
excerpt:
tags: []
image:
  feature:
  teaser:
  thumb:
date: 2014-05-02T22:06:05+00:00
comments: true
---

Following on from my Prerender post [here](http://jonathanben.net/prerender-io-the-highs-and-lows/) and a suggestion from Todd at the prerender team, I went ahead and made a prerender plugin which uses Redis for caching to allow us to have a distributed cache across heroku dynos. You can find the plugin here: https://www.npmjs.org/package/prerender-redis-cache

Please feel free to fork and create a PR for any additional functionality you may require!

https://github.com/JonathanBennett/prerender-redis-cache

UPDATE:
Version 0.1.0 has been published to NPM which contains some connection handling (including bypassing the plugin if Redis has gone away), slightly more silent error messaging and general clean up and commenting.
