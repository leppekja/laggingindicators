---
title: "Play 8 Red Squares"
date: "2024-09-15"
categories:
  - "personal"
tags:
  - "coding"
  - "project"
aliases:
  - ../../2024/09/15/play-8-red-squares
---

I re-deployed [8 Red Squares](https://leppekja.github.io/8redsquares/), a site that allows you to play through the [8 Queens Puzzle](https://en.wikipedia.org/wiki/Eight_queens_puzzle) on different sized boards.

![](https://laggingindicators.wordpress.com/wp-content/uploads/2024/09/image-5.png?w=644)

Future enhancements I'd like to add are

- Timer (for each segment), and total running time until the users solves n=8.

- Better mobile support (board size 9 and 10 don't handle small screens well).

- Foundational solutions checklist, e.g., if a user wants to find all possible arrangements (excluding symmetries and rotations).

- Assistant, e.g., allow a user to turn on CSS highlights that show conflicting squares, or hints.

- Optionally pause switching board sizes, allowing the player to examine their solution, e.g., a "Jump to next puzzle" option, or retain the solution when they navigate to a previously solved board size.

Play it at [https://leppekja.github.io/8redsquares/](https://leppekja.github.io/8redsquares/). The site uses React; I originally built it to learn React and how to deploy a site to a cloud-based provider (AWS Amplify), but now uses the `gh-pages` package and is deployed on GitHub Pages.
