---
title: "Obligatory Migration Post from Wordpress to Hugo"
date: "2025-1-26"
tags: 
  - "coding"
  - "personal-projects"
  - "research"
  draft: true
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

To do:

- fix images in articles based on new path
- bring over footnotes to articles.
