# Progressive Web App (PWA) Offline Browsing

# Note:
 - The most attractive feature of progressive web app is offline browsing, that means it works offline
 - My study is mainly focus on offline browsing
 - It seems not very worthy to support offline data upload, while other tools like ODK Collect already support working offline
 - It is not necessary to deep dive service worker Javascript program source code, someone developed a package to support offline browsing already

If you need a comprehensive introduction for progressive web apps (PWA), please visit below web site:
https://web.dev/learn/pwa/

---

# What does progressive means?
 - Progressive means it is continuing to have new features from time to time, you can add new PWA features to your web site progressively
 - Project Fugu, to track capabilities in different platforms


# PWA features:
 - it is installable as a standalone application
 - it has an icon on the home screen, app launcher, launchpad, or start menu
 - it has a separate window within the operating system, totally seperated from browser user interface
 - it shows a splash page (with app icon and app name) when launching app
 - it works offline

---

# Browser cached storage
 - server returned responses can be stored into browser cached storage for offline browsing in future
 - it stores the whole response from server
 - because it is in request / response level, which is more in HTTP level. It is regardless of server side and client side programming languages

---

# How many PWA features are supported?
 - it depends on which browser and what is the browser version
 - it is hard to guarantee from developer side, because user may use any browser with any browser version


# PWA is mainly powered by the Chromium engine:
 - e.g. Google Chrome, Microsoft Edge
 - Firefox does not support PWA
 - Safari partially supports PWA in iPhone
  - Apple does not officially mention they support PWA
  - iPhone Safari supports offline browsing feature, but it does not support app installation feature and splash screen feature

Progressive Web Apps - Cross-browser compatibility
https://web.dev/learn/pwa/progressive-web-apps/#cross-browser-compatibility

Progressive Web Apps Compatibility
https://firt.dev/notes/pwa

---

Service Worker:
 - this is the main character to support offline browsing
 
 - when the web app is visited at the first time
  - we can define what files to be downloaded to browser cached storage

 - when user is using the web app, we can apply different caching strategies:
  1. cache first, use cache first if exist, use network if no cached response
  2. network first, use network first, use cached responsed if no network connection
  3. stale while revalidate, send request and store the response for next time, show the previously cached response for this time
  4. network-only, send request via network and show latest response
  5. cache-only, show response from cached storage

Service Worker
https://web.dev/learn/pwa/service-workers

Serving
https://web.dev/learn/pwa/serving/

 - However, none of the above caching strategies meet our needs

 - What we need:
  - when user is online: send request to server, receive response from server, show response to user, store response to browser cached storage
  - when user is offline: show previously cached response if user visited this page before. Otherwise show a static offline page

---

A laravel package "Laravel PWA" can do what we need:
https://github.com/shailesh-ladumor/laravel-pwa

P.S. Please note that there is a modification required in one program file "laravel-pwa/src/stubs/sw.stub"

line 53
change from: if(!event.request.url.startsWith('http')){
change to  : if(event.request.url.startsWith('http')){

---

Browser Dev Tools [F12] Application tab
 - available in Google Chrome, Microsoft Edge
 - not available in Firefox
 - allows you to view PWA details, e.g.
  - manifest (app name, app icon, background color, etc)
  - service workers
  - cached storage

Tools and debug - Web app manifest tools
https://web.dev/learn/pwa/tools-and-debug/#web-app-manifest-tools

---

File description for PWA files:

logo.png: app icon, normally with 512 x 512 pixels

manifest.json: app details including app name, app short name, app description, starting url, background color, theme color, app icons

offline.html: a static HTML page to be showed when there is no cached response when offline

sw.js: Javascript functions make use of service worker library

app.blade.php: This is a PHP blade view file to be included for all pages. To include app icon, manifest file, service worker Javascript file in home page

---

Offline browsing considerations:

 - Be aware and be responsible for what you store in user's browser cached storage. As the responses could occupy huge storage space in user computer and/or mobile phone.

 - There is a storage limit for each PWA. (TBC for exact limit). Use storage space wisely to store essential responses only.

 - For a Vue component with filter feature (e.g. search analysis result for different region)
  - for a normal web app, we send request with filter parameter to server, server do the filtering, and then returns the filtered result
  - for offline browsing, we may always get the same result from server, then do the filtering at Vue compoenent level

  - pros: store one response in cached storage when online, then it can support multiple enquiries when offline
  - cons: more network overhead when online, server always returns full dataset
  
  - concerns: 
   - we need to balance the performance (i.e. loading time) and technical feasibility (performance is different when showing 100 records and 100,000 records...)
   - it is good if we could estimate how many records in response
  