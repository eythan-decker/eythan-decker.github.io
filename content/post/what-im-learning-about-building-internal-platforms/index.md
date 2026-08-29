---
title: "What I’m Learning About Building Internal Platforms"
description: "How product discovery, developer feedback, and early experimentation are shaping the way I think about internal platform engineering."
date: 2026-08-29
slug: "what-im-learning-about-building-internal-platforms"
image: cover.png
categories:
  - Platform Engineering
tags:
  - platform-engineering
  - product-management
  - developer-experience
draft: false
hidden: false
comments: true
---

I haven’t published anything since April.

It hasn’t been for lack of ideas. I have notes everywhere, most of them a few scattered paragraphs about something I think could become a technical post. Turning those thoughts into something coherent enough for a random person on the internet to read has been harder than capturing them in the first place. I have gained a lot of respect for those who are able to do this consistently.

Life outside work has been busy with family responsibilities and vacations over the summer. Work itself has been the bigger reason for the gap.

In June, Yum! Brands announced agreements to sell Pizza Hut. LongRange Capital would acquire the business outside Mainland China, while Yum China would acquire the Mainland China business through a separate transaction. At the time of writing, Yum China had completed its acquisition, while LongRange Capital’s purchase remained pending. [Yum! Brands transaction update](https://investors.yum.com/news-events/financial-releases/news-details/2026/Yum-Brands-Completes-Sale-of-Pizza-Hut-China-to-Yum-China-Holdings/default.aspx)

A lot of people have reached out to ask how I feel about the move. I understand the concern. There are plenty of ugly private equity stories, and I don’t know how this particular story will end.

From where I sit today, though, the work has been energizing. Preparing for the separation has led to some of the busiest and most fulfilling work of my career. Many of the people I work with are excited about the amount of opportunity in front of us.

As one of my peers put it, “With a lot of uncertainty comes a lot of opportunity.”

Separating a company brings its dependencies into focus. Systems and services that previously came from a parent organization have to be understood and, in many cases, migrated or replaced. Ownership becomes much more concrete when there is no longer another part of the enterprise to depend on.

A platform engineering mindset has helped us approach that work. We think of the engineering organization as our customer. We look for places where self-service tooling and automation can remove repeated work. We also need a reliable inventory of infrastructure, systems, and ownership so that people can find information without knowing who to ask.

All of that creates a lot to build. It also raises the question I have been thinking about most this summer: how do we decide which things are actually worth building?

## Why I picked up *Inspired*

I kept running across versions of the same argument in articles and conversations about agentic engineering.

Building software is getting cheaper. Prototypes that once took weeks can now take days or hours. That shifts more of the difficulty toward deciding what to build and getting people aligned around it.

The argument made sense to me. It also exposed a gap in my own experience.

I have spent years working in infrastructure, platform engineering, and engineering leadership. I knew much less about formal product discipline. Reading *Inspired* by Marty Cagan was my first deliberate step toward learning more about it.

Cagan is blunt about the product manager role. A good product manager needs a deep understanding of the users and the business, enough technical knowledge to work well with engineers, and the ability to make sense of data and stakeholder expectations. That is a difficult combination, especially for someone operating as an individual contributor.

Even a great product manager will be wrong regularly. Cagan argues that many product ideas will fail to create the expected value. Promising ideas also tend to require several rounds of testing and refinement. [The inconvenient truth about product](https://www.svpg.com/the-inconvenient-truth-about-product/)

I found that reassuring in a strange way. The goal is not to find the person with a magical ability to predict the right answer. You need people who can learn quickly enough to avoid spending months executing a bad assumption.

One phrase from the book stayed with me: fall in love with the problem, not the solution.

That is easy to repeat and much harder to practice. Engineers become attached to solutions because we spend so much time reasoning through them. Once the architecture makes sense and the implementation is taking shape, handing it to a user who may reject the whole approach can feel terrible.

That feedback is also far cheaper during a prototype than after a full rollout.

## Who is the platform for?

Most internal platforms serve a large part of the engineering organization. That needs to be stated and agreed upon.

If the platform team and its management answer that question differently, the roadmap will lose coherence. One group may be optimizing for developer enablement while another expects the team to operate mainly as a centralized infrastructure or compliance function. Both sets of work may be necessary, but the team needs clarity about the outcomes it owns.

On teams I have led, I have intentionally hired strong software engineers without traditional SRE or DevOps backgrounds. They could learn the cloud ecosystem while bringing experience from product engineering teams. That perspective helped us write better internal tooling and question some assumptions that seemed obvious to people who had spent years in infrastructure.

Over time, I noticed a problem with that approach. The longer someone works away from feature development, the harder it becomes to remember all the small frustrations involved in using the platform every day.

Eventually, platform engineers become experts in the platform team’s own experience.

We needed a way to keep hearing from everyone else.

## What we learned from a short survey

One experiment we ran was a survey of our internal engineering organization.

No one gets excited when another survey appears in their inbox, so we tried to keep it short. We asked how often people used the platform, how satisfied they were, whether self-service worked for them, and where they were losing time.

The open-ended questions were the most useful:

> What is the single biggest friction point in your platform experience right now?

> If we could fix or build one thing next quarter, what would it be?

We also asked whether teams had completed any recent AWS work outside the tools provided by the platform. Work that happens around the platform can tell us as much as work completed through it.

The responses surfaced several problems that were not on our radar. One was GitLab runner reliability.

We believed we had enough observability around the runners. The dashboards showed acceptable uptime, and we did not see an obvious reliability problem from the platform side.

The survey responses told a different story, so we started digging.

That investigation uncovered a timeout scenario we were not measuring. Our dashboards looked fine because our telemetry did not include the failure users were experiencing. We moved the work onto our third-quarter roadmap and addressed it quickly.

This was a useful correction for the team. We had been looking at the data available to us and drawing a reasonable conclusion from it. The survey gave us a reason to question whether the data represented the whole experience.

I don’t want to oversell surveys. Response rates can be low, the people who answer may have unusually strong opinions, and users often describe the solution they want instead of the underlying problem. We need to combine surveys with support conversations, interviews, usage data, and our operational telemetry.

For this particular issue, the survey did exactly what we needed. It pointed us toward a blind spot.

## Getting unfinished tools in front of people

The next challenge is getting feedback earlier.

Platform teams can borrow some simple habits from product engineering. Release notes are a good example. When an internal tool changes, the users should know what changed, why it changed, and where they can provide feedback.

Earlier access matters even more.

One of the hardest things for an engineer to do is hand a tool to someone while it still feels unfinished. There is always another error case to handle or another abstraction that needs work. The engineer knows every shortcut in the proof of concept and naturally wants to clean them up first.

There is a judgment call here. A broken prototype wastes people’s time and can damage trust in the platform team. Waiting six months to show anyone the supposedly finished product creates a different kind of waste.

We are trying to work between those two outcomes by identifying beta teams inside the engineering organization. Those teams know they are getting early software and that their feedback may change the direction of the work.

That gives us room to test ideas without pretending they are already products.

## Where the CLI and Backstage might fit

Two of the ideas we are exploring are a CLI for self-service cloud workflows and further development of our internal developer portal using Backstage.

I have a theory about how these tools may fit together.

With the rise of Claude Code, Codex, and other agentic engineering tools, I expect more infrastructure workflows to move toward the terminal. A well-designed CLI is useful to an engineer and relatively easy to incorporate into an agent workflow. It can scaffold infrastructure, provision resources, and return structured output that another tool can use.

Backstage serves a different set of needs well. It gives people a centralized place to browse technical documentation, find service ownership, and explore the organizational catalog. Those activities do not always translate naturally into CLI commands. Backstage also provides a more approachable interface for people who do not spend most of their day in a terminal. [Backstage documentation](https://backstage.io/docs/overview/what-is-backstage/)

Some of the same catalog and documentation context could eventually be exposed to agents through APIs or MCP. That still depends on having a well-maintained source of information underneath it.

My current guess is that the CLI will work well for scaffolding and multistep provisioning, while Backstage will remain useful for discovery, documentation, and the catalog. The beta teams may come back with a cleaner split. They may also ignore one of the tools or use it in a way we did not expect.

I would rather learn that by giving them real work than by asking which interface they prefer in the abstract.

## What I’m carrying forward

Before reading *Inspired*, I associated a product mindset with visible practices such as self-service tooling, roadmaps, release notes, and user surveys. I still think those practices are useful. This summer has made me pay more attention to the feedback loop underneath them.

Did the survey change what we worked on?

Can the beta teams influence how we divide responsibilities between the CLI and Backstage?

Will we stop investing in an idea if people struggle to use it, even when the implementation is technically strong?

Those are harder questions than whether we shipped something on time.

The separation work is still unfolding. The CLI and Backstage experiments are early, and I don’t know where we will land with either one. There is a good chance that at least one of my current theories will not survive contact with the beta teams.

That will probably be the most useful thing we learn, and maybe the subject of the next post.
