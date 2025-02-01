---
title: "SNAP State Performance Indicators Datasets"
date: "2024-02-21"
categories: 
  - "snap"
tags: 
  - "analytics"
  - "data"
  - "project"
aliases:
  - ../../2024/02/21/snap-state-performance-indicators-datasets
---

I started to post [SNAP performance indicators](https://www.fns.usda.gov/snap/efficiency-effectiveness-measures) from the USDA in machine-readable format, aggregating and cleaning the data from the many PDF tables. [This repository](https://github.com/leppekja/SNAP-performance-indicators) includes quick-access to cleaned, FIPS-linked CSV long-format tables of yearly state reported indicators for [Application Processing Timeliness](https://www.fns.usda.gov/snap/qc/timeliness), [Program Error Rates](https://www.fns.usda.gov/snap/qc/per), and [Case and Procedural Error Rates](https://www.fns.usda.gov/snap/qc/caper) (coming soon).

USDA Descriptions as follows:

- Application Processing Timeliness "measures the timeliness of states’ processing of initial SNAP applications. The Food and Nutrition Act of 2008 entitles all eligible households to SNAP benefits within 30 days of application, or within 7 days, if they are eligible for expedited service."

- Payment Error Rates (PER) "measures how accurately a state agency determined SNAP eligibility and benefit amounts for those who participate in SNAP. Errors include both overpayments -- when households receive more benefits than they are entitled to – and underpayments – when households receive less benefits than they are entitled to."

- Case and Procedural Error Rates (CAPER) "assesses the accuracy of state agency actions in cases in which applicants were denied, terminated, or suspended and did not receive benefits. It also measures a state's compliance with federal procedural requirements, including the timeliness and accuracy of notifications sent to affected households."

Planning to use this as a simple dataset for exploratory data analysis and test visualizations. I added [FIPS codes](https://www.census.gov/library/reference/code-lists/ansi.html), and the data is in [tidy format](http://www.jstatsoft.org/v59/i10/paper), so should be very easy to go straight to visualization.

Note: this data is not synced regularly. Be careful when using the data and ensure it matches any posted documents. Data was accessed Feb. 2024, and extracted with OCR. Rates are published yearly. I don't see a way to be notified if any corrections are made, so data will be refreshed with each USDA release.

The [GitHub repository is available here](https://github.com/leppekja/SNAP-performance-indicators). You can download the data or read it into dataframes easily with most popular libraries by accessing the raw content, e.g.,

`df = pd.read_csv([https://raw.githubusercontent.com/](https://raw.githubusercontent.com/)...repository_file_path)`
