---
title: "Lessons learned on dashboard design for early-stage apps"
date: "2024-09-07"
categories: 
  - "data-visualization"
tags: 
  - "analytics"
---

What follows are a few takeaways from a recent app launch, where I had some successes and some things I had to fix. I also maintain dashboards for a wide set of clients on a different project that will be overhauled soon, and would like to apply these learnings during the redesign.

- **The three guiding business questions for initial design are:**
    - How are we getting users?
    
    - What our users are finding value in when using the application?
    
    - What problems are users are encountering when using the application?

Perhaps this verges on trivial, but it was helpful to think about these three questions when working with little input from stakeholders. This also shapes how the dashboard is organized, providing a storyline to follow, or natural groupings in which to place metrics and charts within. Users may not look at dashboards until the application has launched, and they may not have feedback until after they see something they dislike or don’t understand.

Of course, these will become more specific over time, so it is essential to stay close the business needs and monitor the questions being asked and results being shared, then to build these into the dashboards. At the top level, consumer questions might be very vague, e.g., how is the product doing, or has anything changed in April that I should know about? But that’s okay - start with general indicators, and over time, incorporate questions that are commonly raised. At the same time, you need to be able to defend and recommend: for the former, to justify the decisions behind what to show on the dashboards, and for the latter, demonstrate why certain variations are better proxies than others.

The audience for your dashboards will obviously influence the granularity of the sub-questions address. Executive leadership may care less about device types past whether web or mobile users are driving adoption, while specific device versions may serve as a key data point for product design teams.

Lastly, when having initial conversations, dashboards should be discussed as cohesive reports. Many people, when deciding they want a dashboard, quickly start listing out metric definitions rather than focusing on the questions they value. Oftentimes, people will need guidance on what metrics exactly should even be used to measure something, so starting with ‘what to track and what to show’ is backwards. Instead of giving people the implicit definition of charts on a page (and people will happily tell you if they want a bar chart or pie chart rather than focus on whether the metric is meaningful), frame the conversation around answers to questions, like a research report was being done. The stakeholders should not design the dashboard!

- **Charts should explain themselves.**

I wrote what I thought was a nice user-friendly explanation of what a chart meant right next to it. I then received many questions about what the chart meant and how to use it.

Unfortunately, data visualizations are often not as easy to understand from the consumer point of view as I think they are. I fall into this trap frequently, trying to design a beautiful and engaging chart that fails in its key goal: deliver information effectively. This is the core of every chart in a dashboard, and more often than not, a large number with a title, subtitle, and basic trend indicator is going to be easier and quicker for a user to understand.

