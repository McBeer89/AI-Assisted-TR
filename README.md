# AI-Assisted Technique Research

A starter kit for researching attack techniques and vulnerabilities with an AI
assistant, following the TIRED Labs methodology developed by Andrew VanVleet.

The AI does not do the research *for* you. It helps *you* build understanding,
pressure-tests your reasoning, helps you model the technique, and helps you
validate it - you stay in the driver's seat and verify every claim it makes. The
goal is a **Technique Research Report (TRR)** and one or more **Detection Data
Models (DDMs)**: discipline-neutral references that document how a technique
works at the level of its essential operations, equally useful to threat
intelligence, red team, detection engineering, and incident response.

You do not need prior TRR experience. You do need enough curiosity to read
technical documentation and the discipline to verify what the AI tells you.

---

## What's in this repo

| Path | What it is |
|------|------------|
| `Prompts/TRR-Research-Prompt.md` | Instructions for the **research** assistant (Phases 1-4: scope, build the DDM, identify procedures, validate by emulation). |
| `Prompts/TRR-Authoring-Prompt.md` | Instructions for the **authoring** assistant (Phase 5: write the TRR and cut the per-procedure DDM diagrams). |
| `Prompts/Vulnerability-Research-Prompt.md` | Instructions for the **vulnerability research** assistant - understand a vulnerability (e.g. a CVE), verify it with a lab emulation test, and identify the technique(s) it aligns with. Ends in understanding, not a DDM/TRR. |
| `Project Knowledge/` | **The files you upload to your AI project.** The reference material as six flat, self-contained files (methodology, example DDMs, four example TRRs). Knowledge bases do not keep folders, so each set is bundled into one file. |
| `TRR_Research_Methodology_Guide.md` | A plain-language walkthrough of the methodology, for the "why" behind the steps. |

Each prompt runs as its own AI project. The two TRR prompts share a common core
and split the technique workflow in half; the vulnerability prompt reuses the
same research-alongside-AI method for a different goal.

---

## How the workflow runs

1. **Research project** (`TRR-Research-Prompt`) - you and the assistant scope the
   technique, build the DDM, identify its procedures, and validate them in a lab.
   Output: a complete **research package**.
2. **Authoring project** (`TRR-Authoring-Prompt`) - you hand the research package
   over, and the assistant turns it into a publication-ready TRR and final DDM
   diagrams.

Keep them as two projects. Each assistant is tuned for its half of the job, and a
clean project per half keeps the AI focused.

Researching a **vulnerability** instead of a technique? Start with the
**Vulnerability Research project** (`Vulnerability-Research-Prompt`). It uses the
same method to understand the vulnerability and verify it with a lab emulation
test, then points you to the Research project if the exploitation is a technique
worth modeling.

---

## Setup: build the AI project

This works in any project-style AI workspace - Claude.ai Projects, Google AI
Studio (or a Gemini Gem), or ChatGPT Projects. Each gives you the two things you
need: a place for **instructions** (the prompt) and a place for **knowledge**
(the reference material).

**1. Create a project.** Make one project per prompt you need: start with the
research or vulnerability-research prompt depending on your goal, and add an
authoring project when a technique reaches Phase 5.

**2. Add the prompt as the project's instructions.** Copy the entire contents of
the prompt for that project (`TRR-Research-Prompt.md`,
`Vulnerability-Research-Prompt.md`, or `TRR-Authoring-Prompt.md`) into the
project's instruction / system-prompt field.

