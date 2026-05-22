# Working Alongside an AI: The Discernment Problem

If you are going to use an AI to help produce technical research, the
hard part is not getting it to generate output. It will generate output
all day. The hard part is catching the output that is wrong, and the
model will not catch it for you.

What follows are seven failure modes in the collaboration itself. They
are distinct from any quality rules in your methodology - those govern
what good output looks like. These govern how good output silently turns
into bad output when a human stops paying attention. Each one is drawn
from real work building a TRR in an ordinary back-and-forth chat.

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

Once the model can see your existing draft, it anchors to it. Paste a
finished section into the chat and ask for a critique, and it tends to
validate what is already there rather than re-derive the answer. This
helps when the draft is right and reinforces the error when it is wrong.

The reliable workaround is to open a fresh conversation and have the
model research the point from scratch, without showing it the draft,
then compare the two results yourself. Seeing the draft first produces
agreement; withholding it produces the gaps that agreement was hiding.

If you want a genuine second opinion, do not hand it the first one.

---

## 4. Hallucinated Capabilities

The model will confidently claim it can do things it cannot, and reason
from context it does not actually have.

It may offer to "run the test and confirm" or "check the lab" when it
can only describe what should happen - it cannot execute anything and
has no results to report. In another case it stated flatly that certain
files did not exist, while it was working from a partial slice of the
project rather than the full picture; it reasoned from an incomplete
view and presented the conclusion as fact.

The model does not reliably know the edge of its own abilities or the
limits of what it has actually seen. When it says it checked something,
make sure that means it looked, not that it inferred.

---

## 5. Drift

Large context windows have taken much of the edge off this one - a model
holding a million tokens does not lose the early thread as fast as one
holding a fraction of that. But over a long enough session it still
happens, in two ways.

The first is scope. A technique you bounded clearly at the start
gradually picks up material from neighboring techniques as the
conversation goes deep. A report scoped to file-based execution starts
absorbing details that belong to a different, module-based technique
entirely, and the boundary you set earlier quietly stops holding.

The second is discipline. The methodology rules the model applied
cleanly early in the session erode as context fills. The exact errors
the prompt was written to suppress - treating a tool as a procedure,
slipping optional operations into the model - start creeping back. You
correct them, they hold for a while, then they slip again.

The practical fix is a clean handoff. Have the model produce a detailed
summary of the conversation so far, then carry that summary along with
your own notes into a fresh session - you will be back in the groove
almost immediately. Re-anchoring deliberately like this beats fighting
the decay in place, especially once a session has run long.

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

Conciseness is a constraint you impose, and the levers are simple. Some
models offer style settings - Claude, for instance, has a concise mode
you can switch on - and even without that, plainly asking the model to
be brief or to present something concisely works well. The catch is that
its instinct still drifts back toward more, so you re-impose the
constraint as the session runs. More is rarely the deliverable.

---

## 7. The Verification Burden

You cannot assume the model did what it said, or that its output is
clean and accurate as delivered.

A dropped connection mid-response leaves you unsure what actually
landed, so you have to check before continuing. The model will sometimes
report that it folded in a change it did not fully make, which only
reading the result back will reveal. It will also hand you sources that
do not hold up - a reference link that 404s, points somewhere unrelated,
or was never a real URL to begin with. Leave one unchecked and you have
published a citation that leads nowhere, or worse, one that does not say
what you claimed it does.

For factual claims in particular, make verification fast by asking for
the sources up front: a link to each reference, and ideally a specific
quote from it. Drop that quote into the page's find function and you
land on the exact context in seconds, instead of rereading a whole
document to confirm a single point.

Verify directly. Read the output back, confirm the claims trace to real
sources, and open the links before you trust them. Checking is cheap. A
silent failure that ships is not.

---

## The Shape of the Problem

None of this is an argument against working with an AI. The throughput
is real, and it is what makes producing this kind of research at volume
possible at all. But every failure here is invisible by default and
ships unless someone in the loop is actively checking the work rather
than accepting it.

It is tempting to conclude that you therefore have to be an expert going
in. You do not. Someone can come to this with no detection-engineering
background and no technique-research experience, learn the method from
good source material, bring real fluency with the tool itself, and -
through a lot of iteration - produce work worth submitting for review.

What that requires is not expertise up front but grounding every step in
something outside the model: a methodology to check the shape of the
work against, documentation and a lab to settle matters of fact, and
eventually real reviewers to catch what you cannot. The judgment gets
built during the work rather than brought to it - but it has to be
built, and it has to stay active. The model supplies speed and
scaffolding. It never supplies the verdict on whether the output is
right.

So the accelerator holds, with one correction: the driver does not have
to start as an expert, but they do have to keep their eyes on the road
and check it against the map the whole way. Stop steering and the model
takes you into the wall at speed. Keep steering, keep verifying, and it
gets you somewhere real - faster than you could have alone.
