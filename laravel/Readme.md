# Laravel Projects

Most projects should start with the [Stats4SD Laravel Template](https://github.com/stats4sd/laravel-template), which already has Laravel Backpack and a few other packages setup.

## Specific Functionality
Many pieces of functionality are not required often enough to just install into the main template, but are sometimes required. Notes are given here below:

### Revise Operation
When you need to track edits that occur in Laravel Backpack CRUD panels.

[Revise Operation](https://backpackforlaravel.com/docs/4.1/crud-operation-revisions#how-to-use-1)

```bash
composer require backpack/revise-operation
cp vendor/venturecraft/revisionable/src/migrations/2013_04_09_062329_create_revisions_table.php database/migrations/
php artisan migrate
```

### Full Text / Indexed Search

For a quick text-search with fuzzy-search, There are 2 options:

#### TNT Search

For in-app searching, no external dependancies. Creates an index locally and uses that when performing searches.

- + Easy to setup, quick to integrate into any front-end
- - Doesn't have the detailed customisable faceted search with front-end widgets that Algolia does.

#### Algolia Search

Sends data out to Algolia - the indexes are stored there.

- + More customisable searches, including faceted (filtering) using front-end widgets, easily present count of results per facet, generally more powerful options available.
- - More complex to setup. Relies on external service. Integrating into Front-ends requires use of their own widgets, which are not the simplest things to learn / customise...

### Laravel Websockets

For listening to server-side events on the front-end, we use beyondcode/laravel-websockets. This is a PHP-based replacement for the Pusher service, that can be run directly on the server you deploy Laravel onto. (So we don't need to rely on / pay for / trust a 3rd party service to manage all our events and notifications!).

#### Setup:

- `composer require beyondcode/laravel-websockets`
- `npm i vue-laravel-echo` (assuming use of echo within vue components)
- Inside config/app.php - enable the BroadcastServiceProvider.

TODO - prepare template code from case studies and snowball apps

- Bring over the vue laravel echo setup stuff from app.js
- Bring over config/websockets.php
- Bring over config/broadcasting.php
- Add correct .env variables
- Run websockets locally: `php artisan websockets:serve`
    - Use VS code tasks to run this alongside `php artisan queue:work` for testing Kobo link etc.

#### Deploying:

- setup nginx block (copy from case studies)
- setup server demon to run websockets:serve comment
    - **double check that ports are matching with the .env variables!**
    - double check that the firewall is allowing traffic on the proxy port

## TODO:
 - Organise the functionality notes above.
 - Create proper templates or demos for the more commonly used features:
   - Laravel Websockets
   - Local Search
