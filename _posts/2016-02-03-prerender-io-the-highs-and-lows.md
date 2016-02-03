---
layout: article
title: Prerender.io - the Highs and Lows
modified:
categories:
excerpt:
tags: []
image:
  feature:
  teaser:
  thumb:
date: 2014-04-29T22:04:51+00:00
comments: true
---

So you're writing a killer single page application - everything's going well so far and you're thinking about the many ways in which this application will change the way your users feel about you.

Then *it* happens.

Search engine optimisation. The most mystical of all of the web practices. The list goes on, and on. You're not sure whether to break the unfortunate news that JavaScript applications, those that only render on the client in the case of single page apps, can't be crawled because crawlers still seem to be behind and don't render JavaScript.

Screwed. Back to the drawing board. Time to pick up the pieces and start the rebuild. But then, decending from the heavens...

[Prerender.io](https://prerender.io/) - The answer to all your prayers. [1]

Prerender is a very lightweight node application, making use of PhantomJS which, in it's own words, 'Allow your Javascript apps to be crawled perfectly by search engines'. Prerender also provides a set of middleware for your applications which detect user agents that need to be served the static version either through the [AJAX crawling scheme](https://support.google.com/webmasters/answer/174992?hl=en) or by User agent switching to stop SEO cloaking penalties. In our project we are using node, so the node middleware has been installed to manage the prerender relationship and for the most part it's been okay. A week out from Go-live, though, we have hit a couple of snags... (These might not be an issue for everyone, but I thought I would detail them all the same in case it helps anyone else.)

### External Script Loading & RequireJS
Although intrinsic to the way the application works (we use Backbone with RequireJS to do AMD), we have had mixed experiences with external script loading through PhantomJS. As part of the configuration of the server, Phantom has an ability to supply a "local-to-remote-url-access" which allows access to external files on the Internet. This has caused particular headaches for us as we have had to program workarounds into standard packages that we use for tracking which require the use of shims to support loading the scripts through Require (GA is not AMD compliant) caused particular issues which we couldn't find a way to work around without merely programming defensively to know whether Prerender was being used. With no reason to believe that these scripts wouldn't be available this problem went away. We ended up using something similar to the following:

`if (!/(prerender)/i.exec(window.navigator.userAgent)) {
	// require files like usual
} else {
    // be sad and hate life
}`

### Multiple "worker" threads
Prerender out of the box suggests using multiple PhantomJS workers to handle more requests in parallel. In our testing we haven't found that this results in higher performance, especially as we host the prerender service on heroku, and have found that setting it to just 1 worker has resulted in near instant PhantomJS reads. I am yet to fully understand why this is the case but with further investigation it may make sense. I can only imagine there is maybe something blocking at this point in time which causes Prerender to stop serving any more requests.

### Heroku hosting gotchas
Prender out of the box has a 3 line startup set of instructions;

`$ git clone https://github.com/prerender/prerender.git
$ heroku create
$ git push heroku master`

and that works fantastically. The only thing we realised quite quickly is that when you use an in memory cache (the default middleware configuration) instead of the s3 plugin, which seems to as of right now have no automated way of expiring the s3 stored assets after a period of time, and distribute that workload across multiple dynos you will soon find that you have to hit each page on the application mulitple times to get the benefits of any caching in memory as, of course, it is not shared across dynos. Not the biggest deal, but it caught me by surprise for a few minutes.

# Conclusion

All in all, Prerender.io has really been the answer to prayers, and although it isn't a new idea (we've often talked internally of headless browsers crawling websites and either archiving snapshots regularly or serving back to crawlers) it has packed it up neatly into an application that does make sense. Going forward, I will look to continue using Prerender more. Thanks again, prerender team!

[1]:https://github.com/prerender/prerender
