---
title: "How to Fix Alibaba Images Not Loading"
description: Sometimes images of the products stop loading on Alibaba and Aliexpress. Here is how to fix it.
image: cover_article_alibaba.jpg
date: 2024-08-07
lastmod: 2024-08-07
slug: fix-alibaba-images-not-loading

categories:
  - noise
tags:
  - web
  - tutorial

draft: false
---

> Sometimes some of the images on Alibaba or AliExpress stop loading.
> The common reason for that is in DNS or router settings blocking content delivery domains.

Let's fix this issue, focusing on Fritzbox routers with additional info for other popular brands.

## How to Fix on Fritzbox Routers

1. Access Fritzbox Interface:
- Open local admin dashboard [http://fritz.box](http://fritz.box) or [http://192.168.178.1](http://192.168.178.1)
- Log in
2. Navigate to Filter Settings:
- "Internet" > "Filters"
3. Create New Filter Rule:
- "Lists" tab > "Allowed Websites" > "New"
4. Paste domains you want to whitelist
```
aliexpress-media.com
ae01.alicdn.com
ae04.alicdn.com
g.alicdn.com
s.click.aliexpress.com
```
5. Save
6. Reconnect or Restart Router
- To reconnect: "Internet" > "Online Monitoring" > "Reconnect"
7. Reload the page on Aliexpress/Alibaba and/or clear browser cache

## Fixing on other Routers
### Netgear
1. Access admin page
- URL: [http://192.168.1.1](http://192.168.1.1) or [http://routerlogin.net](http://routerlogin.net)
3. "Security" > "Block Sites"
4. Add aliexpress-media.com to allowed list

### Linksys
1. Access admin page
- URL: Typically [http://192.168.1.1](http://192.168.1.1)
3. "Security" > "Access Policy"
4. Allow aliexpress-media.com

### TP-Link
1. Access config page
- URL: Usually [http://192.168.0.1](http://192.168.0.1) or [http://192.168.1.1](http://192.168.1.1)
1. "Security" > "Access Control"
2. Whitelist aliexpress-media.com

## Additional Troubleshooting
1. Check DNS Settings on your router and/or in your system:
- Try Cloudflare DNS (1.1.1.1 and 1.0.0.1), or [try app](https://one.one.one.one/)
- Try Google DNS (8.8.8.8 and 8.8.4.4)
2. [Use a VPN](https://surfshark.club/friend/hdzrYxTX)
3. Try Incognito Mode
4. Disable Content/Ad Blockers

## Conclusion
> Whitelisting key domains and adjusting DNS settings often resolves Alibaba image loading issues.