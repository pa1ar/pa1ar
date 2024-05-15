---
title: new project ｢ tldr ｣ Safari browser extension
description: Summarizing articles right from your default browser on iOS and MacOS.
date: 2024-05-12
image: cover_article_tldr.jpg
categories:
  - signal
tags:
  - macOS
  - iOS
  - PSA
weight: 1
---

> I am making an extension currently - tldr, to summarize articles in Safari browser. Mostly for myself actually. Using it already on daily basis.

## why 
I have tried using Arc Search browser as my daily driver, but somehow I always come back to Safari. I like the way it works, the way it looks. But I was missing one feature - the ability to summarize articles. So I decided to make one myself.

## notable features ::

- summary is stored locally, so if you revisit the website - you can read previously created one (not need to summarize again → money saving)
- works on iOS and MacOS
- uses your own OpenAI key - no subscription, pay for what you actually used  

<blockquote class="twitter-tweet" data-media-max-width="560"><p lang="en" dir="ltr">small project <a href="https://twitter.com/hashtag/buildinpublic?src=hash&amp;ref_src=twsrc%5Etfw">#buildinpublic</a><br><br>｢ tl;dr for Safari browser ｣<br>simple extension to summarize current page<br><br>- two-clicks operation<br>- stores summaries for visited URLs → saves money<br>- reads only pages which you allowed it to<br>- powered by your OpenAI API key<br><br>who wants to try?<br><br>↓ <a href="https://t.co/NTAfrYqBE1">pic.twitter.com/NTAfrYqBE1</a></p>&mdash; Pavel Larionov (@pa1ar) <a href="https://twitter.com/pa1ar/status/1784970385462640956?ref_src=twsrc%5Etfw">April 29, 2024</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## more info ::
Here I've created a [project page](https://1ar.io/projects/tldr).

Also I am sharing progress updates on [twitter](https://x.com/pa1ar/status/1784970385462640956) and on [threads](https://www.threads.net/@1ar.io/post/C6zvQinIplF).

### Most recent update ::
<blockquote class="twitter-tweet" data-media-max-width="560"><p lang="en" dir="ltr">progress update ::<br><br>- added settings (currently API key only)<br>- added streaming response into the field <br><br>it took me some time to figure out how to pass values between native Swift app and the extension, but now i can make it even more secure by adding keychain<a href="https://twitter.com/hashtag/buildinpublic?src=hash&amp;ref_src=twsrc%5Etfw">#buildinpublic</a> <a href="https://t.co/FR407awIQP">pic.twitter.com/FR407awIQP</a></p>&mdash; Pavel Larionov (@pa1ar) <a href="https://twitter.com/pa1ar/status/1789005215980527786?ref_src=twsrc%5Etfw">May 10, 2024</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## availability ::
Currently testing it. let me know if you would also be interested.