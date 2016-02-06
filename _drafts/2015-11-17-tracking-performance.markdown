---
layout: post
title:  "Tracking Performance"
date:   2015-11-17
tags: performance, nowtv
---
Over the last month I've been evaluating performance at NOWTV, specifically transactions per second and response times. We run performance tests pre deploy and regularly every week with different blends/profiles. Frequent testing is key to our success, ensures bugs don't creep in to slow existing services and new APIs or consumers don't change our blends and cause knock-on effects.

We've had our share of performance woes and spent time resolving these issues. My colleague Tom Maule covers in his presentation "[NOW TV Linear Streaming, Unpredictable Scalability Challenge](https://www.parleys.com/tutorial/now-tv-linear-streaming-unpredictable-scalability-challenge)" in detail the issue and fix. Tom's presentation title nicely summarises the challenge; un-predictable linear streams. Every traffic profile I've viewed from previous years statistics is similar when there is a single large event taking place.

<<< GENERATE FOOTBALL LOAD GRAPH >>>

Un-predictably we don't always know what our customers will peak on, sometimes it's the most unusual shows or events; in my opinion anyway.

But on top of streaming, we also need to be capable of high purchase throughput, the below day shows a live event clash, 2 very popular events on the same day, happening at similar time periods - this is a nightmare scenario.

<<< GENERATE SPORTS, AND ENTS PURCHCASES/MOVIES >>>

Both Streaming and Purchases impact different systems and have very different profiles.

### Upstream & Distribution of profiles
It became evident early on that the upstream has a big impact on the downstream, traffic is not linearly resolved.

<<< SHOW TABLE WITH PEAK DRIFTS >>>

In a single second

    1 transaction in != 1 transaction out

We see the above more when there is no upstream or the upstream is fast, purchase is slow; a bank is involved and has a different profile

    Requests in
      18:00:00  3 transactions

    Responses
      18:00:00  1 transaction
      18:00:02  2 transactions

Drift that further, response times range from 1 second to 3, we end up with the above table and differing response times.

We performance test with an amount of static drift, the average. We have production data with actual response times, we should use them and randomise the time per request from max to min when we stub the system. After all its the unpredictable that catches us out, it's far too easy and we're great at testing the predictable.

### We use averages and peaks for testing
We've seen traffic profiles cause drift, so when we consider load testing, we should use an average and blend the averages. When we're stress testing, we use the peak and sustain based on the profiles we see above.

Production traffic for 3 years gives us a very accurate picture, if the deviation is minimal it points towards these profiles being a good starting point. If we can cope with a sustained peak then the average is easy.

Anyone familiar with Splunk should understand the below, in essence.

<<< SHOW SPLUNK CODE >>>

Count the number of transactions per second, find the peak in a minute, find the peak minute within an hour period and group by day and hour.

As an example we end up with something similar to below (Note these are not live statistics, I've randomized them - If you want official figures then I refer you to Sky press office...).

<< EXAMPLE OF GROUPING OF PEAK >>

### Don't rely on forecasts or actuals

Ignore everything above, there are caveats with actuals, forecasts are either too lower or too hight, un-predictability creeps its head again.

My stance now is make forecasts or actuals your baseline, but in essence strive to create a system that has far more capability, we use the general rule of double it, I'm not sure this is accurate, we should use the % increase, e.g. 100 tps becomes 200 tps - the laughs ensue when we watercooler talk about HSBC ATM network processes 200 tps, we're not a bank.

100 tps with a 20% uplift (new customers/returning we expect) 120 tps

Interpretation of the data is everything, something I've learnt during this process, when to use averages or not rely on them when mean and standard deviation suggests against it.

I'm not an experienced load tester but it has been eye opening to see how we performance test and digging through real data, I've spent many hours looking at tps for streams, but never deep dived into payments tps which is a fascinating different profile and blend entirely.
