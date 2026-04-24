# Figure Specification: SCRL Methodology (Main Method Figure) — Revised

## Reference
Inspired by the INSPO main figure style. See trail_1.png for the first attempt — this revision addresses:
1. Right panel too detailed — combine assertion grader + failure analyzer into one stage, keep only two components (Analysis + Creation)
2. Thompson Sampling positioning wrong — it should connect rewards to the NEXT round's version assignment, not float separately
3. Too many formulas/annotations — reduce to a few key math notations placed naturally
4. Arrow clutter — fewer arrows, better spatial arrangement of the three output flows
5. Policy Update block misplaced — rearrange so the three flows (policy update, failure collection, TS update) make spatial sense

## Overall Layout and Style

**Target venue**: NeurIPS 2026. Clean, publication-ready.

**Dimensions**: Full-width `figure*`. ~2.2:1 aspect ratio. ~2100 x 950 px at 300 DPI.

**Layout**: Two panels, left (~60%) and right (~40%), with numbered circled headers.
- **Left panel**: **"① RL Training with Within-Group Skill Testing"**
- **Right panel**: **"② Skill Creator Pipeline"**

**Color palette**:
- V_old rollouts: **blue** (#2166AC fill, #DCEAF7 light)
- V_new rollouts: **orange** (#E65100 fill, #FDE8D0 light)
- Success reward (R=1): **green badge** (#4CAF50)
- Failure reward (R=0): **red badge** (#EF5350)
- Policy model: gray (#757575 border, white fill)
- Skill creator components: purple tones (#7B1FA2)
- Thompson Sampling: teal (#00796B)
- Background: white. No gradients, shadows, or 3D.

**Typography**: Sans-serif. Panel headers bold ~11pt. Box labels ~9pt. Sparse annotations ~7-8pt.

**Arrow discipline**: Entire figure should have ~10-12 arrows total. Use spatial proximity and color zones instead of arrows where possible.

---

## Left Panel: RL Training with Within-Group Skill Testing (~60%)

### Main area: Three columns (left to right)

**Column 1: Task + Version Assignment**

A single task box at top: **"Q"** (gray rounded box).

Below it, G=8 rollout slots arranged vertically. Each slot is a small colored tag showing which version is assigned:

```
  V_old  Q
  V_new  Q
  V_old  Q
  V_old  Q
  V_new  Q
  V_old  Q
  V_new  Q
  V_old  Q
```

Blue tags for V_old, orange tags for V_new. All Q blocks are identical (same task). The version split should be visually prominent — this is the key element.

At the bottom of this column, a small teal annotation: **"V ~ TS(α, β)"** — just this, no full formula.

**Column 2: Policy Model**

Center box: **"Policy Model πθ"** with a gear/brain icon and **"Tool Engine"** shown as a sub-element (compact, like the first attempt). Arrows from Column 1 rollouts feed in, arrows out to Column 3.

**Column 3: Trajectories + Rewards**

G output rows, each showing: trajectory label (T1...T8) + reward badge (green ✓ for R=1, red ✗ for R=0). Each row retains the blue/orange border of its version so the reader traces version → outcome.

### Below the three columns: Three output flows

Arrange these as **three compact boxes in a horizontal row** beneath the main diagram, each receiving from the trajectories above via a SHORT downward arrow. This avoids the mess of long crossing arrows.

**Left box: "Policy Update"** (orange accent)
- Receives: all rewards
- Annotation: "GRPO advantage → update θ"
- A curved return arrow going back up to Policy Model (the training loop)

**Center box: "Failure Reservoir"** (red accent)
- Receives: failed trajectories only (R=0 ones)
- Shows: small stack icon with "R" label
- Annotation: "bounded reservoir"
- An arrow going RIGHT toward the right panel's input (this is one of the two cross-panel arrows)

**Right box: "Thompson Sampling Update"** (teal accent)
- Receives: per-version reward counts
- Shows: "update β posteriors"
- A curved return arrow going back UP to Column 1's version assignment area
- This is the KEY connection: TS update → next round's sampling. The arrow should clearly go from this box back to the top of Column 1 where V_old/V_new assignments are made.

### Key: Thompson Sampling as the bridge between rewards and next assignment

The flow should read clearly as: Rewards (Column 3) → TS Update (bottom-right box) → Version Assignment (top of Column 1, next step). This is a LOOP. The spatial arrangement should make this loop obvious without too many arrows — TS Update sits at bottom-right, and its return arrow curves up to Column 1 at top-left.

---

## Right Panel: Skill Creator Pipeline (~40%)

### SIMPLIFIED to two main stages (not four)

**Input: Failure Reservoir** (top)
- Same cylinder/stack icon, connected from the left panel
- Shows 3-4 failed trajectory snippets (T2, T4, T6, T7 with red borders)

**Stage 1: "Assertion-Based Failure Analysis"** (combines old assertion grader + failure analyzer)
- ONE box with purple border
- Label: **"Failure Analysis"**
- Sub-elements inside (compact, one line each):
  - "Rule-based assertions: action_exists(search) → 73%, action_before(answer,search) → 45%"
  - "LLM diagnoses failure patterns, selects representative episodes"
- This is one unified stage — the grader feeds directly into the analyzer, no need to separate them visually
- Output arrow down

**Stage 2: "Skill Version Creation"** (combines analyst + author)
- ONE box with purple border (slightly different shade)
- Label: **"Skill Version Creator"**
- Sub-elements inside (compact):
  - Example operations: "+add: verify-entity-role, ~modify: search-first, -delete: persist-difficulties"
  - "Analyst decides WHAT → Author writes HOW"
- Output arrow down

**Output: Candidate V_new** (bottom)
- Orange box matching the V_new color from the left panel
- Shows 2-3 skill cards: "S₁: search-first", "S₂: verify-role-match"
- Each with a tiny trigger badge

**Connection back to left panel:**
- Arrow from V_new going left back to the version assignment area in the left panel
- Label: **"Test V_new vs V_old"**

---

## Cross-Panel Connections

Exactly TWO arrows between panels:
1. Left → Right: Failure Reservoir → Right panel input
2. Right → Left: V_new candidate → Left panel version assignment

---

## What to REMOVE vs the first attempt (trail_1.png)

1. **Remove**: Separate "Assertion Grader" and "Failure Analyzer" boxes → merge into one "Failure Analysis" box
2. **Remove**: Detailed formulas like "P(V_new) = clamp(P(p_new > p_old), ε, 1-ε)" → just "V ~ TS(α, β)"
3. **Remove**: Separate "Version Analyst (LLM)" and "Version Author (LLM)" boxes → merge into one "Skill Version Creator" box
4. **Remove**: Long text annotations → keep only short key phrases
5. **Fix**: Thompson Sampling position — it should sit at the bottom of the left panel receiving rewards, with a clear return arrow to the version assignment at the top
6. **Fix**: Policy Update — should be one of three compact boxes in a row, not a floating block
7. **Keep**: The assertion percentage examples (73%, 45%) — these are concrete and interesting
8. **Keep**: The operation examples (+add, ~modify, -delete) — these show what the pipeline produces
9. **Keep**: The color-coded V_old/V_new rollout split — this is the star of the figure

## Size
NeurIPS `figure*`, full text width (~6.5 inches), height ~3 inches.
