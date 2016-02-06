---
layout: post
title:  "Server Rendered Javascript Applications"
date:   2016-02-01
tags: isomorphic, nodejs, react
---
In late 2013 airbnb bought our attention back to [isomorphic javascript applications](http://nerds.airbnb.com/isomorphic-javascript-future-web-apps/), whilst not the original inventors the post gave detailed explanation of how they achieved isomorphic javascript with nodejs. Given the year nodejs was still in production infancy. Therefore the topic of running the same app both server and client side produced interesting conversations.

Our team discussed the merits and disadvantages but our ultimate blocker was our framework, we'd only just picked up Angular and there was no server side support built in.

### Single page applications

The idea of rendering a single page application (SPA) server side had a host of benefits, most notably time to first render and SEO. SPAs can grow exponentially, with our choice framework, its all or nothing, no dynamic loading of modules. Currently the javascript alone sits at about 277kb minified and gzipped, only once loaded can the application boot and render. Performance takes a big hit and we optimised where possible to make the user impact small.

From our first SPA we had to address the potential SEO issue, SPAs had little on offer to support crawlers. Google only last year added support for SPAs, we've relied solely on prerender.io to serve up a static only version of our site to crawlers.

React has again peaked our interest in isomorphic applications, a library that makes server side rendering simple. Most popular frameworks to date had little or no support.

### React & server rendering

We've recently built a number of applications in React, it give us the ability to server side render with a small nodejs app. The immediate benefit is to the user, time to first render is within the milliseconds and React is intelligent enough to know the application was rendered server side and only load state and not re-draw. The user shouldn't realise the initial render is simply a static site before the javascript loads.

Our SEO issue is also none existent and no additional tools are required.

### The year of isomorphic js

At the beginning of 2016 Sky initiated a project to review existing technologies and form a strategy for adoption of new technologies and ultimately to their deprecation. Whilst we haven't considered future trends as part of this work, we prefer to empower teams to asses new technologies, it begged the question to myself. What is in store for 2016?

A hard question to answer in the web space, given it changes rapidly; are we using grunt, gulp or webpack now...? Change is never a bad thing but it does make prediction difficult. I'd never have imagined Facebook releasing React followed by the onslaught of conversations, angular vs react with flux thrown in for good measure. Plus the other numerous short lived javascript trends... I can't even remember.

One prediction I will attempt to make, this year isomorphic applications will finally gain popularity.

#### Ps.
Flux is simply PubSub but with a bit more structure and models right? The pattern still confuses me... I really should attempt to implement it.
