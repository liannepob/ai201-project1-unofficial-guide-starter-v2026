# The Unofficial Guide

Lianne Poblador - campus_life corpus

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none, because the grader can't
> read it.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Week 1

## What This Does

This system answers questions about a `campus_life` corpus, a set of
short forum-style posts covering things like course workloads, dorm
amenities and costs, dining hall wait times, and administrative deadlines.
Ask it a question a student would actually ask (like "how much does laundry
cost in the dorms?") and it retrieves the relevant post and answers from it,
citing the source file. If you ask something outside the corpus, it says so
instead of guessing.

Milestone 5.

## Chunking Strategy

**Chunk size:** 800
**Overlap:** 120

<!-- What about YOUR documents made you pick these numbers? Short posts and
long sectioned guides don't want the same chunking, and "800 seemed
reasonable" earns nothing. Point at something you noticed when you read
the documents in Milestone 1.

If you changed your mind partway through, say so and say why. That's worth
more than pretending you got it right first time.

The `campus_life` corpus is made of short forum-style posts, averaging 317 characters against a chunk size of 800. My strategy keeps a whole post as one chunk whenever it fits, and only falls back to splitting on paragraph breaks (and then sentences) if a post exceeds the chunk size. In practice, almost every post in this corpus fit in one chunk on its own, so the paragraph and sentence fallback rarely triggered. This makes sense for the data: each post is already one self-contained thought (a question, a piece of advice, a workload report), so splitting it further would break apart context that belongs together.

Milestone 3. -->

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
AND the function that produced it — the grader checks your code against
what you claim here.

`python app.py chunks -n 5` prints all three for you. Copy them straight
across.

Milestone 3. -->

**Chunk 1** — source: `admin_add_drop_deadline.txt#0` — produced by: `chunker.py::split_documents`

```
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** — source: `course_biol_160.txt#0` — produced by: `chunker.py::split_documents`

```
BIOL 160 Cell Biology

I lived here my sophomore year. Format is lecture three times a week with a weekly lab. Assessment: four unit tests and a cumulative final. Not curved.

Expect 9 to 11 hours a week, the heaviest first-year course by reputation.

The one piece of advice: the unit tests come fast, roughly every three weeks; falling behind once is very hard to recover from.
```

**Chunk 3** — source: `course_hist_118_workload.txt#0` — produced by: `chunker.py::split_documents`

```
Workload for HIST 118 Modern World History

People keep asking so: a lot of reading, about 120 pages a week, but no problem sets. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.
```

**Chunk 4** — source: `dining_pellew_dining_hall_followup.txt#0` — produced by: `chunker.py::split_documents`

```
Re: Pellew Dining Hall

Adding to what people have said about Pellew Dining Hall. The wait figure of 12 to 18 minutes at peak matches what I've seen. If you're trying to eat between classes, go before 11:45 and it's a different building entirely.

Also worth saying: the furthest hall from anywhere, next to the athletics centre. Nobody tells you this at orientation.
```

**Chunk 5** — source: `housing_innisfree_hall.txt#0` — produced by: `chunker.py::split_documents`

```
Innisfree Hall — what it's actually like

Transferred in last year, so take this with a grain of salt. Built 1991, renovated 2022. Rooms are doubles arranged as pairs sharing one bathroom between two rooms.

The good: the shared-bathroom-between-two-rooms arrangement is the best compromise on campus.

The bad: no air conditioning, which matters for the first three weeks of September.

Laundry costs $1.75 wash, $1.75 dry, app-based. On noise: moderate; the building is L-shaped and the short wing is much quieter.
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
visible. Milestone 4. -->

**Question:**  What's the wait time like at Pellew Dining Hall?

**Answer:** At Pellew Dining Hall, the wait time is 12 to 18 minutes at peak.

Sources: dining_pellew_dining_hall.txt and dining_pellew_dining_hall_followup.txt

```
```

**My relevance cutoff:** 0.6

<!-- The number you set in config.py, and how you got there.

In-corpus questions scored between 0.177 and 0.392. Out-of-scope questions
scored between 0.764 and 0.948. There's a wide gap between the two groups
with no overlap, so 0.6 sits comfortably in the middle and cleanly
separates answerable questions from ones the corpus can't cover.

Milestone 4. -->

| Question | In corpus? | Best distance |
|---|---|---|
| How long do I have to add a course? | Yes | 0.392 |
| How much does laundry cost in the dorms? | Yes | 0.379 |
| What's the workload like for HIST 118? | Yes | 0.314 |
| Is Innisfree Hall renovated? | Yes | 0.335 |
| What's the wait time like at Pellew Dining Hall? | Yes | 0.177 |
| What's the capital of France? | No | 0.843 |
| How do I file my taxes? | No | 0.870 |
| What's the best pizza topping? | No | 0.764 |
| Who won the last Super Bowl? | No | 0.948 |
| How do I fix a flat tire? | No | 0.798 |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
what you changed about it.

"I asked Claude to write the chunking function from my notes. It ignored
the overlap, so I added that myself" is the level of detail we're after.
"I used AI to help me code" is not.

Milestone 5. -->

**1.**  I asked Claude for an example chunking function for a short-post
corpus. It gave me a paragraph-and-sentence-splitting strategy with
overlap, but when I ran it on my `campus_life` data, almost every post
fit in one chunk anyway (avg 317 chars vs 800 chunk_size), so the
sentence-splitting logic never actually fired. I kept the simpler
"whole post if it fits" behavior since that's what my data needed.

**2.** I asked Claude to help me read my 10 distance scores (5 in-corpus,
5 out-of-scope) to decide on a relevance cutoff. It pointed out the two
groups had a wide, non-overlapping gap (0.177–0.392 vs 0.764–0.948), so
I kept the starter's default of 0.6 rather than second-guessing it, since
it already sat cleanly in the middle of that gap.

**3.** For Week 2, I used Claude to help trace criterion 5's failure to a specific stage. I pasted my parking question's retrieved chunks and asked whether the miss was retrieval or the gate. It suggested testing rephrased versions of the question through `python app.py retrieve`, which showed the same file scoring 0.682, 0.528 and 0.184 depending on wording, confirming the gate (not retrieval) was the cause. I ran those tests and read the actual file contents myself to confirm the fix would be safe against my out-of-scope questions before changing `config.py`.

**4.** Before finalizing my Milestone 2 verdict on criterion 1, I asked Claude to argue the opposite verdict on my close calls (parking and laundry, which only loosely matched their retrieved files). It pointed out the laundry answer didn't actually address "how do I know when machines are available" even though it cited the right files, which made me note that gap in my "How I decided" column instead of just counting it as a clean pass.

<!-- ── Stretch features ─────────────────────────────────────────────────────
Doing one? Say so here BEFORE you start. A feature this README never
claims earns nothing.
───────────────────────────────────────────────────────────────────────── -->

---

# Week 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
week 1 — the point is that someone can see what you said before you knew
how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
writes it all into results/ for you. Targets come from criteria.md; the
verdict column is your call.

Criterion 3 is measured in one deterministic pass rather than three, so
the same number goes in all three run columns. That's correct, not lazy.

Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 4 of 5 | 4 of 5 | 4 of 5 | MET |
| 2. Every answer names a source | 5 of 5 | 3 of 3 answered | 3 of 3 answered | 3 of 3 answered | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 4. No chunk in a sample of 10 is under 150 characters or cuts off mid-sentence | 10 of 10 | 10 of 10 | 10 of 10 | 10 of 10 | MET |
| 5. Every named source contains the fact used in the answer | 4 of 5 | 3 of 5 | 3 of 5 | 3 of 5 | MISSED |

<!-- Underneath, paste the REAL output for each criterion from one of your
runs — the actual text your system produced, not a description of it.
Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
week — not a new one. Plus a sentence on how you decided. That sentence
matters most where it was close.

If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
The target has to hold, not show up occasionally.

Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunk contains the answer | MET | 4 of 5, exactly at the target. Math, dining, parking and laundry each retrieved a file someone could answer from. Walking did not, since transit_walking.txt only lists walking times between campus buildings. Parking and laundry are close calls: the parking file never mentions commuters, and the laundry files give busy times rather than a way to check availability, so a stricter reading would make this MISSED. |
| 2 | Every answer names a source | MET | All 3 answers the system produced named source files. The 2 refusals returned no answer, so there was nothing to cite. |
| 3 | Gate stops out-of-corpus questions | MET | Refused 5 of 5, with distances 0.825 to 0.934, all above the 0.6 cutoff. Target was 4 of 5. |
| 4 | No chunk in a sample of 10 is under 150 characters or cuts off mid-sentence | MET | All 10 sampled chunks passed. The smallest file in the whole corpus is 183 characters and every post fits in one chunk, so nothing is split or cut off. |
| 5 | Every named source contains the fact used in the answer | MISSED | Only 3 of 5 questions produced a sourced answer, so 4 of 5 was out of reach. Parking (best distance 0.682) and walking (0.674) were refused by the gate. The 3 answered questions were clean: every cited dining, math and laundry file contained the fact used. |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
enough — you need the mechanism.

Not a diagnosis: "Question 3 didn't work."
A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                  one sentence that got split across two chunks, so
                  neither chunk on its own contains it."

The five stages: loading → chunking → embedding → retrieval → generation.

Look for a pattern. If three misses all ask about numbers, that's one
problem, not three.

Missed nothing? Say so, then say honestly whether your targets were set
low, and which one you'd tighten and to what.

Milestone 3. -->

### Criterion 5: every named source contains the fact used (MISSED, 3 of 5)

**Stage: retrieval (the relevance gate).** The sources the system cited were fine. The miss came from two questions that never got an answer because the gate refused them.

- **Parking:** "What parking pass should I get as a commuter?" retrieved `admin_parking_permits.txt` first, but the best distance was 0.6822, above the 0.6 cutoff, so the system returned "I don't have enough information about that." The mechanism is a wording mismatch. The post says "student permits" and "west lots," and my question said "commuter pass." I confirmed this with `python app.py retrieve`: "How do I get a student parking permit?" scored 0.528 and "When do west lot parking permits sell out?" scored 0.184, both under the cutoff, with the same file ranked first each time. The content was retrievable. Only the phrasing pushed it past the gate.
- **Walking:** "Where is a good easy place to walk for a beginner?" was refused at 0.6737. This is not a pipeline fault. `transit_walking.txt` only lists walking times between campus buildings, so the corpus has no answer and the gate was right.

**Pattern:** both refused questions were written in my own words and scored 0.67 to 0.68, while my Milestone 4 in-corpus questions scored 0.177 to 0.392. Math 220 (0.544) also passed with little room to spare. The 0.6 cutoff was tuned on questions that closely matched the post wording, so it is tight for paraphrased ones.

## The Improvement

**What I changed:** Raised the relevance gate cutoff in `config.py` from 0.6 to 0.7.

**Why I picked it:** My diagnosis on criterion 5 traced the miss to a wording mismatch at the gate, not retrieval — the parking question retrieved the right file (`admin_parking_permits.txt`) at 0.6822, just above the old 0.6 cutoff. 0.7 sits above that and below every out-of-scope question I've seen (0.764 and up), so it should let parking through without opening the gate to anything actually out of scope.

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
`python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 4 of 5 | 4 of 5 | 4 of 5 | MET |
| 2. Every answer names a source | 5 of 5 | 4 of 4 answered | 4 of 4 answered | 4 of 4 answered | MET |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | MET |
| 4. No chunk in a sample of 10 is under 150 characters or cuts off mid-sentence | 10 of 10 | 10 of 10 | 10 of 10 | 10 of 10 | MET |
| 5. Every named source contains the fact used in the answer | 4 of 5 | 4 of 5 | 3 of 5 | 4 of 5 | MISSED |

**Did it help?** Partly. The gate now lets parking through, and it retrieves the right chunk (`admin_parking_permits.txt`) every time — but the model only grounds its answer in it 2 of 3 runs. Run 1 and run 3 gave a real answer (west lot permits go on sale in August and sell out in about 3 days; the east lot never sells out). Run 2 said "I don't have enough information" even though the same chunk was retrieved and cited. So criterion 5 moved from a flat 3/5 in every run before, to 4, 3, 4 after — the gate fix worked, but it exposed a separate inconsistency at generation that a cutoff change can't touch.

Walking also passed the gate now (0.674, under 0.7), but the model correctly said it didn't have enough information in all 3 runs, since `transit_walking.txt` only lists walking times between buildings. No hallucination, which is what I was watching for.

Dining, math and laundry are unchanged (their distances, 0.33, 0.54 and 0.45, are all well clear of both cutoffs). Every out-of-scope question is still refused (lowest score 0.825), so the change cost nothing on criterion 3.

<!-- Say plainly whether it did, and how you know. If it made things worse,
say that — a change that backfired, honestly reported, earns full credit
and is more interesting than one that worked. What matters is that you can
tell.

Milestone 4. -->

## What's Still Broken

Criterion 5 is still missed (4, 3, 4 of 5 across the three after-runs). The gate is no longer the problem — `admin_parking_permits.txt` is retrieved correctly every time at 0.6822. What's left is generation: the model sometimes says "I don't have enough information" even when the retrieved chunk answers the question (parking run 2). That's a prompt or grounding-instruction issue, not a retrieval or cutoff issue, so raising the cutoff further wouldn't fix it and would risk letting a real out-of-scope question through instead (out-of-scope questions start at 0.825, so there's still room, but not unlimited room).

What I'd try next: tighten the generation prompt so it's told explicitly to use the top retrieved chunk if it's topically related, rather than defaulting to a refusal when the connection isn't a word-for-word match. I stopped here because that's a prompt-engineering change, not a config change, and I wanted to isolate this unit's improvement to the one thing my diagnosis pointed at (the gate) rather than changing two things at once and losing the ability to tell which one moved the numbers.

## What I'd Do Differently

I'd rewrite criterion 5. Right now it's "every named source contains the fact used, in at least 4 of 5 questions," but a refused question can never satisfy it, so the real ceiling was 3 of 5 until the gate changed — the target didn't account for the gate as a separate failure mode from citation accuracy. I'd split it into two criteria next time: one for the gate (does it correctly pass questions the corpus can answer) and one purely for citation accuracy on questions that do get answered. That would have made this unit's diagnosis land faster instead of me needing to work out that two different mechanisms were both landing on the same criterion.

I'd also tighten criterion 4. A 150-character floor turned out to be nearly unfailable once I saw that every post in the corpus is single-chunked and the smallest file is 183 characters — it never had a real chance to catch anything. Next time I'd set it against something the chunker could actually get wrong, like requiring a specific fraction of chunks to be full posts rather than fragments.