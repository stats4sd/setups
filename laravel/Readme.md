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

## TODO:
 - Organise the functionality notes above.
 - Create proper templates or demos for the more commonly used features:
   - Local Search
