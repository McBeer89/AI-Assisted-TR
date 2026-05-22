# Working Alongside an AI: The Discernment Problem

If you are going to use an AI to help produce technical research, the
hard part is not getting it to generate output. It will generate output
all day. The hard part is catching the output that is wrong, and the
model will not catch it for you.

What follows are seven failure modes in the collaboration itself. They
are distinct from any quality rules in your methodology - those govern
what good output looks like. These govern how good output silently turns
into bad output when a human stops paying attention. Each one is drawn
from real TRR production work.

The single thing to take away: the AI is an accelerator. It makes a
competent researcher faster. It does not make a novice competent, and
every failure below ships silently if no one in the loop can tell right
from wrong.

---

## 1. Confident Wrongness

The model states false things with the same fluency and certainty it
uses for true ones. There is no tonal tell. A hallucinated telemetry
claim reads exactly like a verified one.

In practice, a draft TRR asserted that Sysmon EID 7 fires for
CLR-managed assembly loads. Lab work proved it does not - not for
disk-backed loads, not for reflective ones. Trusting the first answer
would have shipped wrong telemetry guidance. Separately, the model once
placed IIS request-logging telemetry on the attacker-side node when it
belongs on the server side, and it treated Classic ASP and ASP.NET as
one component when they are separate role services installed
independently.

Treat any claim about telemetry behavior or platform internals as
unverified until you confirm it. The lab is the arbiter, not the model.

---

## 2. Agreement Bias

The model builds on what you assert. Tell it "Sysmon will catch this"
and it will agree and elaborate. This is the hardest failure to see,
because it does not look like an error - it looks like the conversation
going well.

The discernment burden is not only catching what the model says cold.
It is noticing when the model agreed too readily with something you
introduced, particularly when you are moving fast and want to be right.

A confirmation from the model is weak evidence. Treat the model agreeing
with you as roughly equivalent to the model having no objection - which
is not the same as the claim being correct.

---

## 3. Anchoring

Once the model can see existing work, it anchors to it. Ask it to review
a document and it tends to validate what is already written rather than
re-derive the answer independently. This helps when the existing work is
correct and reinforces the error when it is not.

The fix that worked was structural: a review process that re-researches
the technique *without reading the existing report*, then compares.
Letting the reviewer see the report first produced agreement. Blinding
it produced the gaps that agreement had been hiding.

If you want a genuine second opinion, withhold the first one. Blinding
the model beats asking it to "be critical."

---

## 4. Hallucinated Capabilities

The model will confidently propose actions it cannot take and assume
tools exist that do not.

In one case an automated check flagged incomplete work and then invented
a "lab-verifier" capability that could run the Windows lab on its own.
No such thing exists; the model cannot execute the lab. In another, the
model made absolute claims that certain files did not exist while
working from a partial view of the project rather than the actual
filesystem - it reasoned from an incomplete picture and stated the
conclusion as fact.

The model does not reliably know the edge of its own abilities or its
own context. When it says "I checked and it is not there," make sure
that means it looked, not that it inferred.

---

## 5. Drift

Over a long session the model loses the thread. The scope you set at the
start fades, and material from adjacent techniques starts bleeding in.
There is a second form of drift too: when your methodology lives across
many files, the model's output quietly degrades whenever any one of
those files falls behind the others.

A scoping run could not distinguish a file-based variant from a fileless
one and had to be steered back by hand - which is why an explicit
scope-confirmation step now sits between research and the rest of the
work. And a single stale methodology file, left behind during an update,
became the root cause of a run of output errors, because the agents were
faithfully following an out-of-date source.

Re-anchor scope deliberately, and expect to do it more as the session
grows. When the methodology changes, every file it lives in has to move
together - the model is only as correct as its least current context.

---

## 6. Overproduction

Left to its defaults, the model produces too much: too long, too
formatted, too many headers and tables. Volume reads as effort, but it
buries the signal.

A set of talking points came back at over two hundred lines and had to
be cut to something scannable. Analyst-facing briefs were repeatedly too
verbose and needed multiple tightening passes, including full restarts
when the model wandered into doing the wrong discipline's work inside
the deliverable.

Conciseness is a constraint you impose and then re-impose. The model's
instinct is always toward more, and more is rarely the deliverable.

---

## 7. The Verification Burden

You cannot assume the model did what it said. Edits fail silently,
connections drop mid-write, and automated steps run away from you.

File edits failed without warning when special characters did not match
exactly; the only reliable confirmation was reading the file back
afterward. Dropped connections meant work had to be checked for what
actually persisted. Research steps defaulted to many more web fetches
than necessary and burned through budget until an explicit limit was
set.

Trust nothing as done until you have verified it directly. Reading the
result back is cheap. A silent failure that ships is not.

---

## The Shape of the Problem

None of this is an argument against working with an AI. The throughput
is real, and it is what makes producing this kind of research at volume
possible at all. But every failure here is invisible by default and ships
unless a person who understands the domain is exercising judgment at each
step.

That is the part worth being honest about: this way of working does not
turn inexperience into expert output. It lets someone who can already
recognize right from wrong reach that output faster. The model is an
accelerator, and an accelerator with no driver only reaches the wall
sooner.
