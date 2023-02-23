# Progressive Web App (PWA) Offline Browsing


## Note
 - The most attractive feature of progressive web app is offline browsing, that means it works offline
 - My study is mainly focus on offline browsing
 - It seems not very worthy to support offline data upload, while other tools like ODK Collect already support working offline
 - It is not necessary to deep dive service worker Javascript program source code, someone developed a package to support offline browsing already

If you need a comprehensive introduction for progressive web apps (PWA), please visit: https://web.dev/learn/pwa/


## What Does Progressive Means?
 - The word "Progressive" means it is continuing to have new features from time to time, you can add new PWA features to your web site progressively
 - Project Fugu, to track capabilities in different platforms


## PWA Features
 - It is installable as a standalone application
 - It has an icon on the home screen, app launcher, launchpad, or start menu
 - It has a separate window within the operating system, totally seperated from browser user interface
 - It shows a splash page (with app icon and app name) when launching app
 - It works offline


## Browser Cached Storage
 - Server returned responses can be stored into browser cached storage for offline browsing in future
 - It stores the whole response from server
 - Because it is in request / response level (i.e. more in HTTP level). It is regardless of server side and client side programming languages


## How Many PWA Features Are Supported In User Device?
 - It depends on which browser and version that is being used in user device
 - It is hard to guarantee from developer side, because user may use any browser with any browser version


## Chromium Engine
 - PWA is mainly powered by the Chromium engine, e.g. Google Chrome, Microsoft Edge
 - Firefox does not support PWA
 - Safari partially supports PWA in iPhone
   - Apple does not officially mention they support PWA
   - iPhone Safari supports offline browsing feature, but it does not support app installation feature and splash screen feature

Progressive Web Apps - Cross-browser compatibility

https://web.dev/learn/pwa/progressive-web-apps/#cross-browser-compatibility

Progressive Web Apps Compatibility

https://firt.dev/notes/pwa


## Service Worker:
 - This is the main character to support offline browsing
 
 - When the web app is visited at the first time
   - We can define what files to be downloaded to browser cached storage

 - When user is using the web app, we can apply different caching strategies:
   1. cache first, use cache first if exist, use network if no cached response
   2. network first, use network first, use cached responsed if no network connection
   3. stale while revalidate, send request and store the response for next time, show the previously cached response for this time
   4. network-only, send request via network and show latest response
   5. cache-only, show response from cached storage

Service Worker

https://web.dev/learn/pwa/service-workers

Serving

https://web.dev/learn/pwa/serving/

 - However, none of the above caching strategies meet our needs. What we need:
   - When user is online: send request to server, receive response from server, show response to user, store response to browser cached storage
   - When user is offline: show previously cached response if user visited this page before. Otherwise show a static offline page


## Laravel Package "Laravel PWA"

"Laravel PWA" can do what we need:

https://github.com/shailesh-ladumor/laravel-pwa

P.S. Please note that there is a modification required in one program file "laravel-pwa/src/stubs/sw.stub"

```
line 53
change from: if(!event.request.url.startsWith('http')){
change to  : if(event.request.url.startsWith('http')){
```

### Browser Dev Tools [F12] Application Tab

 - Available in Google Chrome, Microsoft Edge
 - Not available in Firefox
 - Allows you to view PWA details, e.g.
   - Manifest (app name, app icon, background color, etc)
   - Service workers
   - Cached storage

Tools and debug - Web app manifest tools

https://web.dev/learn/pwa/tools-and-debug/#web-app-manifest-tools


## File Description For PWA Files

1. logo.png: app icon, normally with 512 x 512 pixels

2. manifest.json: app details including app name, app short name, app description, starting url, background color, theme color, app icons

3. offline.html: a static HTML page to be showed when there is no cached response when offline

4. sw.js: Javascript functions make use of service worker library

5. app.blade.php: This is a PHP blade view file to be included for all pages. To include app icon, manifest file, service worker Javascript file in home page


## Offline Browsing Considerations

 - Be aware and be responsible for what you store in user's browser cached storage. As the responses could occupy huge storage space in user computer and/or mobile phone.

 - There is a storage limit for each PWA. (TBC for exact limit). Use storage space wisely to store essential responses only.


## Vue Component With Filter Feature

  - For a normal web app, we send request with filter parameter to server, server do the filtering, and then returns the filtered result
  - For offline browsing, we may always get the same result from server, then do the filtering at Vue compoenent level

  - Pros: store one response in cached storage when online, then it can support multiple enquiries when offline
  - Cons: more network overhead when online, server always returns full dataset
  
  - Concerns: 
    - We need to balance the performance (i.e. loading time) and technical feasibility (performance is different when showing 100 records and 100,000 records...)
    - It is good if we could estimate how many records in response
  
