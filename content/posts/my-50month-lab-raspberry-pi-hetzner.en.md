+++
title = "My <$50/month lab: Raspberry Pi, Hetzner, Home Assistant, k3s, Flux and a bunch of things I now have to maintain"
description = "Like I don’t already have enough work at my day job :D"
date = "2026-09-12T21:24:13+00:00"
author = "@__biancarosa"
tags = ["iot", "homelab", "infrastructure"]
categories = ["development"]
substack_url = "https://backendengineeringadventures.substack.com/p/my-50month-lab-raspberry-pi-hetzner"
+++

*This post was originally published on my [Substack - Backend Engineering Adventures](https://backendengineeringadventures.substack.com/p/my-50month-lab-raspberry-pi-hetzner).*

It's been a while since I posted here but here I am again, trying to share about what I've been up to lately.

Sooooo… I recently moved. We finished building our house, and I am now oficially living in the middle of nowhere, far from big cities, and really enjoying this new life. So this all started with the fact that I wanted to run Home Assistant in my RaspberryPi, and kept expanding whenever I wanted to try something new from the AI world.

So this is my current playground: I’m running Home Assistant, giving a try at building custom IoT devices, and hosting private pet projects on a Raspberry Pi and a Hetzner machine. My recurring infrastructure spend for this is about US$50/month.

That number doesn’t include the hardware, electricity at home, or my time (especially my time!!). But it gives me somewhere to run useful things and work through everything around the application code: deployment, networking, telemetry, and keeping the whole thing alive.

And this how I’ve put it together!

[![](https://substackcdn.com/image/fetch/$s_!rHhQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F25329285-29b9-4e91-ac01-c6cd1219167c_3600x1506.png)](https://substackcdn.com/image/fetch/$s_!rHhQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F25329285-29b9-4e91-ac01-c6cd1219167c_3600x1506.png)

**The stuff that lives at home**

The Raspberry Pi runs Home Assistant and the services around local device integration. I manage it with Docker Compose: service configuration, persistent storage, and updates. I did have an issue where a power outage corrupted my matter devices setup in HA, so I now also have a little backup to another machine (the Hetzner lab) every night that runs through rsync.

A lot of this config was done manually but now I'm porting things to Ansible in case I need to set this up in another machine or suddenly drop a coffee mug on the little PI.

I have a Zigbee dongle, a few Zigbee devices, a few Matter devices (although still using Matter over-wifi, I plan on having a Matter hub soon), a few smart devices at home and a Home Assistant setup that I'm slowly convincing my wife it's safe and smooth. So far just useful to turn lights off when already in bed, or to turn on the A/C without finding the remote but this is gonna become a lil monster soon enough.

I am also now starting (just starting) to play around with IoT devices. Let's see if I can make my own presence sensor. I will update you all on this one day!

**Pet projects go to Hetzner**

The Hetzner machine runs k3s. That’s where I have Kubernetes for application workloads and the infrastructure around them.

I describe deployments with Helm releases and Kustomize, and Traefik handles ingress. Together, those give me a repeatable way to take an application container and turn it into a running service. The whole k3s setup was done through Ansible.

There’s more machinery here than on the Pi, and I’m choosing to operate it. Part of the value of this homelab is having somewhere to practice the operational side of backend engineering, with projects I actually want to run.

Railway is still my cloud of choice when I do want something in the cloud - project that are less pet projects like my blog (biancarosa.com.br) - or when it's a pet project actively used by me or others because I don't yet trust my remote homelab infra :D

**Git is where I change things**

Flux CD connects the repository to the cluster. My deployment workflow looks like this:

1. Change Kubernetes manifests or Helm values in Git.

2. Review and merge the change.

3. Let Flux detect the revision and reconcile the cluster toward that configuration.

The repository separates cluster bootstrap, shared infrastructure, observability, and application workloads. That gives the different parts of the setup a place to live, instead of one growing pile of deployment files.

What I like about this is being able to inspect a change. I can read the diff, see the previous configuration, and revert it through Git. Flux also reconciles drift for the resources it manages.

But a Git revert has limits. If a deployment includes a database migration or changes persistent data, restoring the old YAML doesn’t restore the old data. I still have to think about how that change can be recovered (Git history is doing enough work already).

**Getting to my services**

I use Tailscale for private connectivity and administrative access across the machines. Private tools can be reached through the tailnet without giving every dashboard a public endpoint.

Traefik handles routing to services. Tailscale provides private connectivity, and access controls determine who should be able to reach what. Those are separate parts of the setup, even when they’re involved in the same request.

For private pet-project infrastructure, this is useful: I can reach the tools I run without needing to make each one publicly accessible.

**And yes, I have an observability stack**

On the remote machine, I run Prometheus, Grafana, and Alertmanager for metrics, dashboards, and alerting. Loki handles centralized logs, Tempo handles distributed traces, and OpenTelemetry Collector and Grafana Alloy handle telemetry collection.

That’s quite a few services for a pet-project setup. Apparently, giving myself software to maintain wasn’t enough; I also needed software to observe the software I maintain.

But this is a part I want to practice. Investigating a failed request, understanding resource usage, and connecting application behavior to infrastructure are all backend engineering work. Having somewhere to do that with my own services is useful.

It also means observability is part of the infrastructure I operate. Those services need resources and maintenance too.

They were running in Railway up until two months ago, but it wasn't worthy to pay for Railway infra for a centralized service like that is that is not critical because only my projects use it.

**A lot of room for growth**

About US$50/month buys me recurring infrastructure for this setup. Hardware, home electricity, and my time are accounted for separately, so this is my infrastructure budget, not the total cost of owning and operating a (half-remote) homelab.

I’m also responsible for patching, disk capacity, backups, and recovery. And the remote side is still one Hetzner machine. If that machine goes down, the workloads on it go down together. Kubernetes doesn’t change that failure domain.

For my pet projects, I’m happy with that tradeoff. I get useful services, somewhere to experiment with devices, and real deployment and debugging work on a modest recurring bill.

There’s plenty I can keep evolving here. For now, I have a place to build things all the way from firmware to a running service—and then figure out why they aren’t behaving the way I expected :)

If you’re running a homelab too, I’d love to hear what you keep at home and what you run remotely. Drop a comment below!
