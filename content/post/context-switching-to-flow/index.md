---
title: "From Context Switching to Flow: My Claude Code + MCP Setup"
description: "How I use Claude Code, custom commands, plugins, and the Atlassian and Datadog MCP to minimize context switching and stay in flow state."
date: 2026-01-30
slug: "context-switching-to-flow"
image: cover.png
categories:
  - Platform Engineering
tags:
  - claude-code
  - AI
  - developer-experience
  - MCP
draft: false
---

## Flow is fragile, and context switching kills it

Something I don’t see talked about nearly enough, at least in my feed, is how much modern tooling actively works *against* flow.

Not just notifications. Not just Slack.

It’s the constant requirement to context switch. Between tools, tabs, systems, and mental models.

If you’re unfamiliar with the idea of *flow*, Mihaly Csikszentmihalyi’s original research describes it as the mental state where focus becomes effortless and time compresses. This overview does a good job summarizing it and why it matters:  
https://www.verywellmind.com/what-is-flow-2794768

Most engineers have experienced it.

You sit down to debug an issue. You look up and it’s suddenly 7pm. You missed dinner. You forgot to respond to texts. You weren’t distracted. You were *fully engaged*.

That state is where the best engineering happens.

The problem is that modern work environments are almost perfectly designed to prevent you from entering it. And if you do get there, they’re very good at pulling you back out.

---

## Deep work mattered before LLMs. It matters more now.

I’m going to reference Cal Newport’s *Deep Work* a few times here, because it’s still one of the clearest explanations of why sustained focus is a competitive advantage.

Here’s the book if you haven’t read it:  
https://www.amazon.com/Deep-Work-Focused-Success-Distracted/dp/1455586692

Newport published it in **2016**, long before large language models were mainstream.

His core argument was simple. People who can regularly do deep, uninterrupted work produce outsized value in modern knowledge economies.

What’s interesting is that this argument has aged *extremely* well.

If anything, it’s more relevant now.

From an engineering perspective, if a task can be easily reproduced by an LLM, then the value isn’t in typing faster or producing more artifacts. The value is in:
- Architectural judgment  
- Systems thinking  
- Problem decomposition  
- Knowing what *not* to do  
- Sitting with ambiguity long enough to resolve it  

There’s no shortcut for that. You still have to think.

What AI tools *can* do, though, is reduce the overhead that pulls you out of that thinking. That’s where tools like Claude Code start to matter.

---

## Engineers feel at home in the terminal for a reason

Claude Code is CLI-first. And that’s not an accident.

Most engineers are already comfortable there. The terminal is quiet. Focused. Predictable.

There’s something grounding about running a command and getting back clean, green output that says everything worked.

What we learned quickly after adopting agentic coding tools is that **context matters more than prompts**.

You can call it context engineering. Spec-driven development. Prompt hygiene. Whatever name you prefer, the underlying skill is the same.

Getting the context right *is* engineering.

The downside is that it takes work.

You have to understand the systems you’re working with end-to-end. You have to break problems into digestible pieces. You have to know where authoritative information lives and how to verify it.

That’s why there’s still a large gap in how engineers use AI tools. Many stop at “faster Stack Overflow.” Others treat the setup itself as an engineering problem.

Once you accept that framing, the next instinct kicks in naturally.

If this takes effort, it should be **repeatable, automated, and DRY**.

That’s where my Claude Code + MCP setup comes in.

---

## My setup: turning backlog work into a flow-friendly workflow

I maintain a small set of custom Claude Code commands for epics, stories, and bugs.

Each command maps to a structured markdown template and a specific execution path. I keep these at the global user level in my `claude.md`, so they’re available anywhere.

Repo or no repo. Fresh terminal or existing session.

From Claude’s perspective, it’s acting as an assistant. From my perspective, it’s a single entry point into work that normally spans Jira, Confluence, Datadog, and source code.

