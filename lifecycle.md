# A Guide to Service Worker: The Service Worker Lifecycle
***

[API as related to lifecycle]
[registration]
[unregistration]
[update]

- Outlasting pages
- activate and intall etc
- skipWaiting and client.claim
- closing and reloading pages for control
- updating and replacing workers
- short lifespan
- events




The lifecycle of a service worker is completely different from that of a web page. In particular it outlasts the page that registers it. This allows it 









This often causes confusion [WHAT DOES?]. For example, suppose you loaded a page with a new service worker, the client code being as follows: [TEST THIS]

    navigator.serviceWorker.register('service-worker.js', { scope: "/" });

    navigator.serviceWorker.ready.then(function() {
      console.log('Service worker is ready');
    }

At this point you have a service worker registration, an active service worker (assuming this was a brand new worker, and not replacing another worker at the same scope), but `serviceWorker.ready` will not resolve. This is because the page is not *controlled* by the worker yet. A service worker only becomes a controller on navigation. This means that you'll need at least a page refresh before your notifications start to work—if you have other pages open to the scope you'll need to close them first. You can check whether the page is controlled by a service worker by seeing whether `navigator.serviceWorker.controller` is defined.

This can lead to puzzling or undesirable behaviour when the service worker is active and otherwise running normally. Take for example the case of loading a new service worker that handles push messages and displays notifications. The first the page with the worker loads, you'll register it, and then attempt to get permissions to provide push notifications, which we'll assume the user grants. Some minimal front-end code will look something like this (we'll look at a complete example in the chapter on [Push and Notifications](/push.md)):

client.js:


    // Simplest possible illustration of registering and looking for push permissions
    navigator.serviceWorker.register('/service-worker.js', {scope: '/'})
    .then() //

    // MINIMAL CLIENT CODE SAMPLE

worker.js:

    //MINIMAL WORKER CODE SAMPLE  



At this point you have a service worker registration, an active service worker (assuming this was a brand new worker, and not replacing another worker at the same scope), and a push subscription. Push notifications will now work—the age isn't being controlled, but it doesn't matter for handling a push whether or not any page is controlled. When the user clicks on a notification, the service worker is supposed to open an active window if available, or a new window if there is not an actuve window.

If you now fire a notification and click on it, the worker will open up a *new* tab instead of focussing the existing one.




    

[why should this fail if there doesn't even need to be a controlled page? somehow the worker has a status that is tied to having once been a controller of a page? what if the page had been closed?]



---



If the lifecycle of a service worker is far longer than that of the pages it controls, in another sense it's lifespan may be very short. The browser can and will shut it down whenever it pleases, and then wake it again to handle events. You cannot have the service worker wake itself or keep itself awake, either. For example, you might be tempted to use `setTimeout()` or `setInterval()` to schedule an check for application updates—something like this:

[ILLUSTRATE SETTING AN UPDATE CHECH IN ACTIVATE EVENT]

You might even think this is working if you examine it for a few cycle with the developer tools open, because the browser will often, as a courtesy, keep a worker alive longer while a page is actively being controlled or while you're inspecting the service worker. But once you shut the page or the developer tools, the browser will kill the service worker (if it hasn't already), and nothing will happen when the next interval rolls around. And while you can call `event.waitUntil()` to extend the life of an event, if you return promises that take a very long time to resolve or fail to resolve at all, eventually the browser will decide that something abnormal or overlong is occurring and shut you down anyway. [TEST THIS]

Service workers are woken to handle events. There are currently five of these events [CORRECT?].


[MAYBE SELECT LIST OF EVENTS HERE - WOULD BE NICE TO HAVE A COMPLETE LIST OF EVETS THAT CAN WAKE A SERVICE WORKER ALL IN ONE PLACE - PERHAPS IN AN APPENDIX]

INSTALL

ACTIVATE

FETCH

MESSAGE

PUSH

NOtification CLick?


Eventually there should be more events as additional APIs are developed - see chapter on other related APIs.








## Resources



***

[Home/Table of Contents](README.md)