**3. Upload the reference material.** Upload the six files in the
`Project Knowledge/` folder - they are flat and self-contained, so they work in
any knowledge base (and fit even a Gemini Gem's file limit). They give the
assistant real examples to consult. They come pre-bundled because knowledge
bases are flat - the four example TRRs would otherwise collide as identical
`README.md` files.

**4. Start.** Open a new chat in the project and tell it what you want to research.
The assistant opens by gauging your experience level, then asks for the technique
or vulnerability and walks you through each phase.

Where each piece goes, by platform:

| Platform | Prompt goes in | Reference material goes in |
|----------|----------------|----------------------------|
| Claude.ai | Project **Instructions** | Project **Knowledge** (upload files) |
| Google AI Studio / Gemini Gem | **System instructions** / Gem instructions | Attached files / Gem **Knowledge** |
| ChatGPT | Project **Instructions** (or a Custom GPT) | Project **Files** (or GPT **Knowledge**) |

**Optional - connectors.** Some platforms let you attach connectors (also called
apps or extensions) so the assistant can pull live information - vendor
documentation, code repositories - beyond the files you upload. They are not
required for this project, but they help with the documentation lookups
technique research leans on. On Claude.ai, for example, the Microsoft Learn
connector (live Microsoft and Azure docs) and the GitHub connector (repositories
such as Atomic Red Team) are useful here, with many more in Claude's connector
directory and support for custom remote MCP servers. ChatGPT offers comparable
"apps" (including a GitHub app that reads repository code and docs), and Gemini
offers connectors and extensions - availability varies by plan and changes
often, so check each platform's connector or app settings for what is current.

When you reach Phase 5, repeat this setup in a second project using
`Prompts/TRR-Authoring-Prompt.md`, then carry your research package across.

---

## Using it: pick something and start digging

1. **Pick a target.** Any attack technique, vulnerability class, or cyber behavior
   you want to understand - "web shell execution on IIS", "Kerberos service-ticket
   roasting", a CVE you want to model.
2. **Research it.** In the research project, work through the phases with the
   assistant. Verify its claims against the sources it cites - for every
   substantive finding it gives you a source, a short quote, and where to find it.
   Build the DDM in [Arrows.app](https://arrows.app/); watch the tutorial video in
   the references below if it is new to you.
3. **Validate it.** Where you can, run the assistant's emulation tests in a lab you
   own or are authorized to test, and confirm the technique behaves and logs as
   modeled. Never run them in production.
4. **Author it.** Move the finished research package to the authoring project and
   produce the TRR and final diagrams.

Take it slowly. Accuracy over speed - an honest "I need to verify this" beats a
confident guess every time.

---

## References

- [TIRED Labs TRR Library](https://library.tired-labs.org) - published TRRs and
  the home of the methodology.
- [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)
  by Andrew VanVleet - the articles the whole method is built on:
  - [Plotting a Winning Threat Detection Strategy: A Visual Model](https://medium.com/@vanvleet/threat-detection-strategy-a-visual-model-b8f4fa518441)
  - [Identifying and Classifying Attack Techniques](https://medium.com/@vanvleet/identifying-and-classifying-attack-techniques-002c0c4cd595)
  - [The Relative Strengths of Threat (Detection|Hunting)](https://medium.com/@vanvleet/the-relative-strengths-of-threat-detection-hunting-777b03a89d15)
  - [Compound Probability: You Don't Need 100% Coverage to Win](https://medium.com/@vanvleet/compound-probability-you-dont-need-100-coverage-to-win-a2e650da21a4)
  - [TTPI's: Extending the Classic Model](https://medium.com/@vanvleet/ttpis-extending-the-classic-model-058c572b76f3)
  - [The Threat Detection Balancing Act: Coverage vs Cost](https://medium.com/@vanvleet/the-threat-detection-balancing-act-coverage-vs-cost-cdb71d21412f)
  - [Improving Threat Identification with Detection Data Models](https://medium.com/@vanvleet/improving-threat-identification-with-detection-data-models-1cad2f8ce051)
  - [DDM Use Case: What ATT&CK Gets Wrong about Process Injection](https://medium.com/@vanvleet/ddm-use-case-what-att-ck-gets-wrong-about-process-injection-7c15b6764bfe)
  - [Mistaken Identification: When an Attack Technique isn't a Technique](https://medium.com/@vanvleet/mistaken-identification-when-an-attack-technique-isnt-a-technique-8cd9dae6e390)
  - [Creating Resilient Detections](https://medium.com/@vanvleet/creating-resilient-detections-62f9eb5318eb)
  - [Technique Analysis and Modeling](https://medium.com/@vanvleet/technique-analysis-and-modeling-ffef1f0a595a)
  - [Technique Research Reports: Capturing and Sharing Threat Research](https://medium.com/@vanvleet/technique-research-reports-capturing-and-sharing-threat-research-003c80ac9a4d)
- [Arrows.app](https://arrows.app/) - the tool for drawing DDMs.
- [Arrows.app tutorial video](https://www.youtube.com/watch?v=ZHJ-BrKJ8A4) - a
  short visual walkthrough.
- [MITRE ATT&CK](https://attack.mitre.org/) - technique and tactic reference.
- [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team) and
  [Stratus Red Team](https://github.com/DataDog/stratus-red-team) - existing
  emulation tests to learn from.
