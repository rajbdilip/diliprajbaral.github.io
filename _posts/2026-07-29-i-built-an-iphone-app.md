---
title: "I Built an iPhone App"
date: 2026-07-29
layout: single
classes: wide
excerpt: "How scattered documents, an abandoned web app idea, and years of postponed mobile development led me to build and ship my first iPhone app."
author_profile: false
read_time: true
categories:
  - projects
  - ios
  - product
tags:
  - truefold
  - ios
  - swiftui
  - local-first
  - icloud
---
While filling out a travel visa application, I needed my passport details, a passport-size photo, my flight details, and my hotel booking. I did not have my passport with me. I knew I had copies of everything somewhere, but finding them meant searching Photos, Downloads, old emails, and messages.

This happened more often than I would like to admit. I would need a card number for an online purchase when the physical card was not with me. I would search old chats for bank details someone in my family had sent, or send my own account details to them again. While travelling, tickets, hotel vouchers, and visas would be spread across email, chats, Photos, and Files.

The documents were not lost. They were everywhere.

## A problem that followed me across countries

I was born and brought up in Nepal and finished high school there. I moved to India for college and spent the first eight years of my career there, then moved to Singapore in 2025. Over time, I accumulated IDs, accounts, cards, travel records, education documents, and government paperwork from more than one country.

I used to organize these documents religiously in Google Drive. But as life got busier, keeping that system tidy became harder. A document downloaded for an application would stay in Downloads. Another would remain attached to an email, sit in a family chat, or get saved to Photos. It happened gradually, without me really noticing, until finding the latest copy meant searching several places and hoping I remembered where it had landed.

Storage was not really the problem anymore. Retrieval was.

## The web app that did not happen

My first instinct was to build a small web app and host it on my own domain, perhaps at `pass.diliprajbaral.com`. I imagined a private interface where I could organize the documents and details I repeatedly needed.

But the more I thought about when I needed those details, the less a web app made sense. Airports, immigration counters, government offices, and travel are not places where I want important information to depend on a reliable connection. Even a card number can become awkward to retrieve if the source is online and the connection is not cooperating.

The app needed to work without the internet. So I started thinking about a mobile app that stored the information on the phone, where I could search it even when I was offline.

There was one fairly large problem: I had never built an iPhone app.

## Getting into iPhone development

When I was younger, I taught myself a collection of things simply because I wanted to make something with them. I started with Visual Basic, moved into Photoshop and static websites, and at one point built a Facebook-like social network for my high school.

Mobile development was the thing I never managed to get to. Years passed, work became busier, and learning Swift, SwiftUI, Xcode, signing, Apple's development ecosystem, and App Store submission all at once felt like a lot for an experiment.

AI gave me the push to try. It helped with boilerplate and with finding my way around an ecosystem that was completely new to me. That left more of my limited time for deciding what the product should be, understanding Apple's frameworks, and testing the app.

I still had plenty to learn. AI just made it easier to start.

## Local, but not local-only

Once I committed to storing everything locally, another question came up: what if I lost or replaced my phone? What if I needed the information from another personal iPhone? A local-only vault would work offline, but one lost device could also mean one lost archive.

Around that time, I had recently learned about [Flighty](https://flighty.com/), the flight-tracking app. Flighty [uses iCloud to keep flights and settings in sync across a user's devices without requiring a separate account](https://flighty.com/help/account-sync). I liked that approach, and it answered the same question for my app.

The app could save locally first, remain useful offline, and sync through the user's private iCloud storage. There would be no separate account and no additional sign-in to remember. On another personal iPhone using the same Apple Account, the vault could be restored and kept in sync.

These records are sensitive, so "stored in iCloud" was not enough on its own. Sensitive values and document scans would be encrypted by the app before being saved or synced, with the keys stored separately in iCloud Keychain. Face ID, Touch ID, or the device passcode would protect access to the app itself.

That gave me the balance I wanted: it would work offline, stay available across my own devices, and need neither an account operated by me nor a server of my own holding the vault.

## Making it a real product

At first I was solving my own annoyances: find a record quickly, copy the exact detail I needed, and keep its image or PDF nearby. But as I worked through family records, documents from different countries, offline access, and recovery, the project stopped looking like a private utility tied to my domain. These were problems other people had too.

I named the app [Truefold](https://truefold.app). If other people were going to trust it with important records, it needed a polished interface, careful failure handling, accessibility, physical-device testing, privacy and support material, and all the work required for an App Store release.

## It is live

Truefold is now available for iPhone. It stores IDs, bank details, payment cards, travel documents, and other important records in a structured vault designed for quick retrieval. It works offline, syncs through private iCloud, and does not require a Truefold account.

After years of telling myself that I would eventually learn mobile development, seeing an app I had designed and built appear on the App Store felt particularly satisfying.

Truefold began with a very small frustration: I knew I had the information, but I could not reliably find it when I needed it. Solving that problem took the idea from a possible subdomain, to local storage, to private iCloud sync, and eventually to a product that other people could use.

The documents are still in different shapes and from different parts of life. They finally have one place where I know how to find them.

[Download Truefold on the App Store](https://apps.apple.com/app/id6790803563).