Below is the rough structure of how these commands are organized.
```
claude/
├── epic/
│   ├── example-1.md
│   └── example-2.md
├── story/
│   ├── example-1.md
│   └── example-2.md
└── bug/
    ├── example-1.md
    └── example-2.md

```

Each action assumes access to two MCP servers:

- Atlassian (Jira + Confluence)
- Datadog

Those assumptions are explicit. Nothing is inferred.

---
## Context bloat is real, and you have to design around it

You can’t just throw a mountain of context at an AI and say “figure it out.”

Context bloat is a real failure mode with MCPs, and you have to be intentional about how tools are invoked.

For Atlassian, each command specifies:
- Which Jira projects to search  
- Which backlog(s) are relevant  
- Which Confluence spaces or pages are authoritative  

For Datadog, my global configuration includes:
- Shared Kubernetes clusters  
- Primary AWS accounts  
- Namespaces my team owns  
- Frequently reused log queries  

This avoids Claude repeatedly issuing generic “list everything” calls and slowly narrowing scope through trial and error.

It *will* eventually get there without guidance.

But the time cost is real. And the cognitive interruption is worse.

The goal is to preserve momentum, not test patience.

---

## How this works in practice

The real value of this setup isn’t that it creates tickets faster. It’s that it lets me *stay in the same mental thread* from the moment a problem forms to the moment it’s captured as work.

That continuity is what protects flow.

When I’m planning my week or thinking through higher-level platform initiatives, I’m already operating in a reflective, systems-oriented headspace. Traditionally, that’s exactly the moment when tooling pulls me out of it.

I spend a lot of this time thinking in epics.

These sessions usually happen outside of a repo, often alongside my local Obsidian knowledge base. As I review project notes and think through platform enablement opportunities, I’ll invoke `/epic` with a short description of the outcome I’m aiming for.

By the end of the session, I have a Jira epic created from a standardized template. It includes:

- Desired outcomes  
- Technical context  
- Dependencies  
- Known constraints  

What matters here is not the artifact. It’s that nothing about my environment changes while I’m thinking.

I’m not switching tools. I’m not translating ideas into different interfaces. I’m not reloading context in my own head.

The thinking stays continuous, and the system adapts around it.

For epics, I don’t always need Datadog data. Unless the work is explicitly metrics-driven, I’ll opt into that context manually.

Breaking an epic into stories happens differently.

Stories are where flow usually breaks down.

This is the point where high-level intent meets low-level detail, and most tools force you to constantly rehydrate context.

This work typically happens inside a repo, while I’m already in the code. Context from the codebase matters here. I’ll invoke `/story`, pass in:

- The outcome I’m targeting  
- Relevant code context  
- Any Datadog signals I care about  
- The parent epic  

Because I’m already inside the codebase, this feels less like task creation and more like *thinking out loud with a scratchpad that happens to write tickets*.

That distinction matters. It keeps the cognitive load on the problem, not the process.

Before this setup, backlog work meant breaking my attention across half a dozen tools. Jira, Confluence, Datadog, Slack, and the code itself all competed for focus.

Now, that fragmentation is gone.

The work unfolds in a single thread, and that’s what makes it feel like engineering instead of administrative overhead.

Bugs follow a similar pattern.

They often arrive without context and at inconvenient times. The `/bug` command lets me capture them immediately, even if I’m not in the right repo or don’t yet know where the issue lives.

This is where the Datadog MCP gets used the most. Logs, metrics, and links are attached directly to the ticket.

Claude then decides where the bug belongs based on domain context. If nothing fits cleanly, it defaults to a quarterly bug backlog.

Is it perfect? Probably not.

Does it work for someone wearing multiple hats? So far, yes.

---

## What I’ve seen so far

The most noticeable change hasn’t been speed. It’s been *how the work feels while it’s happening*.

I spend less time reorienting myself and more time staying with problems long enough to actually understand them.

