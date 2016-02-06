---
layout: post
title:  "Angular services and models combined"
date:   2014-12-20
tags: angular, nowtv, crosstv
---
Since using Angular from the very early days the framework has changed a lot and we've also built services and factories in multiple ways, looking back at them all I always ask myself is it the "Angular way"?

I am becoming less concerned with this now and am focusing on what works best for us, since starting a new project we've reimagined the way we work with services and factories.

We push as much logic up to the service as possible keeping the controller's as they should be nothing more than simple interfaces to the service, this makes them easy to test so if we use a service in multiple ways we don't have to drag complexities throughout the controller each time.

We expose all our responses on a simple model property that the service returns, for consistency we access the data from the same place each time. In the past we had to remember to call `getThisData().title` or `getThatData().title` we simply just call `model.title` after doing a `fetch()`.

I like this syntax as its easy to get data and easy to set up watchers and bind models directly to the scope if required.

However if we need computed values, we do all transformation after fetching in the service, doing it only once. Previously we placed calculations in the getters, with every call we execute the function, and doing it on the scope meant Angular re-evaluates on digest cycles to check if the value has changed.

	function getImage() {
    	return this.image.replace('{width}', 240);
	}

Our first move was to re-build the function with a cache:

	var imageCache = {};

	function getImage() {
		if(!cache[this.image]) {
			return this.image.replace('{width}', 240);
    	} else {
			return cache[this.image];
    	}
	}

It made our functions for individual properties more complex. We now simply do `transformImage()` after the `$http.get` promise resolves.

	function transformResponseImage(image) {
		return image.replace('{width}', 240);
	}

We use `angular.copy()` over our model on the service to update it with the response back from the server `angular.copy(model, transformedResponse)`.

Putting it all together we've ended up with simple and easy to read services that work well with Angular's life cycle (and hopefully Object.observe in the future) and our view controllers.

The approach above we could've used filters in some places to further abstract the logic into re-usable components. But we would've still been impacted by running the logic multiple times. Our specific use-case is one where performance matters, so I favour doing the transformation once in the service.

Example of our factory:

	angular.module('contentDiscovery')
    	.factory('Programme', function (RestClient) {

          var model = {};

          function transform(response) {
              // Data transformation...
              return response;
          }

          return: {
              model: model,
              fetch: function (programmeId) {
                  return RestClient.cachedHttp('/programme/' + programmeId).then(function (response) {
                      return transform(response);
                  });
              }
          }
      });
