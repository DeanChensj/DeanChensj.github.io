---
layout: post
title: "Beyond the Chat UI: Reviewing 1,000+ PRs and Running Dozens of Coding Agents Without Burning Out"
date: 2026-10-17 15:00:00 -0700
categories: [AI Engineering, Productivity]
cover: "/assets/blog/swe-hub-paradigm-shift.png"
excerpt_text: "When you run dozens of coding agents and ship stacked PRs in parallel, the bottleneck is no longer typing or reading speed—it is mental load. Here are three design choices behind SWE Hub that keep human attention focused on decisions."
reading_time: "4 min read"
---

Last quarter, working in a fast-moving stacked-PR culture, I reviewed **1,000+ code changes** while shipping my own code stacks and running **dozens of coding agents** in parallel—without working late or rubber-stamping.

People often ask how that volume is sustainable. The answer is counterintuitive: when you push daily software engineering to this level of concurrency, the first thing that breaks is the **Chat UI**.

---

### Why Chat Windows Break at High Concurrency

Anthropic’s engineering team recently published [*Solving Code Review at Anthropic*](https://claude.dev/blog/solving-code-review-at-anthropic/), and one observation stood out to me:

> *"Whether a human read every line is a rough proxy; the real question is what could go wrong with this change, and which layer of checks—automated or human—is best positioned to catch it."*

For a single task, a conversational chat window works fine. You prompt an agent, watch it stream tokens, and nudge it when it gets stuck.

But once you have dozens of agent sessions running in parallel, a chain of dependent branches in flight, and a steady stream of teammate reviews landing in your queue, **typing and reading speed are no longer the bottleneck—mental load is.** Every time you switch tabs to check whether an agent finished, remember which PR in a stack is blocked by its parent, or reconstruct context for a code review, you burn working memory.

<figure class="post-figure" data-lightbox-src="/assets/blog/swe-hub-paradigm-shift.png" data-lightbox-title="Before: Scattered Tabs (You Poll Everything) vs. After: SWE Hub (Only Decisions Surface)">
  <img src="/assets/blog/swe-hub-paradigm-shift.png" alt="Comparison between polling scattered Chat Tabs and SWE Hub's attention-filtered decision funnel" loading="lazy">
  <figcaption>
    <span>Why Chat Windows break at scale: shifting from manual tab-polling to an attention-filtered developer view</span>
    <span class="fig-zoom-hint"><svg viewBox="0 0 24 24" width="12" height="12" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><polyline points="15 3 21 3 21 9"></polyline><polyline points="9 21 3 21 3 15"></polyline><line x1="21" y1="3" x2="14" y2="10"></line><line x1="3" y1="21" x2="10" y2="14"></line></svg>Click to expand</span>
  </figcaption>
</figure>

To fix this for myself—and later for teammates running into the same wall—I built a lightweight internal hub (which I call **SWE Hub**) that brings parallel coding sessions, VCS and branch stacks, automated test recovery, and code review into a single view.

Building and using it every day taught me that scaling AI-assisted engineering isn't about handing the wheel to full-auto agents. It comes down to three concrete design choices:

---

### 1. Shrink the Trust Boundary

Many autonomous agents feel exhausting to use because their trust boundary is too wide. They try to make architectural trade-offs on your behalf, post noisy comments directly in public review threads, or burn retries in infinite loops on broken tests.

A calmer workflow draws a hard line between **mechanical execution** and **human judgment**:

* **Silent Background Recovery**: Routine work that requires zero human taste—pulling failure logs, fixing broken unit tests, formatting code, and addressing straightforward review comments—runs quietly in the background within a strict retry cap.
* **Private Drafts Only**: Agents never post comments or merge code directly under their own authority. They prepare private code diffs and draft replies in the background.
* **1-Line Decision Prompts**: The moment a change touches an architectural boundary, public API contract, or design trade-off, the agent stops and surfaces a single, plain-language decision card. AI prepares the draft; the engineer owns the final sign-off.

---

### 2. Hide Everything That Doesn't Need You

Writing code and reviewing code are two sides of the same coin, yet traditional tools split them across separate apps and browser tabs.

* **On the authoring side**, you shouldn't have to poll dozens of chat windows to see which agent is waiting for tool approval, or keep a mental graph of which branch in a stack needs rebasing. In SWE Hub, passing checks and healthy running agents stay out of sight. The interface acts as an attention filter—only surfacing items that are actively blocked on a human decision or ready to merge.
* **On the reviewing side**, instead of opening a cold diff and spending ten minutes piecing together context, a background agent groups related file changes, verifies behavior compatibility, and flags edge-case risks before you even open the change. That lets you spend 100% of your review energy on core architecture and design choices—which is how reviewing 1,000+ changes in a quarter becomes possible without cutting corners.

---

### 3. Remove Daily Friction

At high concurrency, tiny operational delays compound into major flow-breakers. Much of iterating on SWE Hub was simply noticing and removing those daily rough edges:

* **Automatic Branch Isolation**: Spinning up a new agent task automatically isolates its working branch and syncs the stack so parallel agents never step on each other's files.
* **One-Click Model Failover**: When a long-running agent session hits a rate limit or timeout, one click switches the underlying model and resumes the conversation in place without losing context.
* **One-Action Review Loop**: When a reviewer leaves feedback on your PR, a single action dispatches an agent to apply the code fix, verify the tests, and stage a draft reply for your review.

---

### Protect Human Attention

As AI makes writing code cheaper and faster, engineering teams are quickly discovering that **human attention is the true scarce resource**.

The next leap in developer productivity won't come from louder chatbots or unchecked full-auto agents. It will come from calmer tools that shrink the AI trust boundary, absorb mechanical noise in the background, and save human brainpower for the decisions that actually matter.

*Views and opinions expressed here are strictly my own.*
