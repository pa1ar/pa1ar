---
title: "How to create a preview thumbnail for a ULR in Apple Notes on iOS and MacOS"
description: Creating thumbnails for URLs from within the Apple Notes
image: thumbnail-generated.jpeg
date: 2024-01-09T19:07:02+01:00
slug: thumbnail-apple-notes

categories:
  - noise
tags:
  - iOS
  - macOS
  - Apple Notes

draft: false

---

> Apple Notes creates beautiful thumbnails for URLs, when you share a URL to the note. Unfortunately, there is no straightforward way to create a thumbnail on demand while you are writing the note, e.g. I'd expect smth like long tap → create thumbnail or smth. 

Maybe someday we will have an easier way, but for now there is a workaround.

## How to create a thumbnail preview for a URL from within the Apple Notes on iOS (iPhone/iPad)

The trick is to **simulate drag-and-drop** while inside of the app.  
In other words, we need force the green  `+`  sign to appear.

<!-- <div align="center">
  <img src="apple-note-1.jpeg" alt="1ar.io here is a textual URL" width="50%">
  <br>
  <small style="color: rgba(255, 255, 255, 0.7);">1ar.io here is a textual URL</small>
</div> -->

1. create/open a note
2. paste/type textual URL to the note
3. dismiss keyboard (swipe down outside of the keyboard)
4. tap and hold on the URL
5. move it out of the way so that you can see the body of the note
6. tap with another finger on the body of the note (goal is to call the keyboard again)
- alternative: tap on UNDO (optionally on redo - to preserve the state of the note). *Discovered by [@soundsgoodtome on YouTube](https://www.youtube.com/shorts/8T088LlOxko).*
7. at that point, green plus should appear near the plate with the URL you should still be holding with your finger
8. point with the cursor where you would like to place the thumbnail
9. remove finger from the screen to paste the thumbnail

![holding URL plate without on-screen keyboard](apple-note-2.jpeg) ![holding URL plate with on-screen keyboard - green + is visible](apple-note-3.jpeg) ![thumbnail inserted](apple-note-4.jpeg)

For some really weird reason, sometimes this plus appears later, so occasionally the process needs to be repeated.

I've created a video to demonstrate the process a while back, but someone asked me in the comments to provide a text version of this trick.

{{< youtube 8T088LlOxko >}}

## How to create a thumbnail preview for a URL from within the Apple Notes on MacOS (Macbook)

You can do the same on MacOS, but the process is a bit different and even more weird. The principle remains the same - we need to drag-and-drop the URL to the body of the note.

> You must have a trackpad for this to work, as you will need forcetouch to open the quick preview of the URL.

1. create/open a note
2. paste/type textual URL to the note
3. use trackpad to click and hold on the URL
4. the preview will open (you can release the trackpad)
5. click and drag the URL from the preview; you need to select URL which will lead to the page you want to create a thumbnail for.
6. drag the URL-plate to the body of the note
7. release the trackpad to paste the thumbnail
8. repeat if needed, as you can drop as many thumbnails as you want from this preview (it won't close until you click outside of it)

![pick URL from the page you want to create thumbnail for](macos-1.jpg) ![drop it off to the note](macos-2.jpg)

Here is a video to demonstrate the process:

<!-- {{< youtube 8T088LlOxko >}} -->

I hope it helps and would be happy if you could let me know if you can reproduce this. Feel free drop a comment here, [tag/DM me on 𝕏](https://x.com/pa1ar) or drop a comment on [YouTube](https://www.youtube.com/@pa1ar)/[TikTok](https://www.tiktok.com/@1ar.io).