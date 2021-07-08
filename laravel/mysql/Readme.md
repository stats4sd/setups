## MySQL Databases
MySQL is used as the primary database for most data platforms maintained by Stats4SD.

Conventions are taken based on the (oppinionated) defaults that Laravel typically expects. Notes on SQL setup + conventions used:

 - use snake_case for table names and field names.
 - For tables named after a noun (users, submissions, teams etc), always pluralise the name.
 - link tables for many-many relationship should use the format described in the [Laravel documentation](https://laravel.com/docs/8.x/eloquent-relationships#many-to-many-model-structure):
    > To determine the table name of the relationship's intermediate table, Eloquent will join the two related model names in alphabetical order.
    - e.g. `submission_team`
 - an older convention is to name the link table with `_link`, to make it clearer to see which are link tables.
   - e.g. `_link_submission_team`
   - This older convention is present in a lot of projects, and is fine to keep using for those projects.

TODO:
 - Review notes and provide links to examples in different data platform projects.
 - Include information about where databases are hosted (convention to have a db installed on the same server as the platform for small things, with suggestions for larger projects on remote databases)
 - Include information about using MySQL / other SQL databases outside of a Laravel project.