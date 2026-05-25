# Optimal Menu Depth: An Empirical UX Study on Hierarchical Navigation

A small HCI study asking a question with no obvious answer: when you've got a lot of menu items, are users better off seeing them all at once, or having them tucked behind nested categories? And does the answer change as the menu gets bigger?

I built the experimental apparatus from scratch in Processing, ran trials across five menu configurations, and looked at how depth interacts with menu size. Short version: depth helps more than I expected for large menus, but the benefit has a clear ceiling — and small menus get almost nothing out of it.

## The question

The classic information architecture trade-off: wide vs. deep. A flat menu shows everything but creates visual clutter; a deep menu cleans up the surface but adds clicks and navigation overhead. Most of the existing guidance assumes a fixed menu size. I wanted to know how the answer shifts when the menu itself grows.

So I tested five conditions, varying both size and depth:

| Condition | Item count | Structure |
|-----------|-----------|-----------|
| 1 | 30 | Flat |
| 2 | 30 | 2 levels of nesting |
| 3 | 60 | Flat |
| 4 | 60 | 2 levels of nesting |
| 5 | 60 | 3+ levels of nesting |

## How it works

The whole thing runs as an experiment state machine in Processing. Each participant moves through all five conditions sequentially, doing 20 trials per condition. A prompt in the corner tells them which item to click. For each trial the system logs:

- Target ID
- Elapsed time (ms)
- Number of errors before the correct click

After each condition, the participant gives an ease-of-use rating from 0–10.

A few design decisions worth calling out:

- **Click-to-expand, not hover-to-expand.** I started with hover — faster and more fluid — but it generated false-click errors that contaminated the data. Switched to click and the signal got much cleaner. Worth losing a bit of interaction smoothness for a measurable variable.
- **Random sampling without replacement.** Each trial target comes from the leaf nodes only, and no item repeats within a condition. Keeps comparisons fair across sessions.
- **Within-subjects design.** Every participant sees every condition, so individual differences in speed wash out across the analysis.

The code is split into four classes — `Menu`, `MenuItem`, `Prompt`, and `UserSat` — driven by a `Stage` enum that walks the participant through instructions, trial blocks, and satisfaction prompts. Each `MenuItem` owns its child `Menu`, which is what makes arbitrary nesting depth work without special-casing each level.

## What I found

Three participants, so pilot-scale — keep that in mind for everything below. With that caveat:

- **Big menus benefit more from depth than small ones.** Going from flat to two-level nesting cut search time noticeably for the 60-item menu, but barely moved it for the 30-item menu.
- **Depth has a ceiling.** The three-level version of the 60-item menu (condition 5) performed *worse* than the two-level version (condition 4), even though both beat flat. People got overwhelmed somewhere around the third tier.
- **Subjective ease-of-use mirrored performance only at the larger size.** For 60 items, users *felt* the nesting helped. For 30 items, they didn't notice a difference either way — which makes sense; if the flat version isn't painful, the nested one isn't a relief.

The overall takeaway is that there's no universal "right" depth. Depth should scale with size. Small menus stay flat. Big menus benefit from a layer of organization. Nobody wants three.

## Limitations

Being upfront about what this study can and can't claim:

- **n = 3.** This is pilot data, not a publishable result. The directions are interesting but the magnitudes aren't reliable.
- **Items were image-editor commands** (Cut, Paste, Crop, Brightness Adjustment, etc.), which gave participants real-world category intuitions. That helps realism but means the findings don't necessarily transfer to abstract item sets.
- **One nesting style.** I tested vertical-column, click-to-expand menus. Cascading hover menus, mega-menus, and search-augmented menus would all behave differently.

## Running it

You'll need [Processing](https://processing.org/download).

1. Clone this repo
2. Open `src/main.pde` in Processing
3. Hit Run

The console logs trial data as you go. Press Enter at the instructions screen to start, then follow the prompts.

## Repo layout

```
.
├── docs/
│   ├── menu-depth-study-final-report.pdf   ← full methodology and results
│   ├── prototype-implementation.pdf         ← build notes on the menu system
│   └── experiment-instrumentation.pdf       ← trial logic, randomization, logging
├── src/
│   ├── main.pde                             ← entry point + state machine
│   └── sketch.properties
└── README.md
```

---

Originally a coursework project; kept polished here as a portfolio piece. Built in Processing (Java mode).
