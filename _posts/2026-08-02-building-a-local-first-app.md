---
title: "Building a Local-First App"
date: 2026-08-02
layout: single
classes: wide
excerpt: "Working without an internet connection is only one part of local-first software. The harder questions begin when local copies have to sync, recover, and resolve disagreements."
author_profile: false
read_time: true
categories:
  - architecture
  - ios
  - product
tags:
  - local-first
  - offline
  - cloudkit
  - icloud
  - truefold
---
In [I Built an iPhone App](/blog/i-built-an-iphone-app/), I wrote briefly about why Truefold stores its data locally and syncs through iCloud. That decision started with a simple requirement: I wanted my important documents to remain available without an internet connection.

At first, I thought the storage decision was mostly done. Save everything on the iPhone.

Then I asked what would happen if I lost the iPhone.

Keeping data on the device solved the immediate problem. I could open the app at an airport, immigration counter, or government office without depending on Wi-Fi or mobile reception. But if the data existed only on that device, losing or replacing the phone could mean losing the vault too.

The app needed to work locally without becoming local-only.

## Offline-capable and local-first are not the same

An online-first app usually treats a server as the main copy of its data. The app downloads what it needs and may cache some of it for offline use. If the connection disappears, previously loaded screens might continue to work, but creating or changing something often has to wait.

A local-only app sits at the other end. The device holds the main, and perhaps only, copy. It works without a connection, but moving to another device or recovering after a loss becomes difficult.

In a local-first app, normal work happens against local data. The user can read, add, and edit without waiting for a server. Synchronization happens separately and brings the local copies on different devices into agreement later.

That difference sounds small until something goes wrong.

## Saving locally should not wait for the cloud

When someone saves a record in Truefold, the record is written to the local vault first. A slow connection, unavailable iCloud account, or temporary sync problem should not prevent the save from succeeding on the iPhone in their hand.

The sequence is roughly:

1. Save the record locally.
2. Confirm that it was saved.
3. Queue the change for iCloud synchronization.
4. Report the sync state separately.

"Saved" and "synced" are therefore two different statements.

This distinction is useful even when everything is working. Someone can add a record before a flight, lose connectivity, and continue using it throughout the journey. When the connection returns, the pending change can sync to their other devices.

## Sync is where the difficult questions begin

Adding cloud sync is not just a matter of uploading the local file.

What if two iPhones edit the same record while offline? What if one deletes a record while another edits it? What happens if the user signs out of iCloud, switches to a different Apple Account, or runs out of iCloud storage?

I had to answer each of these rather than leave them to chance. The newer edit wins when two devices change the same record. A deletion wins over an edit made at the same time. Signing out pauses sync but leaves the local vault working. Signing into a different Apple Account stops sync before anything can get mixed; if the user chooses to start fresh, the previous vault is set aside rather than deleted. Running out of iCloud storage does not stop local edits; it leaves them waiting to sync.

There are also versioning problems. A newer version of the app might create a record that an older version does not yet understand. Truefold keeps that record safe and asks the user to update rather than silently discarding something simply because the local model is behind.

Encryption adds another case. An encrypted record might arrive on a second device before its encryption key has finished moving through iCloud Keychain. The data exists locally, but the app has to remain read-only until the key arrives. Non-sensitive metadata can still be shown; sensitive values stay locked.

These are not exceptional edge cases around local-first architecture. They are part of the architecture.

Every case needs an explicit decision: which change wins, what remains available, when editing should be blocked, what can be retried automatically, and what the user needs to know.

## The cloud is not the app's front door

Truefold does not need to download the vault from CloudKit before showing the user their records. It opens the local vault and starts synchronization in the background.

That keeps the common path simple:

- Launch reads local data.
- Search runs locally.
- Records and attachments remain available offline.
- Saving does not wait for the network.
- Cloud problems appear as sync problems instead of making the entire app unavailable.

CloudKit is important, but it is not the front door to the user's own data. It keeps the devices in agreement and can return the vault data on a new iPhone. iCloud Keychain separately carries the encryption keys needed to unlock the sensitive parts.

## The interface has to tell the truth

Once saving and syncing are separate, the interface must stop treating them as the same thing.

A useful sync status should distinguish between states such as:

- Up to date
- Syncing
- Sync paused
- No iCloud account
- Offline
- iCloud storage full
- Waiting for an encryption key
- An app update is needed to read newer records
- Sync failed and will retry

Without that distinction, a cloud icon or a generic error message does not tell the user what they actually need to know: Is my change safe on this phone? Is it also available elsewhere?

## Local-first is a product decision

It is tempting to describe local-first as a storage implementation. In practice, it affects much more than where a file is written.

It decides when a user action is considered complete. It determines what continues working when the network disappears. It defines how devices disagree, how data recovers, and how honestly the app reports its state.

Offline mode is something an app can sometimes do. Local-first is a decision about where the user's data lives and how the product behaves when the cloud is not there.
