# The Cross-Domain Connection Finder

## The idea

AI already "knows" A is true and B is true. What it almost never does is notice that A and B are secretly the same thing, the way Claude Shannon noticed that Boole's algebra and relay circuits are the same thing. That one connection basically gave us digital logic.

So instead of hoping a model has that insight unprompted, build a process that does it on purpose: take one idea, go knock on the door of every other idea and ask, "are you actually related to me?" This isn't really about LLMs. It's about all the stuff written on the internet that is genuinely connected but has never been linked by anyone.

## How it develops

**First version (bad): random prompts.** Have a model throw out random pairs and see what sticks. It dies fast. If the model knows N concepts (N = number of distinct ideas), there are about N²/2 possible pairs. With N in the millions, that's trillions of pairs, and nearly all of them are garbage (Bernoulli + knitting). Generating pairs was never the hard part. Recognising the good one is.

**Second version: systematic pairing.** Pick topic A, then check A against everything else, one by one. Better, because it's a search instead of a lottery. But two problems show up:

1. **Embeddings can't do it.** Word vectors (king − man + woman = queen) only capture what already co-occurs in text. A connection nobody has written down doesn't co-occur anywhere, so similarity search can't find it by definition.
2. **An LLM judge says yes to everything.** Ask "are A and B related?" and a model can spin a convincing story for almost any pair. Without a strict test, you get millions of plausible-sounding false positives.

**Third version (workable): compare structure, not topic.** Real hidden connections almost never share topic words. They share *shape*:
- Shannon: logic and relays are both two-valued things combined with AND/OR.
- A mass–spring–damper and an RLC circuit obey the same second-order differential equation.
- Heat diffusion, particle diffusion and the Black–Scholes option-pricing model are essentially the same PDE.

So the pipeline becomes:
1. **Extract** for each idea / paper: its *purpose* (what problem it solves), its *mechanism* (how), and its underlying *maths* (equation form, constraints).
2. **Match** across fields on similar purpose with a *different* mechanism, or on the same equation form.
3. **Judge** only the shortlist with an LLM, using a strict definition of "related": *does a method from A solve a known problem in B?*
4. **Human check** on the final few.

## Where it breaks

- **The judge is the bottleneck.** Generating candidates is cheap; verifying them isn't. Where there's an automatic checker (code runs, maths verifies), this works. Where "is this useful?" needs taste, it still needs a human.
- **Extraction quality.** If step 1 pulls out the wrong purpose/mechanism, step 2 matches on noise.
- **Novel ≠ unlikely.** Turning up a model's randomness (temperature) gives noise, not insight. The structure-matching has to do the work, not the sampling.

## What it's secretly close to

- **Literature-based discovery (Don Swanson, 1980s):** paper set 1 says A → B, paper set 2 says B → C, nobody linked A → C. Swanson found that fish oil could help Raynaud's disease this way, and it was later confirmed. That's exactly "knows A, knows B, never does A & B", before LLMs existed.
- **Analogy mining (Hope, Chan, Kittur & Shahaf, KDD 2017):** represent each idea as purpose + mechanism, search for similar purpose with a different mechanism. Basically step 1–2 above.
- **FunSearch (DeepMind, 2023) and AlphaEvolve (2025):** an LLM generates candidates, an automatic evaluator scores them, the best get mutated again. They found new results in maths and algorithms. It's the generator + verifier loop, which only works because the verifier is automatic.
- **Genetic algorithms (like my evolutionary trading bot):** mutation gives variety, selection gives progress. Without a fitness function, random variation goes nowhere. Same lesson.

## Next step

Build a small version over my own course material (mechanics, aerospace, electronics): extract purpose / mechanism / maths per topic, match across subjects, see whether it finds any connection I didn't already know. Planned for after Nov 26.
