# Trello-MirrorSync

This is a 2 way one-to-one mirror and sync solution for Trello that uses Trello native webhooks. It is implemented with FASTAPI to provide the api services for both the setup and actual mirror-sync operations and can be deployed on a platform like Deta or Wayscript X that supports Python. Since it uses the admin user's API Key and Token, the operations can be across any workspace or boards that are accessible by the admin user's credentials.

## Endpoints

### /mirror
This endpoint handles all the actual mirror and synchronization operations for the application.
### /clone
This endpoint allows you to clone a card and inititalise the a Deta Base to provide the 1-1 relationship between the 2 cards.
### /setup
This endpoint creates the webhook using the card ids as the idModel for webhooks.
### /stop
This endpoint removes the Deta Base entries and the webhooks.

## Scope
The scope for the mirror and synchronization operations include :

- Name and description
- Due Date and Time, Start Date, due Reminder etc 
- Labels (including custom labels)
- Checklists
- Customfields
- Attachments (also 3rd party attachments like Box, Trello card / board, images, pdf...etc)
