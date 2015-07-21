# A Guide to Service Worker: Push and Notifications

***



- push notifications
- also discuss push without notifications


Browser notifications do not depend on service workers and are already used by many applications [DO NEW FEATURES OF NOTIFICATION API REQUIRE SW?]. But they become much more powerful when paired with service workers and the Push API, becuase it becomes possible to trigger notifications from the server which can then be displayed by the service worker as long as the browser is open (or when the browser opens), regardless of whether or not the site itself is open.

In principle the Push API should be useful for many things besides receiving and triggering notifications; it could be used to trigger a sync of new data to the client or a refresh of cached assets before the user even opens the app for the day [OTHER EXAMPLES? SYNC API WILL ALSO HANDLE SOME OF THESE CASES, MAYBE BETTER]. For now, though, implementation of the Push API has been limited in such a way as to make it usable only for notifications, so that's what we'll illustrate. [CHECK FF STATUS]










You'll need a manifest:







[Note that you must show something]
[complications around lack of payload duplication, right content etc]
[multiple users on browser, multiple devices, etc]















## Resources

- Matt Gaunt, [Push Notifications on the Open Web](http://updates.html5rocks.com/2015/03/push-notificatons-on-the-open-web)
- Push API: [Repository](https://github.com/w3c/push-api), [Specification](http://w3c.github.io/push-api/)
- Notifications API: [Repository](https://github.com/whatwg/notifications), [Specification](https://notifications.spec.whatwg.org/)

***

[Home/Table of Contents](README.md)