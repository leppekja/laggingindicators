---
title: "Obligatory Migration Post from Wordpress to Hugo"
date: "2025-01-26"
tags:
  - "coding"
  - "personal-projects"
draft: false
aliases:
  - ../../2025/01/26/obligatory-migration-post
---

This is the obligatory moved blog from one framework to a different framework article.

Wordpress.com asked me for $78 for a two year subscription. However, I dislike the mobile app and the text editing experience online. I also had difficulties editing any part of the website with the in-browser tools. Originally, I signed up because I wanted my focus to be on writing, rather than on coding, a blog. Simply purchasing the Wordpress plan let me test whether or not I wanted to keep up with writing posts at all. It did that, but I didn't find that WordPress provided much additional value.

Given my time is up, and I enjoy the occasional post, I switched this blog over to Hugo hosted on Github Pages.

Once I decided on Hugo, installation was simple and I had the quickstart up from the command line. I did need to install PowerShell as detailed to avoid accessing it through WSL. I chose the Paper-mod theme after briefly browsing the examples for its simplicity.

I downloaded an export of my site from WordPress with the Export tool.

I used the [wordpress-export-to-markdown](https://github.com/lonekorean/wordpress-export-to-markdown) library. The big issue is that this library does not support footnotes, which I use frequently. The rest of the library went really smoothly. This took maybe a a half hour or so, including research on options.

The author of the library is transitioning to v3, so I used Github Copilot (Claude 3.5 Sonnet) to write a script to extract all footnotes, convert them to markdown format, then export them all to another markdown file separated by post title. Generating a working version of this took about 15 minutes.

```
# Claude 3.5 Sonnet after a few back-and-forths
# Load XML
[xml]$xml = Get-Content "<your-file-website.wordpress.export.xml>"

# Create namespace manager
$nsManager = New-Object System.Xml.XmlNamespaceManager($xml.NameTable)
$nsManager.AddNamespace("wp", "http://wordpress.org/export/1.2/")

# Function to convert HTML links to markdown
function Convert-HtmlToMarkdown {
    param([string]$html)
    while ($html -match '<a href="([^"]+)">([^<]+)</a>') {
        $url = $matches[1] -replace '\\/', '/'
        $text = $matches[2]
        $markdown = "[$text]($url)"
        $html = $html -replace [regex]::Escape($matches[0]), $markdown
    }
    return $html
}

# Create output file
$output = "footnotes.md"
"# Footnotes from Posts`n" | Set-Content $output

# Process each post
$posts = $xml.rss.channel.item
foreach ($post in $posts) {
    # Extract title properly from the title node's inner text
    $title = $post.SelectSingleNode("title", $nsManager).InnerText
    if ([string]::IsNullOrEmpty($title)) {
        $title = "Untitled Post"
    }

    $postmeta = $post.SelectNodes("wp:postmeta", $nsManager) |
                Where-Object { $_.SelectSingleNode("wp:meta_key", $nsManager).InnerText -eq "footnotes" }

    if ($postmeta) {
        $metaValue = $postmeta.SelectSingleNode("wp:meta_value", $nsManager)
        if ($metaValue -and $metaValue.'#cdata-section') {
            try {
                $footnotesObj = $metaValue.'#cdata-section' | ConvertFrom-Json

                # Add post title if we have valid footnotes
                "`n## $title`n" | Add-Content $output

                # Process each footnote
                foreach ($note in $footnotesObj) {
                    $content = Convert-HtmlToMarkdown $note.content
                    "[^$($note.id)]: $content`n" | Add-Content $output
                }
            }
            catch {
                Write-Warning "Failed to process footnotes for post: $title"
                Write-Warning $_.Exception.Message
            }
        }
    }
}
```

I enabled footnotes based on this post: https://lunarwatcher.github.io/til/hugo/footnotes.html, for when I do get them ported over.

I also had to update the links to the footnotes in the blog post itself:

```
# Get all markdown files in posts directory
# Claude 3.5 Sonnet
Get-ChildItem -Path "<path-to-folder-with-posts>*.md" | ForEach-Object {
    try {
        # Read file content
        $content = Get-Content $_.FullName -Raw

        # Create backup
        Copy-Item $_.FullName "$($_.FullName).bak"

        # Replace footnote references using regex
        # Matches [any_number](#uuid) and captures the uuid
        $newContent = $content -replace '\[\d+\]\(#([a-f0-9-]+)\)', '[^$1]'

        # Write back to file
        $newContent | Set-Content $_.FullName -NoNewline

        Write-Host "Updated footnotes in $($_.Name)"
    }
    catch {
        Write-Error "Failed to process $($_.Name): $_"
    }
}
```

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

This type of work is a bit irritating to me - all this to avoid a not-very-large fee to reproduce something that already exists. That said, the practice with Git, PowerShell, and website deployment is always helpful.

To do:

- [ ] fix images in articles based on new path
- [x] bring over footnotes to articles.
- [x] update URLS.
