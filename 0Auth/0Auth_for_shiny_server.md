# Authentication for Shiny Staging Server

We have 2 Shiny installations running on our Shiny server:

- https://shiny.stats4sd.org: all Shiny apps running on this instance are publically accessible.
- https://shiny.stats4sdtest.online: all Shiny apps running on this instance are private, and this is intended mainly for testing apps that are not yet ready for public use.


To access the applications on the testing instance, users must authenticate. We use a small NodeJS application linked to 0Auth to provide authentication.

 - The application on 0Auth is **dev-l8wfoq5p**.
 - Login to the 0Auth management system is with support@stats4sd.org (password in 1Password - IT Vault ).
 - The main method of authentication is "social" - via Github.
 - There is a login **Action** that checks to ensure users have an "@stats4sd.org" email address. Other email addresses are denied access.


