---
title: "From the Cockpit: Orchestration is the New Leverage"
description: "A reflection on building a flight sim MCP server with AI agents, and how shifting from writing code to orchestrating systems changes where engineering leverage lives"
date: 2026-04-23
image: cover.jpg
draft: false
categories:
  - AI for Engineering Teams
tags:
  - mcp
  - ai
  - flight-simulation
hidden: false
comments: true
---
# Orchestration is the new leverage

This is the first real project I've built where I wrote almost none of the code myself. Maybe 5% of it, if that.

The project is an MCP server that lets an LLM interact with a running Microsoft Flight Simulator session. That part turned out to be less of a leap than I expected. The flight sim ecosystem already has hundreds of third-party integrations, so exposing that surface area to an agent was a natural fit. What surprised me wasn't the output. It was how the work got done.

Along the way I forced myself to learn subagents, experimented with [Agent Teams](https://code.claude.com/docs/en/agent-teams), and started to understand how this style of engineering actually works in practice.

This wasn't less engineering. It was different engineering.

## The ceiling I kept hitting

I've been using Claude Code daily since last September, both at work and for side projects. Like most engineers starting with AI coding tools, I started as a beginner. I treated it like a better ChatGPT for whatever bug or task I was working on. Over time I trusted it with larger surfaces, especially around DevOps and platform tooling (that's probably its own post).

But the workflow itself never really changed. Everything stayed synchronous. I was still writing about half the code by hand, still thinking in single-threaded execution, still using Claude as a very capable specialist tool rather than as a team.

Eventually, I plateaued.

The blogs I respected kept pointing at the same next step: delegate to subagents, orchestrate teams of agents. As I started sketching this flight sim project, I decided to use it as a forcing function. No more synchronous coding. I was going to be the architect of an agent team, not the author of every line.

That raised a new question I didn't have a great answer to. Everyone says your AI results are only as good as your prompt. But how do you _prompt_ a system design? How do you prompt a backlog? How do you prompt a team?

As it turns out, good system design and an engineering mindset are still the core of the craft. I did the same things I'd do on any project. Sketched the high-level design. Worked through non-functional requirements. Thought about edge cases up front. Spent real time with the [SimConnect documentation](https://docs.flightsimulator.com/html/Programming_Tools/SimConnect/SimConnect.htm) so I understood the integration I was about to delegate.

That part didn't change. What changed was where my hands actually went next.

## Agent teams felt powerful, until they didn't

