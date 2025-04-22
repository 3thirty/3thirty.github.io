---
layout: post
title:  "Productionizing Strava charts"
date:   2025-04-20 00:00:00 -0600
categories: 
---
I've been a Strava user for almost 15 years and have logged a lot of data there – something like 47,000 km across about 2,000 rides. Strava’s analytics are great for digging into a single ride, but I wanted to zoom out—see trends over time, track year-by-year totals, and watch how my average speed and power have changed.

So, during the pandemic, I built a [Python app to do just that](https://www.github.com/3thirty/strava_charts). It uses:

- [Bottle](https://bottlepy.org/docs/dev/) for handling web requests  
- [Gunicorn](https://gunicorn.org) as the dev web server  
- [Requests](https://docs.python-requests.org/en/latest/index.html) for making calls to the Strava API (including OAuth)  
- [pyChart.JS](https://github.com/IridiumIO/pyChart.JS) for rendering the charts  

The first version was a simple, local-only tool that did exactly what I needed. It was a great experience building something from the ground up using the building blocks of well-designed libraries.

I’d always meant to make the app public but never got around to it—until recently. I decided to **productionize** the app as a learning exercise to get hands-on with some newer AWS tools. Here's what I used:

- [OrbStack](https://www.orbstack.dev) – to build and run Docker images for a clean dev environment  
- [GitHub Actions](https://docs.github.com/en/actions) – to automate Docker builds and deployment  
- [AWS SAM](https://aws.amazon.com/serverless/sam/) – to orchestrate AWS services like ECR, Route53, API Gateway, and Lambda  

This required a few code tweaks—handling Lambda event input and fixing some `requests` caching issues on a read-only filesystem—but most of the effort (and slow trial-and-error) was in setting up the CI/CD pipeline.

Along the way, I picked up a few things:

- **ChatGPT** turned out to be a really helpful tool for ramping up quickly on new tech and syntax—it saved me a ton of time getting the initial AWS SAM and GitHub Actions configurations set up. That said, it tends to get confused when troubleshooting. Being able to clearly frame a specific question made a big difference.  
- **AWS SAM** seems really powerful, but it still feels a little magical to me, probably because I didn’t run into enough issues to force me to look under the hood.

The end result: a fairly streamlined build-and-deploy flow, and a live version of the app now running at [https://stravacharts.3thirty.space](https://stravacharts.3thirty.space)\*. The full source is available for anyone who wants to run it themselves (or using their own API key)

\* Strava login support for users other than me is still pending approval 🤞
