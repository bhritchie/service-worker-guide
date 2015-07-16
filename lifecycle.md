# A Guide to Service Worker: The Service Worker Lifecycle
***





This can lead to surprises. For example, the first time a page with a service worker loads, you'll register the service worker, and then you might attempt to get permissions to provide push notifications, which we'll assume the user grants.


    // Simplest possible illustration of registering and looking for push permissions
    navigator.serviceWorker.register('/service-worker.js', {scope: '/'})
    .then() //FINISH THIS


At this point you have a service worker registration, an active service worker (assuming this was a brand new worker, and not replacing another worker at the same scope), and a push subscription. However, push notifications will not work yet. This is because the page is not *controlled* by the worker yet. A service worker only becomes a controller on navigation. This means that you'll need at least a page refresh before your notifications start to work—if you have other pages open to the scope you'll need to close them first. You can check whether the page is controlled by a service worker by seeing whether `navigator.serviceWorker.controller` is defined.

    

[why should this fail if there doesn't even need to be a controlled page? somehow the worker has a status that is tied to having once been a controller of a page? what if the page had been closed?]





## Resources



***

[Home/Table of Contents](README.md)