I don’t have clean quantitative metrics yet. The signals I pay attention to are indirect, but consistent. They suggest that more of the thinking is happening upfront, while context is still fresh.

So far, that’s shown up as:

- Fewer clarification questions from the team  
- Less rework on stories  
- Better alignment between desired outcomes and implementation  
- Less friction converting tickets into AI-assisted specs  

These are lagging indicators, not proof.

Longer term, I want clearer leading indicators. That’s still an open problem, and one I’m actively thinking about.

If you’re measuring something similar in your org, I’d genuinely love to hear how you’re doing it.

None of this removes the need for deep thinking. If anything, it makes that requirement more obvious.

The difference is that I now have a setup that protects those moments instead of fragmenting them.

---

## Try it yourself

Claude Code documentation (commands + plugins):  
https://code.claude.com/docs/en/plugins

Atlassian MCP:
`claude mcp add --transport sse atlassian [https://mcp.atlassian.com/v1/mcp](https://mcp.atlassian.com/v1/mcp)`

Datadog MCP (requires org access):
`claude mcp add --transport http datadog-mcp https://mcp.datadoghq.com/api/unstable/mcp-server/mcp

### Command examples

I’ve made the examples below generic and stripped of company-specific context. They should give you a feel for how you might tailor this setup to your own environment.

/Epic
```markdown
You are generating a Jira epic for the Platform Engineering team at {company}

Unless otherwise directed, assume you are generating this epic in the JIRA project {Porject Name} and project ID {id}
  
First, read the following files to understand the format and patterns:  

- Read `Jira Templates/style-guide.md` to understand tone, structure, and conventions
- Read `Jira Templates/Examples/{example1}.md` for epic example 1
- Read `Jira Templates/Examples/{exmaple2}.md` for epic example 2
  
Then, generate an epic based on the user's description following these guidelines:  
  
***Structure:**  
## Outcome  
[Clear statement of end state - what will be different when done]  
  
## Business Need  
[Business justification tied to {Company} observability, platform goals, or operational needs]  
  
## Technical Details  
[Implementation approaches, technical context, repositories, decision points]  
  
***Key characteristics for epics:**  
Broad scope (migrations, platform-wide changes)  
Discuss multiple implementation approaches when relevant  
Mention rollout strategy (all at once vs gradual)  
Reference specific repos, accounts, or clusters when known  
Tied to strategic platform goals (observability, developer experience, cost optimization)  
Cross-environment impact (dev, staging, prod)  
  
***Tone:**  
Technical and specific (use account IDs, ARNs, repo URLs when available)  
Business-justified (connect to {company} business value)  
Action-oriented and direct  
No fluff or superlatives  
  
***Platform context:**  
{company domain knowledge}
{links to company knowledge base, other MCP tools to get context} 
{company tech stack and where to refer to for more information on the stack}
If prompted, use the DataDog MCP to pull recent metrics on the platform piece you are creating an epic for.
  


Output the formatted epic description and title in the chat for review, if approved create the epic using Atalssian in the {Jira project} backlog


```

/story
```markdown
You are generating a Jira user story for the Platform Engineering team at {company}  
  
First, read the following files to understand the format and patterns:  

- Read `Jira Templates/style-guide.md` to understand tone, structure, and conventions
- Read `Jira Templates/Examples/example-1.md` for user story example

  
Then, generate a user story based on the user's description following these guidelines:  
  
***Structure:**  

## Outcome  
[What will be delivered - POC, research findings, working feature, new enhancement to a larger product]  
  
## Business Need  
[Developer experience improvement, bottleneck reduction, or capability enablement]  
  
## Technical Details  
[Specific implementation details, technical requirements, infrastructure changes needed]  

  
***Key characteristics for user stories:**  
More tactical than epics, focused on specific deliverables  
Sometimes exploratory ("research and potentially create a POC")  
May result in "clear set of new epics and a basic backlog or specs"  
Include "at minimum" requirements and "worst case" scenarios when applicable  
More granular scope than epics  
May include acceptance criteria if deliverables are concrete  
  
