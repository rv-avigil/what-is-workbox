# Handling Network Requests with Workbox

## What is Workbox
+ Library developed to generate service worker request caching strategies
+ Implemented after requests are already written
+ Allows for handling offline requests

## Common Workbox Strategies
### Cache First
```
workbox.routing.registerRoute(
  new RegExp('/styles/'),
  workbox.strategies.cacheFirst()
);
```

+ If cache exists, resolve request from cache, else fallback to network request.
+ Useful for content that you don’t expect to change often.
+ Scenarios
	+ System defined options for a form
	+ Google Fonts

### Network First
```
workbox.routing.registerRoute(
  new RegExp('/social-timeline/'),
  workbox.strategies.networkFirst()
);
```

+ If network available, resolve request from network, else fallback to cache.
+ If no cache is available, handle the exception thrown from the network request.
+ A successful network request will update the cache.
+ Useful for content that you expect to change regularly, and want to persist information in a cache.
	+ News feed
	+ User profile #debatable

### Network Only
```
workbox.routing.registerRoute(
  new RegExp('/admin/'),
  workbox.strategies.networkOnly()
);
```

+ If network available, resolve request from network, else handle the offline exception.
+ Provides a mechanism to attach additional behaviors and workbox plugins.
+ Useful for content that does not have an offline equivalent.
	+ Analytics

### Stale While Revalidate
```
workbox.routing.registerRoute(
  new RegExp('/images/avatars/'),
  workbox.strategies.staleWhileRevalidate()
);
```

+ Data is served from the cache while a network request is used to populate the cache.
+ Network responses are not fed to the app. Only to the cache.
+ Common to alert the user of available updates to the page.
+ Should be used only when you can afford to render stale data to get *time to first paint* as low as possible.
+ Useful for frequently updated resources of non-essential data.
	+ Avatars

## Requirements
+ Workbox is a bolt-on library that does not require much, if any, change to your current applications architecture.
+ Listens for network requests to implement caching strategies eventfully.

## Honorable Mentions
+ Background Sync for Google Analytics
+ Pre Cache

## References
[Strategy Reference](https://developers.google.com/web/tools/workbox/modules/workbox-strategies)

[Offline Cook Book](https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/)
