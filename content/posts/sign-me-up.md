---
title: "Sign Me Up, Please"
date: "2025-5-18"
tags:
  - "coding"
  - "personal-projects"
draft: true
---

I'd like to take a popular class for 3d printing at the library, but I can't get a spot. The ideal LLM assistant could check for class availability and sign me up, without me monitoring the page constantly.

# Increasingly complicated attempts to sign up

MLK Labs in DC offers apparently highly desirable 3d printing classes. These are posted 30 days out - but have been booked solid for the past few weeks.

### Try 1

Monitoring the page hasn't worked out for me - my random checks every few days yielded nothing. Even when a new class was posted, I would be checking too late in the evenings and the class was full already.

This is the sort of task that would make a mobile LLM assistant valuable to me. A single question that could lead to a small script to check the public sign-up page and send me an alert when an open class is posted, or even sign me up without me needing to tak action, would be fantastic.

### Try 2

A small Python script to parse the page paired with a GitHub Action to send me an email about availability.

[This repository](https://github.com/leppekja/3dprintclassavailability) checks the page once a day to see if there is any availability and if so, makes a push request to the repository, which then triggers an email to me as a reminder to make an appointment right when I wake up.

In Github, under Settings, I enabled Email Notifications for push events for the repository. This saved me any work around setting a notification flow up, as the Actions workflow pushes a small text file if availability is found.

Since there isn't any availability on the page, I can't verify that I've set up the HTML parsing correctly since I don't know what the screen will look like. Given this, it just tries to check that there is something else than the "No spots" message, and considers that success.

A daily workflow like this is straightforward to set up - a quick Copilot check verified the cron expression and how to pass secrets properly to the script.

However, I set it to run once daily at 7am. The pipeline ran, and no availability was detected. I checked the page that morning and confirmed it. But at the end of the day, I checked again, and a class had been posted midday which I missed.

### Try 3

I updated the pipeline to take into account my guess about the class posting schedule. Classes are available every Tuesday (6pm), Wednesday (6pm), and Saturday (10am). Updating the cron expression to `1 14,22 * * *`
