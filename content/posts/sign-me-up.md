---
title: "Sign Me Up, Please"
date: "2025-05-22"
tags:
  - "coding"
  - "personal-projects"
draft: false
---

I'd like to take a popular class for 3d printing at the library, but I can't get a spot. The ideal LLM assistant could check for class availability and sign me up, without me monitoring the page constantly.

Unfortunately we are not there yet.

# Increasingly complicated attempts to sign up

MLK Labs in DC offers apparently highly desirable 3d printing classes. These are posted 30 days out - but have been booked solid for the past few weeks.

### Try 1

Monitoring the page hasn't worked out for me - my random checks every few days yielded nothing. Even when a new class was posted, I would be checking too late in the evenings and the class was full already.

This is the sort of task that would make a mobile LLM assistant valuable to me. A single question that could lead to a small script to check the public sign-up page and send me an alert when an open class is posted, or even sign me up without me needing to tak action, would be fantastic. So I tried to see if this is possible.

There are also a number of services that offer this sort of webpage change detection - [Visualping.io](https://visualping.io) is one that comes up when I ask that boasts 2 million + users - specifically advertising Appointment Availability notifications as a use case. But I was curious about what would be possible on my own.

### Try 2

A small Python script to parse the page paired with a GitHub Action to send me an email about availability.

I used GitHub Copilot running Claude 3.7 Sonnet for all questions and coding adjustments, along with tool usage so the LLM would be able to access and see the page.

[This repository](https://github.com/leppekja/3dprintclassavailability) initially checked the page once a day to see if there is any availability and if so, makes a push request to the repository, which then triggers an email to me as a reminder to make an appointment right when I wake up.

In Github, under Settings, I enabled Email Notifications for push events for the repository. This saved me any work around setting a notification flow up, as the Actions workflow pushes a small text file if availability is found.

Since there isn't any availability on the page, I can't verify that I've set up the HTML parsing correctly since I don't know what the screen will look like. Given this, it just tries to check that there is something else than the "No spots" message based on the CSS, and considers that success.

A daily workflow YML file like this is straightforward to set up - a quick Copilot check verified the cron expression and how to pass secrets properly to the Python script.

However, I set it to run once daily at 7am. The pipeline ran, and no availability was detected. I checked the page that morning and confirmed it. But at the end of the day, I checked again, and a class had been posted midday which I missed.

The issue was that the CSS selector that the LLM used was not a valid selector on the page at all, so no result would ever be returned. I adjusted this based on looking at the DOM to use a regular expression to look for a digit followed by "spots left", but this failed due to the appointments on the page being loaded by JavaScript. Still, this eliminated the need for the BeautifulSoup library and any HTML parsing.

### Try 3

I asked the LLM to use Selenium instead, with worked in a single try. The YML file also correctly installed Google Chrome in the workflow.

I also updated the pipeline to take into account my guess about the class posting schedule. Classes are available every Tuesday (6pm), Wednesday (6pm), and Saturday (10am). Updating the cron expression to `1 14,22 * * *` enabled the script to check the page right after.

### Lessons

One thing I noticed is that the LLM was consistently introducing additional complexity. For example, it would add code to parse availability for every class based on the page structure, where as I really only intended to pull it for the newest posted class. But with each change to the code, the LLM would revert back to adding a for loop to check all possible dates even after I removed it.

Another case of this is when I explained that I'd like to send myself an email as part of the context of the task, it wrote code and an Action that would send me an email from the GitHub action itself by connecting to an email serve, rather than the simpler option of enabling Email notifications on the repository itself. Tasks like this I'm fine with the tradeoff of brittle and unpolished for simplicity.

I suppose my context was misleading, but I'm curious about how using these tools will eventually integrate with the native UI of my phone and what scripts they could write and run. Will it consistently be me providing logins to services I use that offer a LLM-assistant API? Would they recommend me a service, offer to sign up for me (contigent on a payment and referral percentage for the LLM provider, probably) and navigate through the UI of the site? Or similar to how I can write PowerShell code on my computer, will phones offer the capability to write and run machine-generated custom scripts locally for set tasks?