There is an art to writing good helper text. I like to have the title of the chart be a question directly addressing a business need, e.g., how many users are active today?, and have the subtitle should answer the question.[1](#410c1ca3-a3cf-4186-911d-31e7ca803dc9) The subtitle of the chart should embed the metric into plain text, e.g., 37 users logged-in and viewed a post today.[2](#f7e3ca3b-3e71-4b76-97df-0141ab99dea8) The title guides the user to what they are looking for, and the subtitle tells the viewer how to use the answer in a sentence.[3](#e4372ea8-af5b-4b11-a935-11c2a45902dc)

Technical definitions and sources should be embedded, but not intrusive. Standard footnotes should suffice, and fuller documentation should be linked to for complex calculations. As a simple example, take the chart “how many users viewed profiles?”: a subtitle may state “32% of users viewed more information from the search results for at least one restaurant”, and the footnote can state “measures the count of unique logged-in users this month divided by total active users this month with at least one click on a More Information button in at least 1 session.” However, if there are projections about the percent of users that will click it next month, I would just include a link to a document that explains the assumptions behind those.

A number is almost never a simple division like on the product analytics websites claiming it is. The application or site will go through many UI and UX changes.[4](#b4f8e547-03f7-4c1e-b184-3f5edec2cf7c) Bugs in tracking happen, or the product changes, or things are deleted or inactivated, or testing and bot data need to be cleaned and removed… Some instances will require you to share this in the published dashboard, while others won’t, but these adjustments are context specific. A helpful rule for me is if they affect the visual appearance of the chart, then I need to either change the chart or add a note.[5](#3869e54c-ab66-4281-bb24-dc38f448dbba)

Lastly, well-formed titles, subtitles, and footers should point consumers in the right direction about limitations when using the numbers. Measurable numbers are usually proxies for what we want to know. Once a number is put on a dashboard, you have no control over how it is shared out or with what context. The distinctions between users, sessions, visitors, page views are technical, more technical then most consumers will care to distinguish between.

So be mindful of what terms you use, and explain limitations succinctly. For example, for a chart titled “Which products are potential users looking at?,” I would rephrase the subtitle “Product A’s landing page was the most popular in May” to “Product A’s landing page had the most online views in May, based on total anonymous page visits across web and mobile.” This does not explicitly state the limitations, e.g., a user could have loaded the same page over and over on different unlinked devices to game the metric, but it at least acknowledges the calculation method and gives the consumer a starting point to discuss or ask about. And as always, you can include a technical definition in a footer or additional usage documentation for users who are interested.

- **Curate charts aggressively to key needs only.**

Too many variations of a chart with similar business outcomes will lead to user confusion, and users will gravitate toward the best looking number. Obviously, complementary numbers are needed, like including rolling averages of active users that make long-term trends clearer.

Demonstrate events in relation to the relevant or entire user base, depending on which is appropriate. Other segments of interest could be different time periods, sub-populations, or similar actions, but these should be curated such that any comparison from one segment to another is always easily interpretable, statistically valid, and relevant to business questions.

The discovery, sign-up, and onboarding flow will be the primary focus. It might be assumed within the group that the app provides value to the user, and that the discovery and onboarding is the hump to go over. This means defining the funnel for acquisition and activation and clearly communicating this. At the same time, avoid steps that lead to over-optimization early on, e.g., click-through rates on intermediate pages that will be targeted as ‘inefficiencies’ and gamed to increase rates. Focus on the actions that should be providing value.[6](#e599a364-2668-40d5-bf8f-77d29229363a)

I think the only exception to this is showing vanity metrics, e.g., those that will only go up, those that have little practical use for decision making, etc. as these will be requested for marketing use. Again, you will need dashboards that show the success of the application, and I should not disregard this. An example of this would be something like the all time number of page views, which might be helpful for a LinkedIn post, but nothing else. Metrics that are only useful for marketing are valuable for the business, which is something that I have not appropriately recognized in the past at times. In these cases, pair marketing metrics with value actions, e.g., conversion rates or returning rates, to emphasis their importance, or create a separate section entirely, depending on the audience.

- **Chart design should accommodate growth.**

Gracefully handle extreme values and lopsided outcomes in visualizations. When choosing a chart type, ask how will this look in a situation where all values are zero, or when only one data point is very high, or the distribution is heavily skewed?

Prepare for situations where values are zero. Handle null values, divisions by zero, and appropriate wordings of titles and subtitles. For me at least, bugs inevitably will surface on the dashboards - how can the chart fail without degrading the user experience entirely? For example, consider a chart stacked bar chart that shows the percent of users that are new versus returning today. The subtitle could read “{X}% of users today are returning users;” in this case, if zero users were active today, then the subtitle should update its wording to say “No users were active yet today” rather than “NaN% of users today…”

Prepare for situations with heavy clustering when using charts that display individual features, e.g., scatter plots, dot plots. What if growth occurs very quickly? In faceted or stacked charts, what if one segment increases very quickly (and causes outlier or scale issues?) The chart design should consider the hypothetical distribution of possible values to the best possible extent. A representative example: consider a single stacked bar ([like this](https://observablehq.com/@observablehq/plot-stacked-percentages?intent=fork)) with a segment label overlaid directly on each section of the bar. If one segment accounts suddenly for a large majority, the labels of other sections will be squished together and overlap into an unreadable mess.

- **Trust is lost quickly, and don’t assume it exists at the outset.**

Consistency is key, and the difficulty of managing this is multiplied by how many reporting systems exist. Small differences will be quickly identified and questioned; as a trivial example, an internally surfaced activity log may not have test users filtered out, but dashboards do filter out testing traffic. Another BI user may calculate things differently then you. A key example I have dealt with is reconciling and explaining internal logging versus Google Analytics tags, where ad blockers may contribute to sizable differences in reported numbers. Deliberate coordination is required in these instances, but very clear documentation about calculations can prevent some of these issues.

I’ve found myself in the situation on a different project where, because of data quality issues arising from tracking bugs in the past, users may check the dashboard, then send a note to confirm that the number is accurate to use for many months after. Additionally, some users may not care to even log a report and instead simply disregard the dashboard entirely. When people are first looking at the dashboards, they may come in with doubt as well, unsure about what the numbers mean and how to use them. It is an open question for me as to how to ameliorate this issue apart from accessible design.

The dashboards need to be fast and visually appealing. It may seem odd to bring this up under the topic of trust, but I think for many people, this is a signal of competence. People will not wait 15 seconds for a large set of metrics to load. Start with small amounts of pre-aggregated data, then load more where needed at the user’s request. I work with a system where the dashboard refreshes after the user has loaded the page with cached data, so they may be on the dashboard, then suddenly see the numbers change, which is poor design, especially when it takes a minute to update.

On the visual side, creating charts in Excel might work, but people don’t find them engaging. In many cases, dashboards are just reflections of progress, and progress doesn’t feel as good when it doesn’t look pretty. Professionalism goes a long way, and a modern design is a part of this.

- **It is inevitable that users will have questions that aren’t answered in the dashboard. I personally don’t think this is a failure, but you need a process for it.**

Questions become reports. Reports which need to be repeated should become dashboards. Do not try to answer a question with a dashboard, but with a report. The dashboard is a live summary of the underlying validated research.[7](#3f903db1-3ec0-489a-a50f-faffa774abd5)

The minimum ‘user journey’ for a dashboard consumer is to have a question, identify what the answer to the question is on the dashboard, then be able to form a good ‘why’ question based on this that they can follow up on if needed. So a user having a question on a dashboard and wanting to dive deep is okay![8](#8b2a608d-4302-4ea3-9729-6dfffb3043b5)

An approach for a metric in a dashboard I have heard is overview number, drill down to a pre-defined segment, then row view of the data. To me, this is the best version of the ‘self-serve analytics’ trend in which products promise that dashboard consumers would be able to answer questions for themselves.

Self-serve analytics doesn’t account well for early stage launches in which it is more common for bugs and tracking issues to find their way downstream. Investigative questions, or why something occurs, need to be explored fully, which may require looking over data quality, third-party data, custom queries, etc., for small teams. It also assumes a high level of statistical knowledge and data architecture for anything past basic applications and questions.[9](#17739a64-9cd0-4b3a-a63f-0eea7f0dd189) It requires consumers to properly translate their questions into a calculation, which is an acquired skill. Alternatively, it takes a large amount of work for the dashboard designer to constrain users to only certain comparisons that are valid.

The proper workflow should be a dashboard that a user has a ‘why’ question on, which is then referred to a analyst, who then creates a specific report based on expert knowledge of the way the data is collected and aggregated. This is the subject of many complaints, that these questions can’t self-answered within the dashboard, but personally, I think this is the exactly right workflow. Dashboards should allow people to form good questions that can then be handed over to a subject-matter expert.

Lastly, people will want to export the data to review it themselves in Excel or Google Sheets, and they will need a CSV download. If you do include this, expect many questions from them about their self-aggregated findings. If you do not, expect requests for exports. My best advice is make exports available in the context of getting data, e.g., I need the names of contacts of this segment to reach out to them, but not for external data analysis.

- **Prioritize flexibility when choosing an initial framework.**

Having used dashboard software that prioritize UI builders, e.g., Power BI, Tableau, I would recommend lower-level dashboard frameworks. I agree with the current marketing campaign for [Observable Framework](https://observablehq.com): “the best dashboards are built with code.”

Recently, I deployed Observable Framework through an Azure Static App from a GitHub repository, leveraging GitHub Actions to update data regularly and the Azure Entra ID for authentication (but they do just have a easy password protection also). Certainly, learning this took more work than it would have to pay a larger sum of money to just leverage an existing platform. Yet it keeps costs low, is extensible, offers professional visualizations, allows for clear metric definitions in code or templates that are not locked into a vendor’s platform, makes extending existing visualizations into longer one-off reports easy (with Observable notebooks), and leverages common languages (vanilla JS, Python, YAML) rather than vendor-specific terminologies or languages. Note that learning Plot (and D3 where needed) is still a big lift, but the declarative nature of Plot is very similar to other visualization libraries that future analysts should be familiar with.

Note that for many applications, a plug and play solution (Google Analytics, PostHog, etc.) might be the simplest to start with. Note that this won’t work for applications that work with regulated data, (HIPAA, for example), and still requires a significant amount of management, experience, and maintenance.[10](#82d1bb7e-e40f-4c55-8ead-33f7e86377e4) With teams I’ve worked with, Google Analytics is the standard that users will make comparisons to if you do decide to build something outside of this. Additionally, as a solo developer working on projects - if I only need a dashboard for myself - I leverage open source projects that avoid third-party tracking, but do provide pre-built dashboards.

Authentication and data security is frustrating to deal with. Ensure that your dashboard tooling can access your database prior to trying to build anything on it, that users will be limited to their permissions, that managing users and permissions can scale, etc. But that is outside the scope of this post.
