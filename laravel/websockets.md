# Setup Realtime Browser Notifications
To notify the user of server-side events, we can use websockets.

## 1. Install Laravel Echo and PHP Pusher

Follow the installation instructions [here](https://laravel.com/docs/10.x/broadcasting#client-pusher-channels)

    - `npm install --save-dev laravel-echo pusher-js`
    - setup Echo in the main app.js (or which-ever file is used as the entrypoint for JavaScript):
    
Then setup the PHP side
    - `composer require pusher/pusher-php-server`
    - Uncomment `App\Providers\BroadcastServiceProvider::class` inside `/config/app.js`
    

#### For Vite-based applications

```js
import Echo from 'laravel-echo';
import Pusher from 'pusher-js';

window.Pusher = Pusher;

window.Echo = new Echo({
    broadcaster: 'pusher',
    key: import.meta.env.VITE_PUSHER_APP_KEY,
    wsHost: window.location.hostname,
    wsPort: process.env.VITE_PUSHER_PROXY_PORT,
    wssPort: process.env.VITE_PUSHER_PROXY_PORT,
    disableStats: true,
    encrypted: true,
    cluster: import.meta.env.VITE_PUSHER_APP_CLUSTER,
    forceTLS: true,
    enabledTransports: ['ws', 'wss'],
});

```


#### For older Laravel Mix-based applications

```js

import Echo from 'laravel-echo';

window.Pusher = require('pusher-js');

window.Echo = new Echo({
    broadcaster: 'pusher',
    key: process.env.MIX_PUSHER_APP_KEY,
    wsHost: window.location.hostname,
    wsPort: process.env.MIX_PUSHER_PROXY_PORT,
    wssPort: process.env.MIX_PUSHER_PROXY_PORT,
    disableStats: true,
    encrypted: true,
    enabledTransports: ['ws', 'wss'],
});

```

#### For older Laravel Mix-based applications using Echo in Vue Components:

```js

import VueEcho from 'vue-echo-laravel';

Vue.use(VueEcho, {
    broadcaster: 'pusher',
    key: process.env.MIX_PUSHER_APP_KEY,
    wsHost: window.location.hostname,
    wsPort: process.env.MIX_PUSHER_PROXY_PORT,
    wssPort: process.env.MIX_PUSHER_PROXY_PORT,
    disableStats: true,
    encrypted: true,
    enabledTransports: ['ws', 'wss'],
});

```

## 2. Update Laravel Config

Confirm that the config/broadcasting.php file includes the following entry. (Replace the existing `pusher` entry if it exists):

```php
    'pusher' => [
        'driver' => 'pusher',
        'key' => env('PUSHER_APP_KEY', 'app-key'),
        'secret' => env('PUSHER_APP_SECRET', 'app-secret'),
        'app_id' => env('PUSHER_APP_ID', 'app-id'),
        'options' => [
            'host' => env('PUSHER_HOST', '127.0.0.1'),
            'port' => env('PUSHER_PORT', 6001),
            'cluster' => env('PUSHER_APP_CLUSTER', 'mt1'),
            'scheme' => env('PUSHER_SCHEME', 'http'),
            'encrypted' => true,
            'useTLS' => env('PUSHER_SCHEME') === 'https',
        ],
    ],
```

Add the following to the .env file:

```conf
PUSHER_APP_KEY=app-key
PUSHER_APP_ID=app-id
PUSHER_APP_SECRET=app-secret
PUSHER_HOST=127.0.0.1
PUSHER_PORT=6001

PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY=app-key
VITE_PUSHER_APP_ID=app-id
VITE_PUSHER_APP_SECRET=app-secret
VITE_PUSHER_HOST=127.0.0.1
VITE_PUSHER_PORT=6001
VITE_PUSHER_PROXY_PORT=6001
VITE_PUSHER_APP_CLUSTER=mt1
```

(Or, for Laravel Mix apps, replace **VITE_** with **MIX_**

## 2. Setup Soketi
To run / test the websockets locally, you need to install Soketi (or an equivalent websockets server app).

### On Windows

In a terminal window:
- `npm install -g @soketi/soketi`
- confirm soketi can run by running `soketi start`.


If you encounter a permissions error when running soketi, try the following:


#### Update Execution Policy for soketi in Windows

- Launch Windows PowerShell as administrator
- run command "Get-ExecutionPolicy -List"
   - LocalMachine should be "Restricted"
   - For LocalMachine, change ExecutionPolicy from "Restricted" to "RemoteSigned"
- run command "Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine"

Reference: [about_Execution_Policies](https://docs.microsoft.com/en-gb/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2#managing-the-execution-policy-with-powershell)


## 3. Run Soketi and test your appliation

- In a terminal window, run `soketi start`. Leave this running in the background while testing.
- Open the app in the browser and go to a page where Laravel Echo should be running.
- Open the dev tools. In the Network tab, filter to only show "WS" entries. There should be 2 entries with a status of 101.

![WS Entries](https://file%2B.vscode-resource.vscode-cdn.net/Users/dave/Library/Application%20Support/CleanShot/media/media_ZH5iLVHeKt/CleanShot%202023-04-21%20at%2015.17.20.png?version%3D1682086758353)

If the WS entries fail, it means that the application cannot connect to your Soketi instance. This may indicate the .env settings are not correct.

If you do not see these entries, it means that Laravel Echo is not running on the page, and may not be configured correctly in the application.


# For use with Laravel Filament

Laravel Filament has a slightly different way to setup Laravel Echo. If you're using the admin panel, instead of creating a separate JS file for your Echo config, you should put it into `/config/filament.php`. There is a `"broadcasting" => [ "echo" ]` entry that is commented out by default. The `echo` array should be the exact config you would need in the JS file, as behind the scenes Filament runs `@js(config('filament.broadcasting.echo')` to generate the Echo instance in JavaScript. 

So, e.g: inside `config/filament.php`:

```php

    'broadcasting' => [

        'echo' => [
            'broadcaster' => 'pusher',
            'key' => env('VITE_PUSHER_APP_KEY'),
            'wsHost' => env('VITE_PUSHER_HOST'),
            'wsPort' => env('VITE_PUSHER_PROXY_PORT'),
            'wssPort' => env('VITE_PUSHER_PROXY_PORT'),
            'disableStats' => true,
            'encrypted' => true,
            'cluster' => env('VITE_PUSHER_APP_CLUSTER'),
            'forceTLS' => (env('VITE_PUSHER_SCHEME') === 'https'),
            'enabledTransports' => ['ws', 'wss'],
        ],

    ],

```

