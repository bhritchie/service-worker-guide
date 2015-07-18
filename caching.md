# A Guide to Service Worker: Caching
***


- general managment issues
- the api in detail




The caches you create are separate from the browsers own cache. Objects wind up in your caches only if you put them there, and are removed only if you delete them. So you're responsible for maintaining your caches. Several things make this task a little bit more diffecult. First of all, you cannot set an expiration on cached objects. Secondly and relatedly (and this is true for all browser storage), the browser may throw away your cached objects if it decides that you are using too much space, or there is otherwise some sort of storage presuure on teh browser. You should assume that this will sometimes happen, especially if you pursue a relatively indescriminate caching strategy, such as caching everything that gets fetched. There are proposals for allowing web apps to request permanent storage, but these are still in the planning stages [CITE RELVANT SPECS]. And, finally, there is at present no way to determine how much if any space is available to you, except by actually getting an error when you try to save something. [TEST GETTING AN ERROR AS A RESULT OF CAHHING TOO MUCH][reference this article: http://www.html5rocks.com/en/tutorials/offline/quota-research/] This at least should change eventually—the planned [Quota Management API](
https://w3c.github.io/quota-api/) would allow you to do something like this this to help you decide whether you should save more objects to the cache or maybe do some pruning:

    navigator.storageQuota.queryInfo("temporary").then(function (storageInfo) {
      var availableSpace = storageInfo.quota - storageInfo.usage;
    })

But again this is not yet implemented in any broswer and the specification may well change considerably as there are several proposals in this area. [FOOTNOTE A BREIF DISCUSSION]


[SME TRANSITION - IN SOME CASES MAYBE THERE'S NOT THAT MUCH THAT NEEDS TO BE MANGED]


One good approach is to version your caches and take advantage of the activation event to clear out anything you no longer need.[FOOTNOTE: This approach is recommended and demonstrated in [a sample](https://github.com/GoogleChrome/samples/blob/gh-pages/service-worker/read-through-caching/service-worker.js) from the [GoogleChrome github repository](https://github.com/GoogleChrome/), from which the current sample is derived.] You will store the names of your current caches in a variable, and perhaps place objects into them when the service worker installs or as they are fetched. When the assets change, you bump the CACHE version variable. This changes the file, so the brower will see this as a new service worker and install it, allowing it to perhaps cache some assets in cache named by `CURRENT_CACHES['assets']`, which is, say, `assets-v-3`. When all existing pages are closed or refreshed and the new service worker takes over from the previous ones (i.e. gets the `activate` event), it checks the existing set of caches and deletes any that aren't listed in CURRENT_CACHES any longer (e.g. `assets-v-2`).

    var CACHE_VERSION = 1;
    var CURRENT_CACHES = {
      'assets': 'assets-v' + CACHE_VERSION
    };

    self.addEventListener('install'), function(event) {
      event.waitUntil(
        // cache some stuff in CURRENT_CACHES['assets']
      );
    }

    self.addEventListener('activate', function(event) {
      var expectedCacheNames = Object.keys(CURRENT_CACHES).map(function(key) {
        return CURRENT_CACHES[key];
      });

      event.waitUntil(
        caches.keys().then(function(cacheNames) {
          return Promise.all(
            cacheNames.map(function(cacheName) {
              if (expectedCacheNames.indexOf(cacheName) == -1) {
                return caches.delete(cacheName);
              }
            })
          );
        })
      );
    });


One thing I don't love about this approach is that it will require a couple of page loads before the new caches are actually being used (unless other open windows delay this even further). In some cases you could use some combination of `skipWaiting()` and `clients.claim()` to have the new worker take over more quickly (see the chapter on The Service Worker Lifecyle above). Another alternative is to have the service worker manage the cache more actively, independently of it's own versioning. We used such an approach at Sandglaz. [DESCRIBE IT]

[SIMPLIFIED CODE SAMPLE]














## Resources

- Jake Archibald, [The Offline Cookbook](http://jakearchibald.com/2014/offline-cookbook/)


http://www.html5rocks.com/en/tutorials/offline/quota-research/


[READ UP ON ALL THESE ISSUES ETC IN HERE - RELATIONSHIP AND STATUS SEEMS A BIT UNCLEAR]
Storage Durability:
https://github.com/slightlyoff/StorageDurability


Quota Management API:
https://github.com/w3c/quota-api
https://w3c.github.io/quota-api/


Storage Spec:
https://storage.spec.whatwg.org/
https://github.com/whatwg/storage


Permissions API (may wind up include storgae related permissions):
https://w3c.github.io/permissions/
https://github.com/w3c/permissions


***

[Home/Table of Contents](README.md)