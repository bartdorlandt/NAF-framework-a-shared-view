---
marp: true
theme: dream
_class: lead
paginate: true
_paginate: false
backgroundColor: #fff
backgroundImage: url('template/dream_bg.png')
style: |
    section.block h2 {
        margin-top: 0;
    }
    section.block ul {
        font-size: 0.78em;
    }
    section.block p {
        margin: 0.3em 0;
    }
    section.block .icon {
        position: absolute;
        top: 40px;
        right: 70px;
    }
    .sticker-corner {
        position: absolute;
        top: 40px;
        right: 70px;
    }
    section.map h2 {
        margin: 0;
        font-size: 1.1em;
    }
    section.map p {
        text-align: center;
        margin: 0;
    }
    section.map {
        padding: 20px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
    .reqgrid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 18px;
        margin-top: 0.4em;
    }
    .reqgrid .req {
        border-radius: 10px;
        padding: 16px 18px;
        color: #fff;
        font-size: 0.62em;
        line-height: 1.35;
    }
    .reqgrid .req .tag {
        display: block;
        font-weight: 700;
        letter-spacing: 0.08em;
        font-size: 0.85em;
        margin-bottom: 6px;
        opacity: 0.85;
    }
    .reqgrid.one {
        grid-template-columns: 1fr;
    }
    .reqgrid.one .req {
        font-size: 0.72em;
        padding: 22px 24px;
    }
    .reqgrid.four {
        grid-template-columns: repeat(4, 1fr);
        gap: 14px;
    }
    .reqgrid.four .req {
        font-size: 0.55em;
        padding: 14px 15px;
    }
    .req.must   { background: #0b3d6b; }
    .req.should { background: #1f6fb2; }
    .req.may    { background: #6fa8d6; color: #06263f; }
    .req.may .tag { opacity: 1; }

    section.map img {
        max-height: 92vh;
        max-width: 100%;
    }

---

# The NAF Framework

## A shared view for network automation

Six building blocks, one vocabulary

![height:200px](images/naf_icon.png)

<div class="main-name">
Bart Dorlandt
</div>

<!--
The Network Automation Forum framework.

Not a product. Not a stack. A map.
-->

---
<div class="logo-position">

![w:450px](template/dream_logo_r_bg.png)

</div>

## Bart Dorlandt

- Freelance Network Automation Solution Architect
- Lives by: ___There must be a better way___
- Organizer of NLNAM
- Co-organizer of PyUtrecht
- Advisory board member of Network Automation Forum (NAF)


<!--
Hi, I'm Bart.
I've been in networking all my career and I've been in automation half of my career.

People who know me, know I live by: "There must be a better way"
-->
---

## The conversation we keep having

> "We use Ansible, Nautobot, Grafana and a pile of Python."

That answers **what tools we bought/use.**

It does not answer:

- What job does each tool actually do?
- What is missing?
- What is duplicated three times?

<!--
Ask any team what their automation looks like
- and you get a tool list.

A tool list is an inventory, not an architecture.
-->

---

## Starting from tools is backwards

- No consensus on what the tool should do
- Two tools own the same or overlapping data
- Nobody can say *where* the next feature belongs
- Onboarding = tribal knowledge transfer

**We had no shared vocabulary to talk about the problem.**

<!--
Comparing two designs is difficult or
  impossible if nobody speaks the same language.

Difficult to align between teammates, let alone other teams.
  Going non-technical or upstream is near impossible.
-->

---

## Enter the NAF Framework

A **modular, vendor-neutral reference model** for network automation.

- Published by the Network Automation Forum
- Defines **functional building blocks**, not products
- Design around required functionality — *then* pick tools

<div class="sticker-corner">

![w:260px](images/naf_sticker.png)

</div>

<!--
Written by practitioners, not by a single vendor

Key phrase: functional building blocks that can be composed into tools.

Focus on a shared understanding, intra-team, inter-team and inter-vendor.
    Also allows you to explain it to the CEO
-->

---

## The workgroup

<!-- _class: map -->

![h:500px](images/framework_team.png)

<!--
Again, Not a single vendor.

It got started by Ryan, having weekly calls on Fridays. I was there in the beginning, but I also had my kids running around... Challenging...

It took some time to align, yet now we have a great starting point for a reference model.

From the different views on tools, and disagreements on tools, came a shared understanding of the problem space, and a shared vocabulary to talk about it.
-->

---

<!-- _class: map -->

![](images/naf_framework_full.png)

<!--
Six blocks plus the network infrastructure at the bottom.
Write side on one flank, read side on the other, coordination in the middle,
humans on top.

| Block         | Job                                                |
| ------------- | -------------------------------------------------- |
| Intent        | Store and shape the **desired** state              |
| Executor      | **Write** changes to the network                   |
| Collector     | **Read** the actual state from the network         |
| Observability | Persist and process the **actual** state           |
| Orchestrator  | **Coordination** role, tasks in response to events |
| Presentation  | Where **humans** (or machines) meet the system     |
-->

---

<!-- _class: block -->

<div class="icon">

![w:100px](images/icon_intent.png)

</div>

## Intent

> Defines the logic to handle and the persistence layer to store the desired state of the network, including both configuration and operational expectations

<div class="reqgrid four">

<div class="req must"><span class="tag">MUST</span>be capable of representing, in a structured form, any network-related aspect</div>
<div class="req must"><span class="tag">MUST</span>support create, read, update, and delete operations</div>
<div class="req must"><span class="tag">MUST</span>be exposed through a standardized, well-documented API</div>
<div class="req should"><span class="tag">SHOULD</span>provide a consistent and unified view of the desired state</div>
<div class="req should"><span class="tag">SHOULD</span>use a neutral representation that will be derived into vendor-specific configuration artifacts</div>
<div class="req should"><span class="tag">SHOULD</span>include metadata that supports effective data governance</div>
<div class="req should"><span class="tag">SHOULD</span>be transactional, and provide versioned access to data</div>
<div class="req may"><span class="tag">MAY</span>include all the logic related to intended state management</div>

</div>

<!--
This is your intent data (SoT). Note "one consistent view" does not mean "one database" - it may be federated across sources.

Not going to match tools to the blocks just yet.
-->

---

<!-- _class: block -->

<div class="icon">

![w:100px](images/icon_executor.png)

</div>

## Executor

> Encompasses the actual tasks applied to the network to drive changes (e.g., updating configuration) as guided by the intended state.

<div class="reqgrid">

<div class="req must"><span class="tag">MUST</span>be capable of interacting with any of the supported network write interfaces</div>
<div class="req should"><span class="tag">SHOULD</span>come from the intended state or be derived from it</div>
<div class="req should"><span class="tag">SHOULD</span>support any network operation that alters the network state</div>
<div class="req should"><span class="tag">SHOULD</span>provide a dry-run operation to check the expected result of the execution</div>
<div class="req should"><span class="tag">SHOULD</span>support transactional execution of the changes</div>
<div class="req may"><span class="tag">MAY</span>support both imperative and declarative approaches</div>

</div>

<!--
SSH, NETCONF, RESTCONF, SNMP, CLI, etc.
-->

---

<!-- _class: block -->

<div class="icon">

![w:100px](images/icon_collector.png)

</div>

## Collector

> Focuses on retrieving (i.e., reading) the actual state of the network

<div class="reqgrid one">

<div class="req must"><span class="tag">MUST</span>include capabilities for retrieving live data from the network using read interfaces (push, pull)</div>

</div>

<!--
Splitting read from write is the single most useful line in the whole
framework. Most homegrown stacks blur them and then cannot tell you
whether they are describing reality or ambition.

SSH, API, SNMP, CLI, gRPC, gNMIc, metrics, logs, flows, packet captures, etc.
-->

---

<!-- _class: block -->

<div class="icon">

![w:100px](images/icon_observability.png)

</div>

## Observability

> Stores the actual network state, and defines the logic to process the observed data.


<div class="reqgrid four">

<div class="req must"><span class="tag">MUST</span>support historical data persistence</div>
<div class="req must"><span class="tag">MUST</span>offer programmatic access to this data</div>
<div class="req should"><span class="tag">SHOULD</span>offer a capable query language to extract the data</div>
<div class="req should"><span class="tag">SHOULD</span>expose relevant insights into the current network state</div>
<div class="req should"><span class="tag">SHOULD</span>automatically generate events when discrepancies are detected</div>
<div class="req should"><span class="tag">SHOULD</span>normalize data into a vendor-agnostic data model</div>
<div class="req may"><span class="tag">MAY</span>have events processed by humans or connected to the Orchestrator</div>
<div class="req may"><span class="tag">MAY</span>be enriched with contextual information from the intended state</div>
</div>


<!--
Sources: Logs, Metrics, traces, flows, Packet Captures, State, Config, Probes


This is where drift detection lives. Compare intent to reality, emit an event.
That event either reaches a human or the Orchestrator.

Push/Pull/Both - Protocol?

Collector > Normalization > Enrichment > Storage > Query | Event | Insights logic
-->

---

<!-- _class: block -->

<div class="icon">

![w:100px](images/icon_orchestration.png)

</div>

## Orchestrator

> Coordinates activities across various building blocks


<div class="reqgrid four">

<div class="req must"><span class="tag">MUST</span>enable coordination of processes across the various building blocks</div>
<div class="req should"><span class="tag">SHOULD</span>follow an event-driven approach for process execution</div>
<div class="req should"><span class="tag">SHOULD</span>provide a dry-run operation to check the expected result of the workflow</div>
<div class="req should"><span class="tag">SHOULD</span>provide the ability to schedule the execution of a workflow</div>
<div class="req should"><span class="tag">SHOULD</span>allow execution logic to perform reverse/compensating actions</div>
<div class="req should"><span class="tag">SHOULD</span>provide logging and traceability of past and current workflows</div>
<div class="req may"><span class="tag">MAY</span>include logic to correlate multiple events and infer relationships</div>

</div>

<!--
**Coordination. Never touches the network directly.**
- Event-driven: synchronous, asynchronous or scheduled
- Compensating actions for controlled rollback
- Dry-run of a whole workflow
- Logging and traceability of past and current runs
- MAY correlate events and infer the right response

Note the boundary: the Orchestrator calls the Executor, it does not SSH
anywhere itself. If your workflow engine has device credentials, you have
merged two blocks.
-->

---

<!-- _class: block -->

<div class="icon">

![w:100px](images/icon_presentation.png)

</div>

## Presentation

> Provides the interfaces through which users interact with the system, including dashboards, graphical user interfaces

<div class="reqgrid">

<div class="req must"><span class="tag">MUST</span>provide robust authentication and authorization capabilities</div>
<div class="req may"><span class="tag">MAY</span>support both read and write interactions</div>
<div class="req may"><span class="tag">MAY</span>take various forms depending on the needs of the end user</div>

</div>

> This does **not** imply a single pane of glass.

<!--
**The human contact point.**

- Dashboards, GUIs, ITSM, change management, chat, docs portals
- Read *and* write: view data, start tasks, approve changes

-->

---

<!-- _class: map -->

![](images/naf_framework_full.png)

<!-- Quick overview again -->
---

## The rules that make it work

- Blocks are **functions**, not products
- One block MAY be many components — or one tool MAY cover several blocks
- Everything exposes a machine-friendly, schema-documented API
- Version control and CI/CD for all of it
- Make operations idempotent and transactional where possible

<!--
Nautobot is intent plus presentation. Ansible is executor plus a bit of
orchestration.

That is fine - as long as you know which hat it wears.

But you may need to think of where does my batfish validation go?
Do you have automated end-to-end validation with containerlab, where does that go?

(Not for now to discuss, at the end or in the break)
-->

---

## How to actually use it

1. **Map** what you have onto the six blocks
2. Look for the **empty** boxes
3. Look for the **crowded** boxes — multiple tools writing the same data
4. Pick the one gap that hurts most; fill only that
5. Repeat

**The map is not a shopping list.**

<!--
An hour with a whiteboard and these six words/boxes is worth more than a
year-long tool evaluation.
-->

---

## What you get out of it

- A vocabulary your whole team shares
- A place to put every new requirement
- An honest picture of duplication and gaps
- Vendor conversations on *your* terms: "which block do you fill?"
- A way to start narrow without painting yourself into a corner

<!--
The framework does not tell you what to buy. It tells you what question to ask.

Last statement, You are not forced to do it all at once.
-->

---

## Call to Action

Take your own stack and ask:

1. **Where is my intent?** Is it a database or a set of habits?
2. **Can I see drift?** Actual state versus intended state, automatically?
3. **Who writes?** Does anything besides the Executor touch the devices?
4. **What is the one empty box?** Start there.

<!--
Let's have a chat in November at the next NLNAM and see what you have learned.
-->

---
## NAF Matrix - Christian Drefke

https://naf-framework-solution-matrix.vercel.app/

![w:1130px](images/naf_matrix.png)

<!--
Use this to make it nice and easy!
Easy to create, export/import save to png etc.
-->
---

## Q&A

- Bart Dorlandt
- https://linkedin.com/in/bartdorlandt/
- https://dreamnetworking.nl/
- https://net-auto.nl/

<span class="small">The framework:
[reference.networkautomation.forum/Framework/Framework](https://reference.networkautomation.forum/Framework/Framework/)</span>

<div class="bottom-right">

![w:220px](template/qr_bart_linkedin.png)

</div>
