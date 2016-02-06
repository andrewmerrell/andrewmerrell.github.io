---
layout: post
title:  "AngularJS with 900+ DOM elements"
date:   2014-07-28
tags: angular, nowtv
---
Recently we've been working on a new feature for watch.nowtv.com to enable customers to view the entire movie catalogue on a single page. Which we dubbed "[movies explorer](http://watch.nowtv.com/movies/explorer)".

We switched to [Angular JS](https://angularjs.org/) over a year ago now, it's been a long journey and taken us a while to find our feet. Personally its been a big and long continuous learning curve. We spent a lot of time in the run up to the launch of this new feature moving to a single page application (SPA).

![NOW TV Movies Explorer](/assets/images/Screen-Shot-2014-07-14-at-21-22-23.jpg)

The initial requirements were to allow sorting, filtering (with predefined options) and the ability to search movies. A requirements from ourselves (and design) was to create a smooth and slick experience when interactive with the explorer.

###Optimising 900+ JSON objects
Before we began creating and interacting with the DOM we needed to optimise the initial request. Our upstream API returns a response in 2.40s which is about 2.8MB in size. We created a simple web service to only return the required data from the API and do an initial filter server side.

We have the added benefit of using a CDN (Akamai) and turn on GZIP compression, the response time is now 405ms and 176KB in size.

The initial request to the origin API still hasn't changed so we cache the response in our app servers and also on the CDN. We're investigating using pre-fetch so the CDN takes the ~3s hit rather than a customer, otherwise we could write our internal cache to pre-fetch.

###Working with 900+ DOM elements in Angular
Suffice to say we hadn't heard great things when it comes to Angular's performance with lots of DOM elements.

Initially we were using ng-repeat with angular filters. I have to say it beat my expectations (considering what I had read about the subject). However ng-repeat was built for the masses and can only become performant to an extent. We can't customise ng-repeat for our need; render once, no bind - we don't update the DOM after rendering.

We had to rule out the follow:

*  Pagination
*  [Infinite scroll](http://binarymuse.github.io/ngInfiniteScroll/)

Our use-case didn't allow for these two techniques but a quick google will find out loads of examples of them in use.

####Bind-once
Data binding can be expensive and each `{% raw %}{{expression}}{% endraw %}` or `ng-bind="expression"` will set up a $watcher for this expression. Creating a large dirty checking list (how angular watches for changes).

Since our data is only rendered once and never updates we can use bind-once to bind the data to the DOM and then remove all $watch expressions.

https://github.com/Pasvaz/bindonce

**Update [25/08/14]:** As of Angular 1.3 bind once will be built in by prefixing expressions with `::` for example `{% raw %}{{::item.name}}{% endraw %}` more information at https://code.angularjs.org/1.3.0-beta.19/docs/guide/expression#one-time-binding


####Avoiding Angular filters
We opted for updating the elements class rather than using the built in Angular filters, filters essentially copy the entire Array leaving only the correct results.

Combining 3 filters on top of each other will cause 3 potentially large arrays depending on the number of items in each one.

Looping over all items and updating the element allows the browser to apply a new set of CSS rules over the updated DOM tree.

We could've spent a bit more time further optimising this to reduce repaints and layout operations, but the performance as is was relatively good.

#####Searching
When entering input characters there was a noticeable delay, angular implements searching with a filter which loops over all items and returns again another array with only matching results.

We decided to not use the filter approach and again update a value on the element to which certain CSS rules apply to show and hide the item.

By updating the element we stop the DOM from constantly being destroyed and recreated (although Angular is intelligent and trys to reuse cached elements where possible in ng-repeat; see [track by](http://www.codelord.net/2014/04/15/improving-ng-repeat-performance-with-track-by/))

For further optimisation here we can debounce or throttle the input field too.

####Repaints and Layout
Updating the DOM item by item will also cause a lot of repaints again this is how ng-repeat by default works. We decided to use `createDocumentFragment` and build up all the items using $compile and a relevant scope with a loop and then after building all the DOM items which are applied to the document fragment, this entire fragment can be applied at once, resulting in only a single paint.

####Non-blocking
Using a setTimeout and split up the work over multiple ticks this gives processing time back to run UI updates whilst chunking though the data.

We haven't implemented this yet, but its defiantly a an improvement I feel could be beneficial.

###Measuring performance
Over the next few months I hope to track the load time from initial request to final rendering and various other stats, it will also allow us to baseline now and see real user experience.

When making future updates we can measure after a release to hopefully achieve performance improvements, but most importantly to ensure we didn't make it worse.

###Updates
**[25/08/14]:** Change to the bind-once section
