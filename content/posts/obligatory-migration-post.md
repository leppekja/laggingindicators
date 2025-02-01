---
title: "Obligatory Migration Post from Wordpress to Hugo"
date: "2025-01-26"
tags:
  - "coding"
  - "personal-projects"
draft: true
aliases:
  - ../../2025/01/26/obligatory-migration-post
---

This is the obligatory switching blog from Wordpress to a different blogging solution article.

Wordpress.com asked me for $78 for a two year subscription. However, I dislike the mobile app and the text editing experience online. I also had difficulties editing any part of the website with the in-browser tools. Originally, I signed up because I wanted my focus to be on writing, rather than on coding, a blog. Simply purchasing the Wordpress plan let me test whether or not I wanted to keep up with writing posts at all. It did that, but I didn't find that WordPress provided much additional value.

Given my time is up, and I enjoy the occasional post, I switched this blog over to Hugo hosted on Github Pages.

Once I decided on Hugo, installation was simple and I had the quickstart up from the command line. I did need to install PowerShell as detailed to avoid accessing it through WSL. I chose the Paper-mod theme after briefly browsing the examples for its simplicity.

I downloaded an export of my site from WordPress with the Export tool.

I used the wordpress-export-to-markdown library. The big issue is that this library does not support footnotes, which I use frequently. The rest of the library went really smoothly.

I enabled footnotes based on this post: https://lunarwatcher.github.io/til/hugo/footnotes.html, for when I do get them ported over.

I then followed the Host and Deploy on GitHub guide: https://gohugo.io/hosting-and-deployment/hosting-on-github/. I did need to set the Base URL in Hugo to the name of the Github Pages site, rather than the custom domain.

I had bought the custom domain through WordPress, so I had to set my primary domain back to the free WordPress labeled domain. Then, I set the A DNS records to the GitHub pages IP addresses, and mapped the CNAME `www` subdomain to my GitHub profile. This was the more confusing part, since I didn't understand if I had to detach the domain, or change the nameservers, or what exactly. Conveniently the WordPress documentation covers how to move domains into WordPress, but not so much the other way.

I also had to update aliases for older posts. Wordpress used a YYYY/MM/DD/ directory structure for each post, which I didn't retain when I completed the Markdown conversion. This meant that already indexed links were 404-ing. Hugo has aliases, which I add to add to each post.

I generated the script with Github Copilot: I tried GPT 4o, which was unable to do so and could not correct the regular expression to detect the date. However, Claude 3.5 Sonnet wrote a working script on its first try, and perfectly added the aliases to each file. This took about 15 minutes, most of it trying to get 4o to correct the syntax of the regular expression.

```
aliases:
  - ../../old-top-level-post-name
```

And this worked great! Until I realized that the date in the YAML for a few articles was different than the date in the original indexed URL. I think this is possibly to due with editing a page after it had already been published? This was irritating, but I pulled all the `<link>` elements from the WordPress XML export, and just checked the ~30 posts manually. Maybe another 30 minutes, including the time to figure out why some links were different.

So my list of regrets so far:

- went with a framework that allowed for easier exports, like [Obsidian](https://obsidian.md/). I didn't know at the time if I wanted to stick with WordPress.com, but I didn't realize that moving everything away from WordPress.com would be so difficult.

- not just continuing the post structure sorted by date.

To do:

- fix images in articles based on new path
- bring over footnotes to articles.
