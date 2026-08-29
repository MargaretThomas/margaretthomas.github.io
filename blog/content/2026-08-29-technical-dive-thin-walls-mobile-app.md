---

title: "Inside the Build: The Thin Walls Mobile App"
summary: "A technical look at how Thin Walls is being built, and how decisions around sound reviews, authentication, moderation and testing are shaping the mobile app."
date: 2026-08-29
tags:
- Thin Walls
- Behind the Build
- AI-Assisted Development
slug: technical-dive-thin-walls-mobile-app

---

# A Technical Dive Into How I’m Building the Thin Walls Mobile App

This is a story on how a product about something subjective and socially sensitive — the sounds people experience in shared buildings — became a structured mobile application.

The engineering decisions are closely connected to the product decisions: what counts as a review, what remains anonymous, when authentication is required, how new locations enter the system, and where moderation belongs.

## What Thin Walls is

Thin Walls is a platform for helping people build more realistic expectations about sound in apartments, townhouses, hotels and student accommodation.

The mobile app currently allows people to:

* browse locations
* switch between list and map views of locations
* read sound-focused location reviews
* create reviews using a curated sound catalogue
* propose missing locations
* track submitted locations
* manage their own reviews

## Technical stack

| Area            | Technology                       |
| --------------- | -------------------------------- |
| Mobile          | React Native + Expo + TypeScript |
| Navigation      | Expo Router                      |
| Authentication  | Firebase Authentication          |
| API             | .NET                             |
| Database        | PostgreSQL                       |
| Hosting         | Azure                            |
| Maps            | react-native-maps                |
| Location search | OpenStreetMap Nominatim          |
| Builds          | Expo Application Services        |

High-level flow:

`React Native app → Firebase identity → Thin Walls .NET API → PostgreSQL`

The mobile app also communicates with mapping and location-search services.

## Building in vertical slices

Thin Walls did not begin with its current architecture. The build evolved roughly like this:
`UI → dummy data → APIs → authentication → persistence → moderation → installable builds`

Starting with local data let me work through navigation and product flows before the backend existed.

Then, piece by piece, those local boundaries were replaced with real API endpoints and authenticated workflows.

I've tried to keep the same principle throughout the project: build small pieces, test them end to end, and only add more complexity when the app needs it.

## Modelling something subjective: Sound

One of the most important design choices was deciding what a Thin Walls review actually contains. A Thin Walls review isn't a star rating, decibel measurement or audio recording. Instead, users select the sounds they experienced from a curated `SoundCatalogue`.

At its simplest:

A `Location` is linked to many `Review` objects which are linked to selected `SoundCatalogue` items. 
This gives Thin Walls structured, comparable data.

The sound catalogue contains an internal relative-loudness value for ordering sounds, but that isn't presented as a measurement. Reviewer identity follows a similar boundary. A review belongs to a Firebase user internally, while remaining anonymous in the public interface.

The app’s promise that reviews remain anonymous to the public is built into how reviewer identity is stored and displayed.

## Authentication only when ownership matters

People can browse locations and read reviews without signing in.

Authentication becomes necessary when ownership matters: creating, editing or deleting a review, proposing a new location, or tracking a submission.

Firebase handles authentication and confirms who the user is.

The Thin Walls API then uses that identity to manage the app’s data and control which actions the user is allowed to perform.

## Growing the location dataset carefully

Eventually, someone will search for a building that Thin Walls doesn't know about yet.

Instead of allowing new locations to appear publicly immediately, the app creates a separate contribution flow:
`Search for the new location → select location → add sound review → submit → moderation`

The proposed location and its first review enter a pending state. This lets users help grow the Thin Walls location database while keeping new submissions hidden until they’ve been reviewed.

## What native builds taught me about testing

The map feature gave me one of the clearest mobile-development lessons so far.

Automated tests can confirm that coordinates are valid and components receive the right data. They can't prove that a native map provider or API key is correctly configured inside the installed application.

Testing installable builds also surfaced issues that development tooling hadn't, including incorrect initial routing and configuration errors reaching the UI.

My verification process now looks more like:
`source code checks → automated tests → app build → physical-device verification`

Each stage tells me something different. A passing test suite gives me confidence in the code. A working standalone app on my phone gives me confidence that the app actually behaves the way I expect it to. For mobile development, I’ve learned that I need both.

## Building with AI without handing over the decisions

AI use is part of the Thin Walls development workflow, with each part of the process having a clear role.

1. I define the intent, make the product decisions, and shape the architecture.
2. ChatGPT helps me think through the implementation, surface blind spots, review decisions, and turn the work into a clear prompt for Codex.
3. Codex makes the code changes based on that implementation prompt.
4. I review the changes, run the app, test the behaviour, make adjustments where needed, and then commit the work.

The workflow looks roughly like this:
`intent and decisions → ChatGPT review and prompt design → Codex implementation → human review → runtime testing → adjustments → commit`

I also keep the development prompts versioned alongside the project documentation. The code records what was built, while the prompts preserve some of the reasoning and implementation intent behind those changes.

## Still building

There are already parts of Thin Walls that I know will need more attention as the app grows, including better behaviour on slow networks, review drafts persistence and map clustering.

One of the things this project keeps teaching me is that not every future problem needs to be solved today. Sometimes the better decision is to build the simplest version that works, test it properly, and wait until the product gives me a real reason to add more complexity.

Thin Walls is still actively being built, so I expect that list to keep changing as more of the app moves from code into real-world use.

---

That's it for this blog. See you next time.

Happy building,  
Margaret