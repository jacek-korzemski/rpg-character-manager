# RPG Character Manager - Frontent (Work in Progress)

Small 🚀Astro Project - system for managing characters for RPG games Like Dungeons and Dragons, Warhammer, etc. 
This is just the frontend part. Backend is realised in CodeIgniter 4, and will be soon also availible on my GitHub.

## Roadmap

### Level one

- ✔ Initial Layout in Astro / Svelte
- ✔ Register / Login forms
- ✔ Register / Login API communication
- ✔ Add a card form (at least one system)
- ✔ Add a card API communiaction
- ✔ List cards
- ⚪ View single card
- 🛠 Remove Card
- ✔ Edit card (frontend component)
- ⚪ Edit card (api Communication to update)
- ⚪ PDF export (at least one system)

### level one and a half

- ⚪ Add basic suppor for at least 3 systems
- ⚪ Audit some security, learn about it, try your best 💪

### Level two

- ⚪ Create game room (in CI4)
- ⚪ Join game room (new API with Websockets on Node - totally separate project)
- ⚪ Send rolls from player card to Websocket Game Room API
- ⚪ Include everything that I've forgot during making this list

## Backend - static Layer

Backend is made in CodeIgniter 4, include JWT support for handling user auth, and I will post in on github soon.
While I was working on this project, I've came across some difficulties while trying to run both - front and back - locally, 
so I'll try to include some kind of tips there.

## Backend - dynamic Layer

I will try to add separate backend for Websocket handling, so the players could join game rooms, and rolls dice there. 

## Summary

In Summary - it's a little like Roll20 but without the map and tokens, and a little like Owlbear but with Character Cards support (and still - without map and tokes 😝)