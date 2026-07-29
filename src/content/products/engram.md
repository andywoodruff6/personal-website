---
title: Engram
type: digital
status: Shipped
price: Free · Bring your own API key
date: 2026-07-26
description: A calm macOS learning app. Type a topic and Engram researches it from real web sources, teaches it as a short course, and turns each lesson's self-check into spaced-repetition flashcards.
cover: /images/engram-library.png
link: https://www.glaze.app/app/engram-Js72PP
tags: ['AI', 'building', 'tech']
featured: true
order: 1
---

> Making the cards is the reason most people quit flashcards. Engram makes them for you.

**[Get Engram on the Glaze Store →](https://www.glaze.app/app/engram-Js72PP)**

Type a topic — "the Krebs cycle," "Norse mythology," "how tariffs work" — and Engram researches it across the web and writes you a short course, citing its sources. Drop in a PDF or paste a link and it teaches what that document actually says.

![Engram generating a course](/images/engram-gen-writing.png)

## Lessons before cards

Every other flashcard app hands you an empty deck and calls it a feature. Engram teaches before it tests: each course is a handful of short lessons, and finishing one turns its self-check questions into your cards. No card ever arrives that isn't tied to something you've read.

![A researched course, open](/images/engram-course-norse.png)

Rate a card Again, Hard, Good, or Easy and an SM-2-style scheduler brings it back right before you'd forget it. Points, levels, and a daily streak are there to get you to open the app tomorrow.

![The review loop](/images/engram-review-loop.png)

## What it's made of

An Electron + React 19 app built with [Glaze](https://glaze.app), Raycast's AI app builder, for the Glaze Awards. Courses, cards, and review history are plain files on your machine — no account, no sync, no lock-in. Your Anthropic API key is encrypted through the system keychain and never leaves the device or crosses into the renderer.

The interesting constraint was building the whole thing by prompt. Every feature is a numbered spec handed to an agent, and every claim about it was verified by reading the app's own data files rather than trusting the build report — cards persisting with correct due dates across a relaunch is the only test that counts. The palette is [Flexoki](https://stephango.com/flexoki).

![Session complete](/images/engram-session-complete.png)

Requires macOS on Apple Silicon and an [Anthropic API key](https://console.anthropic.com/settings/keys).
