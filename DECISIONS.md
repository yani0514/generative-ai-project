# Decisions

# Week 1: the stack, the first call, and what it costs

## Week 1

**Run conditions.** Everything below was produced on:

- machine: ASUS ROG Strix G16 G614JVR, Intel(R) Core(TM) i9-14900HX, 32 GB RAM
- model: qwen3:4b-instruct
- served by: Ollama, one request at a time, locally
- date: 2026-10-02

Every number in this file is meaningless without those four lines, so they
are stated once here and referred to rather than repeated.

### 1. Machine and model set

I am running the required plus optional model set.

The installed models are:

- `qwen3:4b-instruct`
- `nomic-embed-text`
- `qwen2.5:7b`
- `qwen3-vl:4b`

The optional models are already available locally, so no additional model setup is required before week 9.

### 2. The first call

| | |
| --- | --- |
| finish reason | `stop` |
| prompt tokens | 24 |
| completion tokens | 42 |
| elapsed | 6.26 s |

The finish reason was `stop`, meaning the model ended normally. If it had returned a truncation/length-related finish reason instead, the program should treat the output as incomplete rather than as a valid short answer.

### 3. Variance

| cell | distinct (recording) | distinct (mine) | median latency |
| --- | ---: | ---: | ---: |
| closed_short, t=0.0 | 1/12 | 1/6 | 0.06 s |
| closed_short, t=1.0 | 1/12 | 1/6 | 0.06 s |
| open_list, t=0.0 | 1/12 | 1/6 | 0.57 s |
| open_list, t=1.0 | 11/12 | 6/6 | 0.54 s |

How many distinct answers did you get? Does your machine agree with the recording?
-At temperature 0, both the `closed_short` and `open_list` prompts produced one distinct answer on my machine. This agrees with the recording.

Which cell still returns a single answer at temperature 1.0, and why that one:
-The `closed_short` cell still returned a single distinct answer at temperature 1.0. The prompt is highly constrained because it asks for the capital of Luxembourg in one word, so there is very little room for a valid alternative response.

Which cells a test asserting exact string equality would pass on, and what that tells me about testing this system:
-An exact-string equality test would pass for `closed_short, t=0.0`, `closed_short, t=1.0`, and `open_list, t=0.0` in my run. It would fail for `open_list, t=1.0`, where all 6 responses were different strings. This shows that exact string equality can work for tightly constrained outputs, but it is not reliable for open-ended generation where several differently worded answers may still be valid.


**The sentence that carries into week 10.** Exact output repetition can be relied on more for tightly constrained prompts, especially at low temperature, but it should not be assumed for open-ended generation where multiple valid outputs are possible.

### 4. The cold start

- cold call: [ ] s
- warm call: [ ] s
- ratio: [ ]

What this implies for a system that uses more than one model, and what I
will do about it:

[...]

### 5. Cost, estimated

A 200-case golden set, at the token cost of my long case:

| | one run | nightly for the semester |
| small tier | | |
| large tier | | |

Estimates against the price list dated [date in `project/prices.py`], not
measurements. Running locally, my actual monetary cost was zero.

Which tier I would run nightly, which I would run before a release, and why
not the same one for both:

[...]

### Deferred

[Anything you did not get to, and why. An explicit deferral with a reason is
engineering. Silence is not, and the project rubric can tell the
difference.]
