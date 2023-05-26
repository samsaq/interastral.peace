# interastral.peace
A project site heavily inspired by bray.tech, just for Honkai: Star Rail instead

# Current Design Doc / Plans (Heavily WIP)

## Planned Features for MVP

* A landing page for the site with readouts of the most important information
  * A login button + modal at the edge of the screen
    * email + password, github / google / etc - we'll use firebase for this one, use of API passing the authenticated user will isolate & enforce security 
  * Redeemable Codes
  * Upcoming Dates (Banners, Events, Server Reset, etc)
    * Add a ? tooltip to clarify what the server reset actually resets
  * A dual split selection (aka a big and fancy shortcut) to the Relic Optimizer & the Team Builder
  * Misc shortcuts to other data
    * Archived data (Character data, relic sets, light cones, materials, books, lore, theorcrafting basics (eg: damage calcs and equivalents to things like guage theory when discovered) - maybe link to external resources if preferable)
    * Character Guides & Builds - link to keqingmains & link in to videos from actually reputable sources
  * A blog / devlog system for release notes, commentary, etc - inspired by bray.tech

* A user profile / settings page, where the user can modify settings (eg: unreleased content) & delete their user / wipe their data, as needed
* A set of pages for character data, with filters by element and class, where the pages themselves include:
  * List the name and icon + a brief intro, etc
  * Lists stats and materials + a calculator from one level & ascension to another
  * Talent material clickable graph (generic graph overlaid with talents, click and then hit set for a base, then click more to see difference of new upgrades
  * Voicelines, lore, etc - section per character (use firebase's file bucket)
* A set of pages on relics - include their lore & stats + icons, etc + leveling cost calculator
* A set of pages on materials - include their descriptions & sources + avg fuel / time cost per, if relevant, etc
* A set of pages on light cones - include stats at level breakpoints via selector & at various selectable superpositions + a calculator as for characters

* A way to update and maintain all this information (eg: add new relic set, add new code, add new etc without actually issuing pushes to github (or maybe with a better way of working through that)
  * Should I add something like a CMS / am I gonna end up making a small one myself?

* Character data might be split into one character calculator page (where you select the character and get their calcs) and a seperate set of character lore pages, for user clarity

## Planned Features for the future

Two parts of the arhcival data are not critical:
* A set of pages on the books & lore - include text of books with filters based on location of collection & a link to ashikai's lore videos
* Theorycrafting basics - Include an explainer on the damage calculations, on speed calculations (link to grim's spreadsheet), and on team composition & then link again to Grim, Keqingmans, and other sources for full depth explainers
* A PWA & Mobile focused redesign as needed for such use
* A companion mobile scanner app & computer program to scan the relics of a player in so they can optimize them
  * Maybe we can take inspiration from artiscan.ninjabay.org but I imagine this will be a truly massive amount of work

## Frontend Design
We're themeing the site off of the inter-astral peace corporation (the IPC) - as if this was an educational / info site hosted by the megacorporation themselves. As we lack detailed concept art or dev detailing of the IPC's style that I can find, we're gonna lean heavily onto the aesthetics of the herta space station, meaning white, black, and gold, with gold being switched out for other bright tietary colors like red, blue, and green as needed to distinguish between parts of the site (eg: maybe we could make the lore section blue, etc). Also, we'll be using the hexagon & trinary symbols within the site (maybe replicate that red trinary from the master control zone for the favicon) and generally going for a futuristic / techy style. Not too sure what we'll run for a dark mode, if we have one at launch.

## The Stack
Currently planning on a next / react front end-site, as I'm aiming to gain practice in that. The backend needs for the site will be handled without a direct serverful backend - I'm planning on the use of prisma as an ORM within the server components / api routes of the next project and connected to a planetscale DB for my data needs. Incidentally, the reason that planetscale was chosen, and partly why next was chosen, was because of planetscale & vercel's very nice free tiers, which should allow me the ability to handle a much larger amount of users than this project will likely ever reach (and give me the option to scale should it ever do so). I do imagine I'll run into read / write limitations before I do space, but both are far off concerns. I could also implement database caching locally for a user (eg: on the relics page) to save on reads / writes, but that's a feature for the far future.

A relational database was chosen both for familiarity on my part, but also due to the desire for later features like relic analysis that might come into play in future (as I am not entirely certain on all the ways I will query the data in future, the data-focused instead of query-focused building of a relational db seems more ideal - it offers me flexibility later). I also believe that the use case for the majority of my data will fit closer to the more even split of read/write favored by relational databases as compared to document dbs (like firestore) that I was previously considering which prefer read heavy workloads. This is due to the the expectation that the use of the database will mostly be for queries to a user's relic inventory for optimization - and I expect such queries to be done in bursts with an upload of an invetory and then some 50-100 or what have you to it for various teams. The main thing is that I expect a new upload of an inventory every time a user gets around to optimization, as their relics will have changed quite a lot. As such, there'll be a lot of writes alongside the reads.