Right as I started this project, Anthropic released an experimental feature called [Agent Teams](https://code.claude.com/docs/en/agent-teams). It felt like the perfect chance to jump into AI-native engineering head first. I paired it with a pattern I'd been using at work for a few weeks, [spec-driven development](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/). Define the system clearly, then let implementation follow.

My first prompt was something like:

```
Today we are designing the spec for "flight-sim-mcp". Review @notes @design 
@edgecases with an agent team. Have the team work together on a spec document 
and design for this project, with an agent focused on each file. Have each 
agent ask questions about the proposed design and go back and forth with me.
```

At first, it looked promising. Claude spun up three agents, they did some analysis, asked a few questions. Then the session limit hit. About 15 minutes in, I was done for the day.

Anthropic flags this directly in the docs. Without tight direction on how many agents you want, what they specialize in, and how they should collaborate, Claude makes those calls for you, usually at the expense of your token budget. Parallelism without structure is just expensive noise.

I kept at it because at work I have a lot more headroom on tokens, and agent teams turned out to be a great fit for one of the [use cases Anthropic actually recommends](https://code.claude.com/docs/en/agent-teams#when-to-use-agent-teams): research and review. I started using them for approach comparisons, asking a team to debate different solutions and argue for their preferred one. Entertaining, and genuinely useful for pressure-testing a design.

Back on flight-sim-mcp, after enough trial and error, I landed on a team that actually worked for the spec phase:

- A **SimConnect expert**, doing the research and becoming the source of truth on the integration.
- A **Go systems architect**.
- A **Quality Engineer** focused on the testing strategy.
- A **Product Owner** acting as the team lead, breaking ties and keeping everyone aligned to the outcomes in my notes.

The delegation lesson I pulled out of this phase: the prompt has to carry the structure. Clear outcomes and metrics. Exact agent count and specialties. Specific tools each agent can reach for. Who breaks ties. The more detail, the better the team actually behaves.

The original spec [still lives in the repo](https://github.com/eythan-decker/flightsim-mcp/blob/main/spec.md) if you want to see what came out of it.

One side benefit I didn't expect: while the team ran, I could do something else. Fold laundry, answer Slack, work on a different problem. In hindsight I probably should have used that time to throw edge cases at them for parallel consideration. Still learning that muscle.

## The real problem was statelessness

The token burn wasn't actually what broke the workflow. Context was.

Like most side projects, this one lived in short, inconsistent sessions. Every time I came back, I had to rebuild the team. The roles, the goals, the reasoning behind earlier decisions. You can `/resume` a Claude session, but eventually it gets compacted and the _why_ starts to disappear. I had a "project lead" agent keeping a running status file, which helped, but I was still retyping the same lengthy team-setup prompt over and over.

At that point the engineering instinct kicked in.

This wasn't DRY.

## Why I moved to subagents

[Subagents](https://code.claude.com/docs/en/agent-teams#compare-with-subagents) solved a different problem. Instead of redefining a team every session, I could define each role once as a markdown file in `.claude/`, version it in source control, and reuse it across sessions. That also meant the agents I built were shareable, which turned out to matter at work more than I expected. I've already reused some of these role definitions on completely different projects on my team.

The tradeoffs compared to agent teams are worth being specific about:

- **Communication.** Teams can talk to each other. Subagents only report back to the main agent.
- **Focus.** Subagent files let me specify inputs, outputs, allowed tools, and things the agent is forbidden from doing. That specificity is hard to replicate in a prompt every time.
- **Cost.** Agent teams without tight context engineering can burn tokens fast. Subagents are much more predictable.

For the implementation phase I settled on four subagents: a Go engineer, a DevOps agent, a test writer, and a product owner. My prompts changed immediately. Instead of re-explaining how the team should operate, I could focus on what needed to be done:

```
Today we are working on SIGTERM handling for the MCP server running in a 
container. Review the relevant section of the spec. Delegate test writing 
to the Go test writer agent per the TDD philosophy in the spec. Use the 
Go architect and engineer for implementation.
```

The thing I didn't appreciate until later: agent teams can (and should) pull in your project's subagents. The two aren't opposing choices. The right split, at least for me, has been using agent teams when I'm exploring and want parallel perspectives, and subagents for sustained work. Often both together, with the team operating through predefined subagent roles.

## It still felt like real engineering

The workflow shifted. The work didn't get easier.

With a solid spec and TDD baked in, 80% of the initial project came together fast. That's not an AI thing, that's a good-fundamentals thing. Small commits, enforced lint and formatting rules, tests that can't be skipped. Claude performs well when it's getting clear signals and rapid feedback, and those practices produce exactly that. Opus 4.5 onward, through 4.6 and now 4.7, Claude genuinely matches or exceeds what I'd write by hand under those constraints.

The remaining 20% was integration, which is where real engineering usually lives. I was still reviewing outputs, running tests, chasing edge cases in the SimConnect binary parsing.

The moment that shifted my trust happened late in the project. Going into a full end-to-end test with a running MSFS session on my PC, Claude noticed the implementation was built against an older version of the SimConnect API. There was a newer one that would simplify the integration. A mistake that originated in _my_ spec, not in its output. It proposed the refactor, made the changes, ran the tests, and picked up exactly where we'd left off.

That wasn't impressive because it was perfect. It was impressive because it recovered.

We tend to put engineers on a pedestal and assume we don't make mistakes. We make them constantly. Why would I expect an agent working from my incomplete requirements to never make one? What I actually want is an agent that catches them, including mine, and keeps going. That's what happened here.

## A quick detour: MCP as forced platform thinking

One thing I've appreciated watching MCP spread across the industry is that it's quietly pushing teams toward platform-as-a-service interaction models. Expose your capability as a clean, machine-consumable interface. Let other systems consume it without needing tribal knowledge or a meeting.

This is the thing I care about most in platform engineering. Coordination tax and information gathering are huge cognitive drains on teams. [Bezos famously mandated this at Amazon](https://konghq.com/blog/enterprise/api-mandate) in its early days. MCP is the same idea, applied through the LLM as the consumer. The adoption across tools like Jira and Atlassian, which I touched on in my last post, is going to be a significant unlock for how teams actually work.

Flight-sim-mcp is a tiny example of that pattern. A flight simulator exposing its state through a well-defined interface that anything can consume.

## What's next

The current version is read-only. It can inspect the state of a flight in real time: runway, airport, weather, basic telemetry. Seeing that work end-to-end through an MCP integration was genuinely cool.

The repo is here: [github.com/eythan-decker/flightsim-mcp](https://github.com/eythan-decker/flightsim-mcp). I'm also running it in my [homelab](https://github.com/eythan-decker/homelab), and I'd be curious to see if anyone else can get it running locally or in their own K8s setup.

Is it useful for most simmers? Probably not. Several of the big third-party MSFS tools are already baking AI in. This is more of a power-user playground than a finished product.

The natural next step is write actions: letting the agent actually control the sim. A "copilot" persona running a checklist with you, or making autopilot changes dynamically. That opens up a different, more interesting class of problems, especially around safety and confirmation flows.

## How to level up if you want this kind of leverage

If you want to get the 10x results engineers keep talking about with these tools, a few things have worked for me:

1. **Get comfortable with [spec-driven development](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/).** Use your engineering skillset to plan the system at a high level before writing any prompts.
2. **Make TDD non-negotiable.** This should have been table stakes before AI. Now it's mandatory. Set a coverage standard, put it in your project's context, tell Claude new code has to meet it. [Learn Go with Tests](https://quii.gitbook.io/learn-go-with-tests) is still one of the best primers I know of, and the pattern applies in any language.
3. **Lint everything.** If you find yourself correcting style or patterns repeatedly, that's a linter's job, not yours. Add it to the build and to the project context. For Go, [golangci-lint](https://github.com/golangci/golangci-lint) is the standard.
4. **Treat your `CLAUDE.md` like a living document.** Context engineering is a skill. A useful trick: after a good session, ask Claude `Based on the work we just did, what recommendations would you make to update the project's CLAUDE.md?`
5. **Delegate by default.** Once the foundations are in place, treat every problem as something a team should handle. Start with agent teams when you're exploring. Move to subagents for sustained work and check them into source control.

## Closing

This project didn't make me a better coder. It changed where the work happens.

Less time writing code. More time defining systems, shaping constraints, directing execution. The engineering didn't go away. It moved up a layer.

DHH said something on [a recent Pragmatic Engineer podcast](https://www.youtube.com/watch?v=JiWgKRgdgpI) that stuck with me. This isn't project management for agents, even if delegation skills help. Coding with AI tools, especially for a strong engineer, is more like suiting up in a mech suit. The 10x engineer becomes a 50x or 100x engineer. The backlog you thought you'd never get to becomes an afternoon's work.

That's what I'm starting to feel at work. Projects and refactors that used to be unjustifiable on opportunity cost are suddenly small. The feedback loop of knowing you're building something that works, faster than ever before, is genuinely addicting.

That's where the leverage is starting to show up.