***Tone:**  
Technical and specific (use account IDs, ARNs, repo URLs when available)  
Business-justified (connect to developer productivity or platform maturity)  
Action-oriented and direct  
No fluff or superlatives  
  
***Platform context:**  
{company domain knowledge}
{links to company knowledge base, other MCP tools to get context} 
{company tech stack and where to refer to for more information on the stack}
If prompted, use the DataDog MCP to pull recent metrics on the platform piece you are creating an story for.
  
***Useful patterns:**  
"At minimum, [base requirement]"  
"If possible, leverage [tool] for [purpose]"  
"Worst case: implement [fallback approach]"  
"The spike should result in a clear set of new epics"  
  
Output the formatted story description and title in the chat for review, if approved create the story using Atalssian in the {Jira project} backlog. IF given an epic id, attach the story as a child to that epic. If given no epics, use atlassian to pull epics on the {backlog ID} and provide the user of a list of which epics you would recommend attaching this story to.
```

/bug
```markdown
You are generating a Jira bug ticket for the Platform Engineering team at {company}  
  
First, read the following files to understand the format and patterns:  

- Read `Jira Templates/style-guide.md` to understand tone, structure, and conventions
- Read `Jira Templates/Examples/bug-example.md` for bug example

  
  
Then, generate a bug ticket based on the user's description following these guidelines:  
  
**Structure:**  

## Summary  
[One-line description of the bug fix]  
  
## Issue Type  
Bug  
  
## Priority  
[High/Medium/Low based on impact]  
  
## Environment  
[List all affected environments:  

- Dev cluster/environment
- Staging cluster/environment
- Production cluster/environment]

## Description  
[Brief overview of the failure]  
  
### Error Details  
[Full error message, stack trace, or log output with request IDs if available]  
  
### Root Cause  
[Technical explanation of why the error is occurring]  
  
### Required Action  
[Specific changes needed to resolve, including IAM policies, permissions, configurations, code bases potentially involved.]  
  
## Acceptance Criteria  

- [ ] Error resolved in dev environment
- [ ] Error resolved in staging environment
- [ ] Error resolved in production environment
- [ ] No error logs present after fix
- [ ] Resources show healthy status
- [ ] Changes documented in IaC (CDK/Terraform)
- [ ] Validation testing completed

## Technical Notes  
**Affected Components:**  
[List components involved]  
  
**Environments:**  
[Reiterate environments if needed for clarity]  
  
**Related AWS Accounts:**  
[Account IDs and their purpose, especially for cross-account issues]  
  
## Labels  
[e.g., platform-engineering, infrastructure, k8s, istio, iam, aws, security, platform]  
  
**Key characteristics for bugs:**  

- Complete error messages with request IDs
- Root cause analysis included
- Multi-environment scope (dev, staging, prod)
- Specific IAM roles, ARNs, and account IDs
- Acceptance criteria tied to error resolution across all environments
- Cross-account considerations when applicable
- Technical notes section with affected components
- Using Datadog MCP, metrics and recent logs for the workload if given a workload name or app name

**Tone:**  

- Technical and specific (include exact error messages, ARNs, account IDs)
- Root cause focused
- Action-oriented with clear resolution steps
- No speculation - state facts about the error
  
**Platform context:**  

- Team supports {domain knowledge}
- Infrastructure: AWS, {domain knowledge}
- Common issues: {domain knowledge}

Output the formatted bug description and title in the chat for review, if approved create the story using Atalssian in the {Jira project} backlog. If given an epic id, attach the bug as a child to that epic. If given no epics, use atlassian to pull epics on the {backlog ID} and provide the user of a list of which epics you would recommend attaching this story to OR give the user the "adhoc / Dev Support bucket" epic for the current quarter as a default option.