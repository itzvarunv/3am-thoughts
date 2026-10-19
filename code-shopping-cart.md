# The Code Shopping Cart

## The idea

What if building software felt like shopping on Amazon?

Instead of asking an AI to write an entire app from scratch (and getting long, messy, half-hallucinated code), you browse a store of verified, pre-tested code blocks. You pick your stack, search for what you need, compare options, and drop blocks into a cart. When you check out, an AI does not write the code. It only stitches the blocks together. The actual code that ships is code that was already tested.

## How it develops

**1. Pick a stack**
Language and framework first (Python + Flask, C++ + ROS, and so on). The store only shows blocks compatible with that stack.

**2. Browse and benchmark**
Every block is a self-contained unit (an auth system, a PID controller, a CSV parser, a rate limiter) with:
- a clear interface (inputs, outputs, types)
- tests that pass
- benchmarks (speed, memory) so you can compare two blocks that do the same job

**3. Add to cart**
You collect the blocks your project needs. The cart is basically a dependency list with intent.

**4. The AI is only the glue**
This is the core rule. The AI never outputs raw code. It outputs a structured blueprint (JSON / AST-level routing): which block's output feeds which block's input, in what order, under what conditions. That is a much smaller, much more checkable job than writing an app from scratch, so there is far less room to hallucinate.

**5. Zero-hallucination build**
The builder takes the blueprint, pulls the real verified source for each block, wires them according to the blueprint, and compiles. If the blueprint references something that does not exist or connects incompatible types, the build fails loudly instead of silently producing garbage.

**6. Self-improving catalogue**
When a project needs a function the store does not have, that gap gets written, tested, benchmarked, and, if it passes, listed in the catalogue. Every project makes the store bigger.

## Where it breaks

- **"Verified" is doing a lot of work.** Verified by whom, against what spec? Tests only prove what they test. A block can pass every test and still be wrong for your use case.
- **Interface hell.** Blocks written by different people make different assumptions (error handling, data formats, threading, state). Stitching them is exactly where real bugs live, and that is the part the AI is doing.
- **Not everything is a block.** Glue logic, business rules, and the weird parts unique to your project do not exist in any catalogue. Those still have to be written, and that is usually the hard part.
- **Versioning.** A block updates, and every project using it can break. Now you need dependency pinning, which is basically reinventing a package manager.
- **Incentives.** Why would anyone upload well-tested blocks? Needs some reason for contributors to care.

## What it's secretly close to

- **Package managers** (pip, npm, cargo): a catalogue of reusable code, minus the "AI wires it for you" part
- **Component-based software engineering**: an old idea from the 90s that software should be assembled from standard parts like hardware
- **Program synthesis**: generating programs from specifications, an active research field
- **Low-code / no-code platforms**: drag blocks, connect them, ship
- **Visual programming** (LabVIEW, Simulink, Node-RED): blocks with typed ports wired together, which is exactly the blueprint layer

The new part is the combination: a benchmarked marketplace plus an AI that is restricted to wiring, never writing. That restriction is the whole point.

## Status

Raw. Needs skills in compilers/ASTs, sandboxing, and benchmarking before attempting a V1. A realistic first prototype: a tiny catalogue of about 10 Python blocks, a JSON blueprint format, and a builder that wires them. No AI at first, just hand-written blueprints to prove the wiring works.
