---
title: "Reflections on Building a COSS Personal Finance App"
---

TL;DR - From late 2021 to mid 2025, I worked on "Maybe", the personal finance app. Maybe has since pivoted, and we're now working on a B2B financial planning and forecasting app to help business owners gain insights into their finances without complicated spreadsheets. This post is about the _prior_ personal finance app.

## Preface

Here's the (short) story of how we landed here.

I've been working at Maybe Finance Inc. since 2021. But as it sometimes happens at a startup, my path has not been linear. In short, I worked on a team of 3 engineers (I was the first hire) from 2021-2023 building a closed-source, personal finance app. We built an awesome product, but funding ran out, and the company effectively shut down in mid-2023. At this time, I decided it was my chance to finally put some serious work into growing my golf websites [The DIY Golfer](https://www.thediygolfer.com) and [Local Golf Spot](https://www.localgolfspot.com), which I had been growing for several years in a part-time capacity. I wanted to prove out whether there was a business in either of these sites, so I worked hard on this for ~12 months until one day, Maybe's founder decided to open-source all the code we had written for the V1 version of the personal finance Maybe app (with permission of course). Well... It went viral on Github, and the company raised another pre-seed round of funding. At this point, I was asked if I wanted to work on this again as a COSS (commercial open source) version of the prior app. I had to make the tough decision of whether to continue working on my own thing or make a second attempt at the app that had failed prior.

I [decided to take the offer](https://x.com/zg_dev/status/1751988660646424979) and dive head-first into the world of open-source software. I was absolutely pumped.

Our first major decision for the V2 version of Maybe was tech stack. We had previously written the app in Next.js and had deployed everything to AWS primarily for compliance reasons. The prior version of our app had a financial advising component to it, which meant we had to be registered as an advisor with the SEC and were subject to a huge list of compliance requirements. In other words, our prior tech stack was relatively complex _by necessity_, and given we were pivoting to an OSS app that self-hosters would be running on their own machines, we felt that this complexity was going to weigh us down from the start.

While we knew it would require a lot of re-building, we decided to scrap the Next.js version of the app and start over with Ruby on Rails, which would provide our now very small team (1 engineer, 1 designer) a productive framework that was easy to self host.

We got to work, and over the course of the next year or so, we made a ton of progress and quickly surpassed our "V1" app in features. I got a crash course in maintaining a popular OSS repo, we started a successful code bounty program, we got 2M+ package downloads, amassed 50k+ stars on Github, and launched a commercial hosted offering. Fortunately for the OSS community, but unfortunately for our business, the self-hosted app became wildly popular, but the hosted offering failed to generate the revenue growth we needed. It was at this point our founders made the [difficult decision to pivot](https://x.com/Shpigford/status/1947725345244709240) to what we're now working on.

As the lead developer of both the V1 and V2 versions of this product, it was bittersweet to shut it all down for a second time. I gave it my all and still believe we built one of the simplest, highest quality personal finance apps on the market (I still use the self-hosted version for my family's finances).

All that said, business is hard, and business in the fintech industry is harder. While I was primarily working on the engineering side of things, there were a lot of technical and business lessons learned over the years of trying to make this app work. I've summarized many of them in the final release I shared with our OSS community below.

## Final Release (repost)

_Below is the [repost](https://github.com/maybe-finance/maybe/releases/tag/v0.6.0) of my final release notes, which I wrote up to reflect on some of the biggest challenges of building a personal finance app._

### Thanks to our contributors and beta testers!

Before I talk too much about the engineering and product side of things, we want to extend our gratitude to our awesome contributors! We had hundreds of individuals contribute to the project and even more helping report bugs, suggest features, and raise new ideas.

This is a huge project and we couldn't have gotten this far without our early supporters and OSS contributors.

### Demo

Below is a quick demo of what the Maybe app can do. You can learn how to deploy it following our [Docker Setup guide](https://github.com/maybe-finance/maybe/blob/main/docs/hosting/docker.md).

<video controls width="100%">
  <source src="https://cdn.zachgollwitzer.com/maybe-demo.mp4" type="video/mp4">
</video>

Self hosted features include:

- Dashboard to see your net worth trend over time and current personal balance sheet
- Rich transaction filtering, searching, bulk updating, and viewing
- Account views with the ability to see balance trends, reconciliations, and detailed breakdowns of your balance changes
- View your monthly budgets with category averages, income summary, and more
- Intelligent AI chat that knows your finances and can answer questions about them
- Multi-currency support
- API with key management to build automations on top of Maybe
- CSV imports for accounts, transactions, and trades
- Categories, Tags, Merchants
- Rules to auto-categorize your transactions, detect merchants, and more
- Invite members to your household
- 2FA

### Recap, Reflections

For many of you, this final release comes with a sense of disappointment, and that's understandable. We're not excited about this either, and wish we had unlimited time and money to fulfill our vision for this app (we have years worth of new feature designs in the backlog)! In the end, our obligation as a company is to our investors, and as outlined in [Josh's post here](https://x.com/Shpigford/status/1947725345244709240), we've determined that continuing to build this app is not our best chance at paying back our investors and profiting as a company.

That said, over the last few years, our team has dedicated some serious amount of thought asking the question, "How can we build a better personal finance app?". And in that light, I'd be remiss not to share some of our biggest successes, failures, and unexpected challenges along the way. We hope that some of these reflections propel the OSS community forward and one day lead to a truly great, OSS personal finance app.

### The wins

We've been thinking about personal finance for a lot of years. This isn't our first attempt at creating this app, and along the way, we've learned some valuable lessons that have made it into the final release of this software.

#### A simple, beautiful personal finance app

![Maybe Dashboard](/assets/images/maybe-dashboard.png)

Let me give credit where it is due. Our lead designer @justinfar has done an incredible job crafting a _simple and clean_ UI. Some of you may look at our app and think, "That's it?". But believe us when we say, this was intentional. We spent a lot of time asking the question, "What can we delete?".

Our goal with this app was to _remove complexity_ from personal finance. Many personal finance apps overwhelm the user with dashboards, complex UIs, and too much detail. We believe that many people _want_ to track their finances but are simply too overwhelmed by the challenge of getting started. Most people need to know just a few important things about their finances:

- How much money do I have?
- Am I spending less than I'm earning?
- How much did I spend last month?
- What am I spending my money on?

This app answers all of those questions and gives the user a simple interface to categorize, organize, and get to those answers quicker. While some users might look for a richer feature set, we believe most users are satisfied with less; not more.

#### A place for everything

One of our greatest frustrations with other personal finance apps is not having a "place" or a "home" for common financial scenarios.

One common scenario in personal finance is a "transfer". A transfer is a movement of funds from one account to another, and creates a transaction of opposite values on each of those accounts. Many personal finance apps treat these the same as the rest of your transactions and force you into this uncomfortable pattern of using a "Transfer" category or "Excluded" category to filter this noise out of your budgets and metrics. Furthermore, some transfers (like loan payments) should be included in your budgeting while others (like credit card payments) are a "double-count" that should be _excluded_ from budgets. It forces users to create a "Junk drawer" for the things they don't know what to do with.

The Maybe app treats transfers as first-class citizens. We auto-detect your transfers and _don't even allow_ you to categorize them because they _shouldn't be categorized_. This way, transfers are automatically excluded from budget totals and other metrics.

<video controls width="100%" autoplay loop muted playsinline>
  <source src="https://cdn.zachgollwitzer.com/maybe-transfers.mp4" type="video/mp4">
</video>

Another common scenario is "one time expenses". Similar to transfers, these expenses (e.g. "moving expenses") don't really belong in budgets either. The Maybe app allows you to mark these as "one time" and automatically excludes from your "average spending" and other budget totals.

<video controls width="100%" autoplay loop muted playsinline>
  <source src="https://cdn.zachgollwitzer.com/maybe-onetime.mp4" type="video/mp4">
</video>

Outside these common scenarios, the Maybe app also allows you to create various account types and even "reconcile" them to a new balance without creating that nasty "adjustment" transaction that nobody knows how to categorize or what to do with.

<video controls width="100%" autoplay loop muted playsinline>
  <source src="https://cdn.zachgollwitzer.com/maybe-reconcile.mp4" type="video/mp4">
</video>

We set out to create an app that has a place for all your financial scenarios, and while I think there is still work to be done on this front, we handled the most important ones (unlike many other apps). And we're proud of that!

#### A simple stack

No self-hosted app is "easy" to deploy. But we did our best to make things simple. The Maybe app can be [hosted all inside a single Docker container](https://github.com/maybe-finance/maybe/blob/main/docs/hosting/docker.md) with _optional_ market data API dependencies.

For the demos of this write-up, I started a brand new self hosted app in less than 10 minutes!

### The losses

As we're stopping active development on this project, there were clearly some losses. Aside from the fact that growing a B2C SaaS app is challenging in its own right from a business perspective, I'll focus on some of the product/engineering losses that we feel could have been remediated with some more time and money to throw at this problem.

#### Data providers, data providers, data providers

The single biggest challenge with a personal finance app in 2025 is bank providers.

Some companies have had enough time to _work around_ this challenge with all sorts of clever UIs and mathematical shortcuts, but when push comes to shove, the state of "Open Banking" and bank provider data comes with endless frustration. While we believe we could have slowly but surely solved a _majority_ of these problems, we simply needed _more time and more money to do so_. Below are just a _few_ of the challenges we were still working through:

- Unsupported banks (there are a TON, and most users churn if even _one_ of their banks is unsupported)
- Banks that only support logins at certain times of day
- Bank provider documentation not matching the production data we received
- Bank provider data being plain _wrong_ (there is a surprisingly large amount of this)
- Idiosyncracies of each financial institution (everyone reports their data a little differently)
- The vast number of financial securities (i.e. stocks, options, etfs, etc.), many of which have differing data across market providers, and many of which have _sparse to zero_ data

Needless to say, this is a _massive_ challenge for anyone building a personal finance app and is the primary reason why "bootstrapping" a personal finance app with automated bank syncing is an uphill battle. You need a lot of money and time to get this right.

#### Data consistency and cache invalidation

While a dashboard with a net worth graph doesn't look all that complicated, it's one of those "iceberg" problems. The more you dig, the more complexity you find.

A personal finance app needs to show a user's total net worth trend over time. A personal finance app needs to show average spending. A personal finance needs to show correct balance sheet values.

Every view of the app touches nearly _all_ the user's data. If _any_ piece of data is _wrong_, every view in the app is wrong. There is nowhere to hide in a personal finance app, and even the slightest change to the _date_ of a historical transaction propagates upstream and affects the net worth graph, account sidebar trends, metrics, budgets, and pretty much every other view of the app!

The UI required by users demands a high degree of focus on both performance + accuracy. But in a finance app (or… any app!), these two things are at odds with each other. Achieving full accuracy can be done by writing "facts" to the database (event-sourcing) and _deriving_ results through queries and materialized views. Keep the data in its "raw" form and delegate the complexity to the _read_ side of things. Using this approach, you're _guaranteed_ updated data everywhere in the app.

Unfortunately, taking this approach with no answer for the performance side of things will result in an app that is 100% accurate and takes 3 minutes to load each page.

Having learned some lessons from our V1 approach to building this app, we opted for a "hybrid event-sourced" approach. We wrote "facts" (i.e. transactions, trades, valuations) to the database, "synced" them in background jobs, and wrote them to "cache tables" (i.e. `balances`, `holdings`). This made the _query side_ of things a lot simpler (still not simple though) and improved performance, but at the _cost_ of data consistency.

While I believe the final version of this app comes _close_ to full accuracy / consistency of data, there are still inevitable challenges here and something we wish we could have spent some more time on.

### The unexpected challenges

And finally, let's talk about a few of the challenges we didn't know we'd be facing.

#### OSS, financial privacy, and project management

As an open source project, we constantly received bug reports. Unfortunately (but understandably), many users are not comfortable sharing their personal financial situation.

This created an odd dynamic. We are open source, but in order to fix data bugs, we need to look at the data. If the user can't share all the required information, we can't reproduce the issue.

I'm not sure there's a great solution to this. It made the project management and bug fixing side of things extremely challenging. We wanted to manage all issues on the public repo, but by the nature of the app, we ended up having to track some issues internally and privately and others publicly.

It made things hard to see it all in one spot, and we didn't realize how much of a roadblock this would become for us.

#### Multi-currency + Investments Data

Building a multi-currency app is tough to begin with. We took on this challenge in the spirit of OSS and I believe we built a fairly comprehensive and solid solution.

That said, having global users exposed us to the global markets.

To say this is challenging is an understatement and then some. Let's start with some basic numbers (don't fact check me, this is largely coming from a quick ChatGPT session, but should be in the ballpark):

- There are ~180 currencies globally
- There are ~46k global exchanges, all of which have different securities and different prices
- There are ~55k publicly traded companies globally
- There are 120k+ mutual funds, ~10k ETFs
- Crypto… (we won't go there)

In order to show an _accurate_ investment portfolio graph, we need historical prices for _all_ the user's securities in their portfolio. Given the fact that there is no single data provider that supplies all these prices, many providers treat exchange codes differently, and market data is _extremely expensive_, this become one of our foremost challenges.

We ended up allowing users to create "manual" tickers which has worked fairly well. But the scope of this problem is huge and there's a reason entire applications are built to solely handle investment data (often US-based only).

### Closing thoughts

Given our tiny team and short time horizon, we're proud of what we've built here and the contributions we've made to OSS in the process. We've built the "engine" and we hope the OSS community can take this core engine and build some interesting use-cases on top of it.

With that, farewell! Maybe…
