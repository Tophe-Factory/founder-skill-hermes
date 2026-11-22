---
name: founder-board
description: >-
  Your board of directors. Puts a business idea in front of a three-member board,
  each a sub-agent reading the idea through one published framework: the Offers
  lens (from Alex Hormozi's $100M Offers), the Monopoly lens (from Peter Thiel's
  Zero to One) and the Product lens (from Walter Isaacson's biography of Steve
  Jobs). Each member scores the idea, names the risks that would kill it and
  votes fund, fund with conditions, or pass. Use when the user says "test my
  business idea", "would this work", "pressure-test this", "what would a board
  say", "is this a good business", or starts the founder pack on a new idea.
version: 1.0.1-hermes
license: MIT
metadata:
  hermes:
    tags: [Business, Validation, Startup]
    requires_tools: [delegate_task]
---

# founder-board

The first meeting. Before anyone builds a website or signs a lease, three board
members tear the idea apart, each from a different angle, and vote.

The idea comes from the user's message. If the user gave none, take the idea from
the conversation.

## The rule about the three names

The lenses are summaries of **published frameworks**, nothing more. The board
members are not those people, never speak as them, never quote them, never claim
they would endorse anything. Write "the Offers lens", never "Alex Hormozi says".
If the user asks what one of them really thinks, say you can only apply their
public frameworks.

## Step 0: the idea file

Everything in the founder pack reads `founder/idea.md` in the user's workspace. If
it does not exist, write it now from the conversation:

```markdown
# <business name, or a working name>

- What it is: one sentence.
- Who it is for: the target customer, as specific as the user can make it.
- What it sells, at what price: the core product and its price.
- Where and how: city or online, how customers find it, how it is delivered.
- Budget and constraints: money, time, skills, anything already decided.
```

If the name, the customer, the product or the price is missing, ask for those (at
most three questions in one message), then write the file. Do not invent a
budget, a market size or a competitor.

## Step 1: brief the board

Write `founder/board/brief.md`: the idea file plus anything else the user said
that a stranger would need. **Sub-agents start with a completely fresh
conversation and know nothing of this one**, so the brief must stand on its own.
Include in the brief the full path of the lens file each member must read. Do not
add your own opinion of the idea to it.

## Step 2: convene

Call `delegate_task` ONCE with three parallel tasks (Hermes runs up to 10
concurrent sub-agents by default). Each task carries everything its member needs,
because context arrives only through `goal` and `context`:

```python
delegate_task(tasks=[
    {
        "goal": "Apply ONLY the Offers lens and write your board memo to founder/board/offers.md",
        "context": "<full text of founder/board/brief.md>\nFirst read the lens file at ~/.hermes/skills/founder-board/lenses.md (Offers section) and apply only it."
    },
    {
        "goal": "Apply ONLY the Monopoly lens and write your board memo to founder/board/monopoly.md",
        "context": "<full text of founder/board/brief.md>\nFirst read the lens file at ~/.hermes/skills/founder-board/lenses.md (Monopoly section) and apply only it."
    },
    {
        "goal": "Apply ONLY the Product lens and write your board memo to founder/board/product.md",
        "context": "<full text of founder/board/brief.md>\nFirst read the lens file at ~/.hermes/skills/founder-board/lenses.md (Product section) and apply only it."
    }
])
```

Adjust the lens file path if the skill was installed somewhere other than
`~/.hermes/skills/founder-board/` (the file is `lenses.md`, next to this
SKILL.md). Each sub-agent writes its memo itself with its own terminal; do not
write memos for them. Each memo, in this shape:

```markdown
# <Lens> lens
Score: <1-10> | Vote: FUND / FUND IF / PASS
## What is strong
## What would kill it (the top 3, most dangerous first)
## The questions I would ask the founder
## If FUND IF: the conditions
```

## Step 3: the chair's summary

Read the three memos (they are short; `delegate_task` returns each member's
final summary, and the full memos are in `founder/board/`) and write
`founder/board.md`:

1. The vote, e.g. "2 fund if, 1 pass", and the average score.
2. The risks more than one member raised: these go first.
3. Every condition, merged, as a checklist the rest of the pack can tick off
   (the CFO, the consumer panel and pricing will test most of them).
4. One paragraph: the strongest version of this business the board can see.
   It may differ from what the user pitched; say so plainly.

Keep the members' disagreements. A board that agrees on everything was not doing
its job.

## Rules

- Score the business, not the founder.
- No made-up facts. If a lens needs a number nobody has yet (market size, cost of
  acquiring a customer), name it as an open question and say which skill answers
  it (`founder-competitors`, `founder-consumer`, `founder-cfo`).
- A PASS is a result, not a failure. Say it plainly.

## Output

`founder/idea.md`, `founder/board/` (brief and three memos), `founder/board.md`.
Then say the vote in one line and suggest the next step:
`founder-competitors` to see who is already there, or `founder-consumer` to put
it in front of 100 simulated buyers.

## Where the pack goes next

board -> competitors -> consumer -> pricing -> offer -> cfo -> marketing -> brand -> ops -> launch -> plan

Every skill can also run on its own; each reads whatever is already in `founder/`.

## Pitfalls (Hermes-specific)

- In one-shot sessions (`hermes chat -q`), the number of spawned subagents is capped by `delegation.oneshot_max_children` (default 2). Run the board from an interactive session, or raise the cap in config.yaml.
- If a sub-agent returns before writing its memo file, re-read the file; only re-delegate if the file is truly missing.
