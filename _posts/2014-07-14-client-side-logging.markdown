---
layout: post
title:  "Client side logging"
date:   2014-07-14
tags: logging
---
Server side logging; been doing it for years. However trends are changing, especially for our team, we are starting to offload logic from servers to the client using a javascript MVC and soon it will become a single page app. Our team will soon loose that valuable insight we get from server side logs.

Why is this a problem? Well for some maybe it's not, but for me this is a crucial loss, I find this data invaluable when debugging and more recently I use this data to chart the health of our application with real time analysis (I'll cover this in another post).

### So how do we capture this lost valuable insight?
Granted not a hard question to answer, there are many ways to capture client side errors; a simple google will prove this. The simplest method which we are currently using is watching window.onerror now we capture all JS errors thrown, however now we have a problem.

We have a lot of noise as we log:

1. Browser plugin errors.
2. External libraries.
3. Hacked machines? We've seen suspicious scripts throw errors.

###Browser plugin errors
Most modern browsers these days allow users to install browser plugins. They have the ability to put javascript into the page, sometimes throwing errors which we then catch, this data is completely useless and we don't need it and have the ability to do nothing about the errors thrown. So a prime candidate for not being logged.

### External libraries
When I mention external libraries I'm mainly referring to anything you include from CDNs or 3rd party dependancies.

I work with two main culprits; ad tracking and analytics libraries (Angular and jQuery also fall in here too, but they don't usually cause problems). Whilst the latter usually has no issues the former happens to managed by an external agency, who have free will to put in any js onto our site they like, but we can't escape for the time being.

Why log these errors then? We can now see if a particular library or piece of JS they've installed throws errors. Insight we never had before, unless we experienced the issues (the eat your own dog food approach). We can evaluate our site with the same set up our customers have, lets face it the browser/OS combinations are diverse. Way more than we would be able to test ourselves. We can then pass the errors across to our external agency to resolve.

### Hacked machines
I may be making a few assumptions here, but from my investigations it would seem like this is the case. I can see suspicious libraries throwing errors. What makes them suspicious, the JS is often obfuscated and huge, bigger than a plugin would include and not one our external agency includes. Also some googling of the library name and fragments I could pick out from the obfuscated code would seem to suggest the library is linked to suspicious activity.

Now we have some ethics questions:

* Do we inform the user their machine may be compromised?
	* What if we are mistaken? We would annoy the user by misinforming them?
* Should we recommend they run a virus scan?
	* Where do we send them and who do we recommend? McAfee, Symantec etc..

We have yet to answer these questions within our team, but we will eventually need to, do we continue to log these errors to our log server or remove them as noise. That will ultimately lead us to answering the above questions at the same time.

### What's our set up?

We listen to the window.onerror (we have a little wrapper) and send these errors to a log server, in particular to we use logentries.com (our server side logs have a different set up).

We attempt to tag the logs on entry so we can group them into categories in an attempt to make searching easier across the noise.

Our setup is and was incredibly simple to get set up and put into production.

### Conclusion
So far my experiences are positive, but we still have a lot of noise and are not yet using the logs similar to how we do server side. I am sure when the noise is reduced we will gain more value.

Right now the pain for me is filtering though to find our errors. Then comes the task of trying to recreate how the user go into the issue, with only errors it's hard to track what the user did up to this point to get such an error. Was it path based or simply a 1 off based on browser/OS?

As a team we're still in a steep learning curve, no one has had experience with client side logging before. We also have the added issue of trying to figure it all out in production, with thousands of lines streaming past our eyes on the console.

### Future
Within our team we refer to what I've been calling the log server as the event collector.

At the moment we send logs of type ERROR, our server side logs also include INFO and WARN. Should we extend this to the client side? Probably, we saw value to do it on the server.

I mentioned at the start I use this data to chart the health of our application with real time analysis. Whilst a lot of that is done via ERROR logs, key early warnings can come from the INFO logs when we combine this is path analysis. Giving a bit of background I work on a site which has traffic spikes, whilst most calls we make are lightweight, most of our journeys end in at least 1 heavyweight call. When we see a spike in INFOs at the start of a journey, we could give an early warning to the API team.
