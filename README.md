# Trello-MirrorSync

This is a 2 way one-to-one mirror and sync solution for Trello that uses Trello native webhooks. It is implemented with FASTAPI to provide the api services for both the setup and actual mirror-sync operations and can be deployed on a platform like Deta or Wayscript X that supports Python. Since it uses the admin user's API Key and Token, the operations can be across any workspace or boards that are accessible by the admin user's credentials.

## Endpoints
**Update** Apr 3 With increase in functionalities added to the webhook the clone, setup and stop endpoints are now in a separate micro.

### /mirror
This endpoint handles all the actual mirror and synchronization operations for the application.
### /clone
This endpoint allows you to clone a card and inititalise the a Deta Base to provide the 1-1 relationship between the 2 cards.
### /setup
This endpoint creates the webhook using the card ids as the idModel for webhooks.
### /stop
This endpoint removes the Deta Base entries and the webhooks.

By keeping /setup and /clone separate endpoints, it allows users to embed and call the endpoints from within Trello automation for both a new setup as well as any two existing cards.

### Documentation
The redoc documentation can be found at https://6yokvk.deta.dev/redoc (setup) and https://moeb8z.deta.dev/redoc (webhook)

## Scope
The scope for the mirror and synchronization operations include :

- Name and description
- Due Date and Time, Start Date, due Reminder etc
- Labels (including custom labels)
- Checklists (**Update** now supports advanced checklist due date and member assignment)
- Customfields
- Locations (**Update** Apr 3 : support available)
- Attachments (also 3rd party attachments like Box, Trello card / board, images, pdf...etc)
- Comments (**Note** : once comment is deleted it is no longer available to synchronise with the other card, to overcome this comments marked with a prefix eg **del will be deleted via a cron instead.)

## Demo
See https://youtu.be/Th1WMViIznc

### Automation
Here are two examples using card buttons with the endpoints to setup the mirroring :

- Uses the /clone to create a copy of the card and /setup to create the create_webhooks
`post to url "https://25puys.deta.dev/clone" with payload "{"card_id" : "{triggercardidlong}"}", and post to url "https://25puys.deta.dev/setup" with payload "{"card_id" : "{httpresponse.card_id}" , "alt_card_id" : "{httpresponse.alt_card_id}"}"`
**Update** Trello not returning {httpresponse} correctly : Apr 2, 2022.

- Uses Trello automation to create a copy of the card and /setup to create the create_webhooks
`copy the card without comments to the top of list "Up Next" on board "Dojo 1", and post to url "https://25puys.deta.dev/setup" with payload "{"card_id" : "{triggercardidlong}", "alt_card_id" : "{copycardidlong}"}"`
