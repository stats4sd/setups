# Platform Name
Add quick description here ....

Link (if live and public)

## Purpose
Add description here ....

# Development
This platform is built using Laravel/PHP. The front-end is written in VueJS and the admin panel uses Backpack for Laravel. __(\*\*UPDATE IF NOT APPLICABLE\*\*)__

## Setup Local Environment
1.	Clone repo: `git clone git@github.com:stats4sd/.......... ` __(\*\*UPDATE\*\*)__
2.	Copy `.env.example` as a new file and call it `.env`
3.	Update variables in `.env` file to match your local environment:
    1. Check APP_URL is correct
    2.	Update DB_DATABASE (name of the local MySQL database to use), DB_USERNAME (local MySQL username) and DB_PASSWORD (local MySQL password)
    3.	If you need to test the Kobo link, make sure QUEUE_CONNECTION is set to `database` or `redis` (and that you have redis setup locally). Also add your test   KOBO_USERNAME and KOBO_PASSWORD / ODK_USERNAME and ODK_password __(\*\*REMOVE IF NOT APPLICABLE\*\*)__
    4.	If you need to test real email sending, update the MAIL_MAILER to mailgun, and copy over the Stats4SD Mailgun keys from 1 Password __(\*\*REMOVE IF NOT APPLICABLE\*\*)__
4.	Create a local MySQL database with the same name used in the `.env` file
5.	Copy `auth.json.example.json` as a new file calling it `auth.json` and add the login details for the Stats4SD Spatie account - this is required for the Media Library Pro package __(\*\*REMOVE IF NOT APPLICABLE\*\*)__
6.	Run the following setup commands in the root project folder:
```
composer install
php artisan key:generate
php artisan backpack:install #REMOVE IF NOT APPLICABLE
php artisan telescope:publish #REMOVE IF NOT APPLICABLE
php artisan updatesql #REMOVE IF NOT APPLICABLE
npm install
npm run dev
cd scripts/Rscript && Rscript -e "renv::restore()" #REMOVE IF NOT APPLICABLE
```
7.	Migrate the database: `php aritsan migrate:fresh --seed` (or copy from the staging site)

__Examples of optional development sub-sections to include in ReadMe:__
## Photo Storage
This platform includes photo management. It is setup to connect to Google Cloud Storage as the main 'media' storage driver. It uses the [superbalist/laravel-google-cloud-storage](https://github.com/Superbalist/laravel-google-cloud-storage) driver for this.

## Setup Link to Google Cloud
Add description here ....

## Translation Management
The platform is linked to translation.io to manage the translations. There are also custom commands that enable translation of Vue component strings *and* DB entries via the service. To sync to translation.io:

1. Run the meta command `php artisan translation:all`. This will do the following:
   1. Run `php artisan translation:db` to search the database for translatable strings agethow get the back into the databasend add them to the set of strings that get sent to translation.io
   2. Run `php artisan translation:vue` to search the Vue components from strings and add them.
   3. Run `php artisan translation:sync` to sync the platform with the strings on the translation.io project
   4. Run `php artisan translation:extract-json` to extract the translated strings to a JSON file (Vue is setup to read translations from JSON)

## User Permissions
Add description here ....

## Run Laravel Websockets & Queues
To run the local notifications, start up Laravel Websockets locally: `php artisan websockets:serve`. This runs the websockets server on localhost port 6001.

To test the job queue locally, run Horizon: `php artisan horizon`.

## Note on ....
Add description here ....
