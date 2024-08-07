---
title: "Using Firebase in SwiftUI App"
description: Connecting Firebase to SwiftUI App, fixing errors
image: 
date: 2024-07-21T15:02:31+02:00
lastmod: 2024-07-21T15:02:31+02:00

categories:
  - noise
tags:
  - iOS
  - dev
  - tutorial

draft: true

---

## Connecting Firebase to SwiftUI App

### Steps on Google Account Side
You need Google account. So go here [Firebase Console](https://console.firebase.google.com/) and create one.  

Create new project, and add an app - you will need a bundle ID (can be found in your main project settings) and optionally an Apple Store ID (can be extracted from you Apple Store URL).

You will get a `GoogleService-Info.plist` file. Download it.

You will also get the URL for the Swift Package Manager - copy it to clipboard.

### Steps on Xcode Side
#### Adding .plist File
Add the doenloaded plist file and add it to your project. Important: do NOT change this name and drop it into the root of your main app folder (where the other `.plist` files are)

#### Installing Firebase Dependencies
Use Swift Package Manager and past the URL into the search field. You shall see the Firebase package. Open it and let it sync.

Packages to install with explanation of the differences:

