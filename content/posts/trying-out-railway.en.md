+++
title = "Trying out Railway - is it really good for backend systems?"
description = "Initial thoughts on Railway platform after 2+ weeks of use"
tags = [
    "railway",
    "deployment",
    "backend",
    "devops",
    "platform"
]
date = "2025-01-25"
categories = [
    "development"
]
author = "@__biancarosa"
substack_url = "https://backendengineeringadventures.substack.com/p/trying-out-railway-is-it-really-good"
+++

*This post was originally published on my [Substack - Backend Engineering Adventures](https://backendengineeringadventures.substack.com/p/trying-out-railway-is-it-really-good).*

[Railway](http://railway.app) has been gaining attention as a developer-friendly solution in this evolving landscape of cloud deployment platforms. I recently took it for a test drive with a new project, and I have to say it – it felt like heaven.

## The Project Setup

For this experiment, I deployed [findash](https://findash.platformlabs.co), an open-source tool I'm experimenting with available on [GitHub](https://github.com/platformlabs-co/findash). The stack is simple, for now:

- A PostgreSQL database
- A Python API service
- Docker containerization

[![](https://substackcdn.com/image/fetch/$s_!LlEj!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5c792d94-3869-43dc-b823-d633b2cb3b42_1568x561.webp)](https://substackcdn.com/image/fetch/$s_!LlEj!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5c792d94-3869-43dc-b823-d633b2cb3b42_1568x561.webp)

## The "No Pipeline" Pipeline

Here's where Railway really shines – and might make your release engineering team raise an eyebrow (in a good way, at least I raised mine). The deployment process was refreshingly simple:

1. Point Railway to the GitHub repository
2. Watch it automatically detect and build the Dockerfile
3. ...that's it. No, really.

No YAML files to configure, no pipeline definitions to debug, no hair-pulling moments trying to figure out why your deployment failed for the fifth time. Railway just... works (well, sometimes it doesn't, but in my case, the problem was within my Dockerfile and I hadn't tested it locally before deploying… don't do that at work).

Plus, it easily supports cron-based dockerfiles. Just give it a cron rule and a project and it'll run your container whenever you need to.

[![](https://substackcdn.com/image/fetch/$s_!UaR7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faff4ccb0-dbf0-4c79-a466-b6e3f9b587bc_1176x1008.webp)](https://substackcdn.com/image/fetch/$s_!UaR7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faff4ccb0-dbf0-4c79-a466-b6e3f9b587bc_1176x1008.webp)

## Database Setup: Postgres in Paradise

Adding a PostgreSQL database to the mix was equally painless. Railway's database provisioning is incredible. It provided me:

- Automatic database provisioning
- Secure credential management

[![](https://substackcdn.com/image/fetch/$s_!jkdb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3d88b0f7-37ea-4e5e-9702-7d7e8e060dfc_1102x868.webp)](https://substackcdn.com/image/fetch/$s_!jkdb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3d88b0f7-37ea-4e5e-9702-7d7e8e060dfc_1102x868.webp)

## Internal Networking

One of the most impressive features is Railway's approach to internal networking and variable sharing. Services can communicate securely out of the box, and environment variables are managed seamlessly across your stack.

[![](https://substackcdn.com/image/fetch/$s_!qxI-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4493f514-b9a4-49dc-89bd-bb053ded6bdb_1519x784.webp)](https://substackcdn.com/image/fetch/$s_!qxI-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4493f514-b9a4-49dc-89bd-bb053ded6bdb_1519x784.webp)

## App Sleeping

The app sleeping feature is particularly clever. It's like having an environmentally conscious infrastructure that knows when to take a power nap. The restart policy ensures your services wake up fresh when needed, making it perfect for:

- Development environments
- Low-traffic applications
- Prototypes
- Internal applications
- Cost-optimized deployments

[![](https://substackcdn.com/image/fetch/$s_!Tp1G!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff35ceb65-4bf7-4089-a2c6-9669e90ec9af_1568x373.webp)](https://substackcdn.com/image/fetch/$s_!Tp1G!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff35ceb65-4bf7-4089-a2c6-9669e90ec9af_1568x373.webp)

## Templates are incredible

I've tried templates for two things:

- N8N: <https://railway.com/template/0vH6fh>
- Grafana: <https://railway.com/template/0vH6fh>

And I've never been happier with how a tool handles this.

My Grafana instance could easily connect to my database, my N8N services came with their own database, load balancer, and everything needed, without me having to sweat about it.

## **Pricing**

I'm pretty sure Railway can get pricey if running multiple workloads. It is also a tool with a particular niche, so cost optimization might not be as easy as it is in AWS.

But for my playground stack, I'm not even reaching my 5 USD credits that I paid for the hobby plan (because I wanted a custom domain). So it is 100% worthy it.

## Things I haven't tried it with

While my experience has been overwhelmingly positive, it's worth noting that I haven't tested Railway with:

- Complex CI/CD requirements: You know those CI projects where you have to generate OpenAPI specs, where you need to do /something/ after a deployment like trigger other automation etc? Haven't tried those. I think I would build something webhook-based with Railway. Listening to deployment events over orchestration. Gotta try it before saying it works, though!
- Custom build pipelines: All my deploys were point-and-click setups, where Railway handles docker builds and deploy. I have not added tests to a deployment pipeline - at maximum, I've told Railway to "wait for tests” on GitHub actions to start running the build & deploy.
- Heavy-load production scenarios: It seems like it will handle things well if you do a good job at modularizing your app, but I need to see it working to be confident.
- Config-as-a-code: important for non-prototype projects, for sure. I want to explore this more before even beginning to recommend this for a serious project.

## The Verdict

Railway has managed to strike that sweet spot between simplicity and capability. It's not trying to be the next AWS – instead, it's like the friendly neighborhood infrastructure service that just wants to help you deploy your apps without the usual drama.

For backend systems, especially those in the development phase or with straightforward deployment needs, Railway is more than good – it's excellent. The platform takes care of the infrastructure complexity while letting developers focus on what they do best: building features.

Is it perfect for every use case? No. But for teams looking to move fast and without complex needs in terms of infrastructure.

I'll be moving my last.fm last played API (github.com/biancarosa/lastfm-last-played) to Railway shortly, and I'll report back if I have insights!

And if you haven't tried Railway yet, I highly recommend you to [go try it out](https://railway.com?referralCode=sWx4lg)!

*Have you tried Railway for your projects? I'd love to hear about your experiences in the comments below. And if you like what I posted so far, please subscribe to receive future posts ;)*
