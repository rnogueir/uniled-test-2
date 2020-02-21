# UniLED Technical Test 2

This is the overview of a solution for a campaign feeds management under Laravel.



## Architecture

A Laravel installation will provide both the front-end aplication and the REST API.



## Front-End

The front-end will contain:
- Login page: uses plain login via e-mail/password, eventually allowing social login via Laravel Socialite
- Registration page: offers a registration form in order to capture user data
- registration confirmation page: to be shown when the new user clicks on a link sent via e-mail, confirming that the address is valid
- main dashboard: a welcome page, after login, will show some management data (might include some charts) and a navigation menu
- API token management menu: allows the user to activate his/her API and initialize/refresh a token
- Feeds menu: list of all feeds, ordered by creation/update date (with pagination)
- Add Feed menu: opens up a form for Feed data entry, allows user to add existing News Items
- Edit Feed option: allows the user to add/edit/remove news items (whenever possible) and publish the feed (changes allowed while not published)
- Add News Item menu: opens up a form for News Item data entry
- Edit News Item option: allows the user to change a News Item if not yet published
- Duplicate News Item option: generates a copy of an existing News Item

The API will contain:
- authenticated access via authorization code and access token (OAuth 2.0)
- a single resource called /feed that will retrieve Feed data (including all related News Items)



## Models

- User: contains 0 or more Feeds (table USERS)
  Main properties:
  + ID
  + Name
  + E-mail
  + Password
  + API token

- Feed: contains 0 or more News Items (table FEEDS)
  Main properties:
  + ID
  + Creation date
  + User ID
  + Title/Description
  + Publication status
  + Publication date

- News Item: belongs to 0 or more Feeds (table NEWS_ITEMS)
  Main properties:
  + ID
  + Creation date
  + Headline
  + Type
  + Logo image
  + Content image
  + Description text



## Tables

Besides a table for each mentioned model, a join table FEEDS_ITEMS_RELATION will be created in order to link the tables FEEDS and NEWS_ITEMS. The join table will use foreign keys to connect a Feed ID to a News Items ID. The models will be linked in Laravel by using the `belongsToMany()` relationship.



## Testing

The following tests must be made:
- Login process and authentication
- Registration process (including the registration e-mail and confirmation link)
- Feed CRUD operations
- News Item CRUD operations
- All validations regarding the relation between Feeds and News Items
- API authentication (OAuth 2.0 process)
- API resources (GET method only)



## CI/CD

All code and configuration will be maintained in a Git repository. The database structure will be deployed via Laravel migrations.

By using the `git flow` branching model, each Git branch will be directed to a specific environment: unit (local) testing, release testing, staging and production. All tests must be passed before the changes are merged forward from local testing to production.

All environments will have servers installed in AWS. The DNS configuration (Route 53) will add subdomains for staging and for the API, which will run separated from the main web application. Smaller EC2 and RDS instances may be used for testing, while the production instances have to be dimensioned according to the expected load (including preparation to scale by using ELB and more EC2 instances).
