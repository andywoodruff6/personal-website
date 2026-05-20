---
title: "What Does a Product Owner Look Like in the Age of AI?"
date: "2026-05-20T13:34:14.849Z"
draft: false
description: "In the AI age, the Product Owner is the king of context — and the king of context doesn't just collect, they govern."
tags: ['AI', 'product', 'context', 'work']
---

![Product Owner as the king of context](/images/shifting-current-product-owner-context-crown-20260520.png)

**What does a Product Owner look like in the age of AI?**

In January, I was building business requirements documents for OCME — defining how the site should work, documenting what decisions we'd already made, and why. I gave our development team access to the full document set and connected it to their AI systems.

Developers stopped coming to me with clarifying questions. Not because they stopped caring — but because the AI could answer them directly from the documentation. Bugs got fixed more accurately. Decisions got made faster. The context that used to live in my brain was now a queryable resource the whole team could access.

That's the unlock.

---

There are four primitives that determine how effective an AI system is: the **model**, the **context**, the **prompt**, and the **tools**.

The model is owned by whoever built it — a frontier lab, an open source project, a vendor you chose. The prompt belongs to the individual. A developer writes a different prompt than a marketer. Tools are configured by whoever has the technical access to connect them.

Context is different.

Context is the one primitive shared across every role. Your dev team, your marketing team, your executive team — they all bring their own model, prompt, and tools. But they're all pulling from the same pool: the objectives, the constraints, the decisions, the history of the product.

Someone has to own that pool. That person is the Product Owner.

Most POs are still running the old model.

A developer pushes back on a proposed approach — suggests a better one — you agree. The better approach wins. The code gets implemented, but no one writes it down. Six months later, an AI recommends the original approach again because it never knew you'd already ruled it out.

A customer files a complaint. You DM the developer. It gets fixed. But the context behind the fix — the pattern it reveals — never gets captured.

You decide not to work with a particular vendor. The reason is legitimate. It lives in your head. A year from now, an AI recommends that vendor confidently. Your team spends two weeks on an integration. Management kills it the moment they find out.

This isn't a process failure. It's a context failure.

---

At OCME, business requirements live in a GitHub repository.

`main` is the approved branch — what the whole team references when they need to know how something is supposed to work. `pending` is where changes get staged. New feature? Write up the business requirements, push to pending, let stakeholders review. When it's approved, it merges to main. Affected teams get notified.

QA becomes a context reconciliation exercise. I compare what's live on the server to what the business requirements say should be there. Discrepancies become tickets. When production outpaces the spec — our dev team gave us 20GB file upload support when I'd only specced 10GB — I update the documentation to reflect reality.

The BRD isn't a snapshot. It's a living document that tracks what the product actually is.

Here's what POs get wrong about this shift: they think their job is getting easier.

It isn't.

Your development team is moving faster than ever. Outdated documentation is more dangerous than no documentation. An AI operating on stale context doesn't flag uncertainty — it acts on wrong information with full confidence. Removing context that's no longer true is just as important as adding context that's new.

You're not a knowledge holder anymore. You're a knowledge curator. Curation requires constant discipline — adding, updating, and removing.

The product owner is the king of context. And the king of context doesn't just collect — they govern.

---

I recently came across [Prefect Horizon](https://www.prefect.io/horizon). It is pitched as the "context layer" between AI agents and the business, and I must say, their marketing verbiage tells me they understand the problem. It provides deployment, versioning, and governance for MCP servers — the operational infrastructure you need once context stops living in one person's head and becomes a system. It launched four months ago, built by the team behind FastMCP, which already runs the majority of MCP servers in production.

I'm shipping a small MCP server on Horizon for the Madison cycling community I'm part of. I'm not shilling some AI principle that I don't believe in or ever use. If you want to see what a lightweight context surface looks like in practice, that's a working example.

Nobody has named this in a job description yet. The PO standup still runs like it's 2019. But the job — understood correctly — has always been context curation. The AI age just made you the *Force Multiplier* of your entire product.

In owning that context, you own the product.
