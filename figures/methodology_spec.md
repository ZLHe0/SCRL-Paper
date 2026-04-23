# Figure Specification: SCRL Methodology (Main Method Figure)

## Reference
Inspired by the INSPO main figure (docs/papers/INSPO/figs/main2.pdf), which shows:
- Left panel: individual rollouts as colored blocks (I+Q pairs → Policy Model → R colored by reward), with explicit instruction sampling
- Right panel: experience-driven instruction generation with a verification sub-panel and self-reflection sub-panel
- Clean two-panel layout with numbered headers (①, ②)

Our figure follows the same spirit but adapts for SCRL's key differences:
- We split **within a group of rollouts from the same task** (not across different instructions/tasks)
- We use **Thompson Sampling** to allocate rollouts to V_old vs V_new (not softmax-weighted instruction sampling)
- Our skill creator has **assertion grading → analyzer → analyst → author** (not self-reflection on instruction-trajectory pairs)

## Overall Layout and Style

**Target venue**: NeurIPS 2026. Clean, publication-ready.

**Dimensions**: Full-width `figure*`. Approximately 2.2:1 aspect ratio (wide). ~2100 x 950 px at 300 DPI.

**Layout**: Two panels, left and right, with numbered circled headers like INSPO.
- **Left panel** (≈60% width): **"① RL Training with Within-Group Skill Testing"**
- **Right panel** (≈40% width): **"② Skill Creator Pipeline"**

**Color palette**:
- V_old rollouts: **blue tones** (#2166AC fill, lighter #DCEAF7 bg)
- V_new rollouts: **orange/coral tones** (#E65100 fill, lighter #FDE8D0 bg)
- Reward = 1 (success): **green** (#2E7D32 or #4CAF50)
- Reward = 0 (failure): **red/pink** (#C62828 or #EF5350)
- Policy model: **gray** (#757575 border, white fill)
- Skill creator components: **purple tones** (#7B1FA2 for analyzer, lighter for others)
- Thompson Sampling: **teal/dark cyan** (#00796B)
- Arrows: dark gray (#555), minimal count
- Background: white

**Typography**: Sans-serif (Helvetica). Panel headers bold 11-12pt with circled numbers. Box labels 9pt. Annotations 7-8pt italic.

---

## Left Panel: RL Training with Within-Group Skill Testing (~60% width)

### Description
This panel shows one GRPO training step. A single task Q is sampled, and G rollouts are generated. The key visual: the rollouts are explicitly split between V_old (blue) and V_new (orange) via Thompson Sampling. Each rollout produces a reward (green=success, red=failure). The rewards feed three destinations: policy update, Thompson Sampling posterior update, and failure collection.

### Layout (left to right flow, similar to INSPO's left panel)

**Column 1: Task + Skill Version Assignment (leftmost)**

Show a single task box at the top:
- **"Q"** — a rounded gray box representing the sampled task

Below the task, show G individual rollout slots (e.g., G=8 for visual clarity, arranged vertically). Each slot shows which skill version is assigned. Use the INSPO style of paired blocks:

```
  V_old  Q     ← rollout 1 (blue V_old tag + gray Q)
  V_new  Q     ← rollout 2 (orange V_new tag + gray Q)  
  V_old  Q     ← rollout 3
  V_old  Q     ← rollout 4
  V_new  Q     ← rollout 5
  V_old  Q     ← rollout 6
  V_new  Q     ← rollout 7
  V_old  Q     ← rollout 8
```

The V_old/V_new tags should be visually prominent colored blocks (blue vs orange), similar to how INSPO shows I1, I2, etc. as colored instruction tags paired with Q1, Q2, etc.

Important: the Q is the SAME for all rollouts (they all share the same task). This is visually conveyed by all Q blocks being identical (same color, same label "Q"). The ONLY difference is the V_old/V_new tag. This is the key distinction from INSPO where each rollout has a different I+Q.

Below the rollout column, show the sampling mechanism:
- A small annotation or formula: **"V ~ Thompson Sampling(α, β)"**
- Or a small box labeled **"Thompson Sampling"** with a teal border, with an arrow pointing to the V_old/V_new assignments
- Show the probability: e.g., "P(V_new) = clamp(P(p_new > p_old), ε, 1-ε)" in small text

**Column 2: Policy Model + Tool Engine (center)**

Similar to INSPO's center column:
- A large rounded box: **"Policy Model πθ"** with a gear/brain icon
- Above or overlapping: **"Tool Engine"** box with bidirectional arrows (a ↑↓ o) showing action-observation loop
- The G rollout pairs from Column 1 feed into this box (arrows from left)
- The box produces G trajectories + rewards on the right

**Column 3: Trajectories + Rewards (right side of left panel)**

Show G output blocks, each consisting of a trajectory indicator and a reward badge:

```
  T1  R=1  ✓    (green reward badge)
  T2  R=0  ✗    (red reward badge)
  T3  R=1  ✓    
  T4  R=0  ✗    
  T5  R=1  ✓    
  T6  R=0  ✗    
  T7  R=0  ✗    
  T8  R=1  ✓    
```

Each trajectory block should retain the color of its skill version (blue border for V_old trajectories, orange border for V_new trajectories) so the reader can trace which version produced which outcome.

The reward badges (green ✓ / red ✗) should be prominent, similar to INSPO's R1-R6 colored blocks.

**Below the columns: Three output flows (the triple role)**

From the trajectories+rewards column, show three flows going downward or branching:

**Flow ① Policy Update** (orange accent):
- Label: **"Policy Update"**
- Brief annotation: "All G rewards → GRPO advantage → update θ"
- Arrow from all rewards into a "Policy Update" box
- A curved arrow from Policy Update back to the Policy Model (the training loop)

**Flow ② Failure Collection** (red accent):
- Label: **"Failure Reservoir"**
- Brief annotation: "Failed trajectories (R=0) → bounded reservoir R"
- Arrow from the red (R=0) trajectory blocks to a small cylinder/stack icon
- An arrow from the reservoir going to the right panel (Skill Creator Pipeline)

**Flow ③ Posterior Update** (teal accent):
- Label: **"Thompson Sampling Update"**
- Brief annotation: "Per-version rewards → update Beta posteriors"
- Arrow from rewards to a small teal box showing:
  - "V_old: Beta(α_old, β_old)" 
  - "V_new: Beta(α_new, β_new)"
- A curved arrow from this box back up to the Thompson Sampling assignment mechanism in Column 1 (closing the bandit loop)

These three flows should be compact — one line each below the main diagram, or arranged as a small sub-region. Do NOT use many arrows. Use spatial grouping: the three destinations can be three small boxes in a row below the trajectories, each receiving from the appropriate subset.

### Key visual features of the left panel
1. **Same Q for all rollouts** — all Q blocks are identical, making it obvious this is within-group
2. **Color-coded version split** — blue V_old vs orange V_new is immediately visible
3. **Reward colors** — green success vs red failure on each trajectory
4. **Three labeled flows** — ①②③ clearly marking the triple role, but kept compact

---

## Right Panel: Skill Creator Pipeline (~40% width)

### Description
This panel shows what happens every K_e steps: the skill creator pipeline takes failed trajectories from the reservoir and proposes a new skill version. The flow is: failure reservoir → assertion grading → LLM analyzer → version analyst → version author → V_new candidate. This runs periodically (not every step) and produces the V_new that gets tested in the left panel.

### Layout (top to bottom flow)

**Input: Failure Reservoir** (top of right panel)
- The same cylinder/stack icon from Flow ② of the left panel (visually connected)
- Label: **"Failure Reservoir R"**
- Shows a few stacked trajectory snippets inside (T2, T4, T6, T7 — the failed ones from the left panel, with red borders)

**Stage 1: Assertion Grading** (rule-based, no LLM)
- A box with a subtle border
- Label: **"Assertion Grader"**
- Sub-label: "Rule-based checks on trajectories"
- Inside or beside: show 2-3 small assertion cards:
  - "φ₁: action_exists('search') → 73%"
  - "φ₂: action_before('answer','search') → 45%"
- Small badge: **"No LLM"** (to distinguish from the LLM stages below)
- Output arrow down labeled: "Pass rates + trajectory sample"

**Stage 2: Failure Analyzer** (LLM)
- A box with purple border (#7B1FA2)
- Label: **"Failure Analyzer (LLM)"**
- Sub-label: "Diagnose failure patterns, evolve assertions, select episodes"
- Inside: a brief representation like "Pattern 1: 42% fail to verify... Pattern 2: 28% wrong entity..."
- Small badge: **"1 LLM call"**
- Output arrow down labeled: "Failure diagnosis + selected episodes"

**Stage 3: Version Analyst → Author** (LLM, two steps)
- Two connected boxes or one box split into two halves:
  - Left half: **"Version Analyst"** — "Decides WHAT to change (add/modify/delete)"
  - Right half: **"Version Author"** — "Writes HOW (skill content + triggers)"
- Purple border, slightly different shade for each
- Small badge: **"2 LLM calls"**
- Inside the analyst half: show example operations like "+add: verify-entity-role", "~modify: search-first", "-delete: persist-through-difficulties"
- Output arrow down

**Output: Candidate Version V_new** (bottom of right panel)
- A box in orange (matching the V_new color from the left panel)
- Label: **"V_new"**
- Inside: show 2-3 skill cards with names:
  - "S₁: search-first-for-facts"
  - "S₂: verify-entity-role-match"  
  - "S₃: require-multi-step-verification"
- Each skill card has a small trigger badge: "trigger: every_step" or "trigger: regex(search)"

**Connection back to left panel:**
- A curved arrow from V_new going left back to the Thompson Sampling assignment area in the left panel
- Label: **"Test V_new vs V_old"**
- This closes the full loop: left panel tests → right panel creates → left panel tests again

### Key visual features of the right panel
1. **Pipeline flow** — clear top-to-bottom progression through 3 stages
2. **LLM vs rule-based** — badges distinguish which stages use LLMs (3 total calls)
3. **Concrete examples** — assertion pass rates, failure patterns, skill operations make it tangible
4. **Color match** — V_new output matches the orange V_new blocks in the left panel

---

## Cross-Panel Connections (keep minimal)

Only TWO arrows cross between panels:
1. **Left → Right**: Failed trajectories from the reservoir (Flow ②) feed into the right panel's Failure Reservoir input
2. **Right → Left**: The V_new candidate output feeds back into the left panel's Thompson Sampling assignment

That's it. No other cross-panel arrows. The connection is clear from these two plus the matching colors.

---

## Key Differences from INSPO Figure

| Aspect | INSPO | SCRL (Ours) |
|---|---|---|
| Left column shows | Different instructions I1-I6 paired with different questions Q1-Q6 | Same question Q paired with different versions V_old/V_new |
| Sampling mechanism | I ~ p(·\|W; P) softmax over instruction population | V ~ Thompson Sampling(α, β) from Beta posteriors |
| Right panel | Self-reflection on instruction+trajectory pairs → new instructions | Assertion grading → LLM analyzer → analyst → author → new version |
| Verification | Separate verification panel with additional rollouts (Ins+Q → Tool → Evaluate) | NO separate verification — testing happens within training rollouts (left panel) |
| Key visual cue | Colored instruction blocks I1-I6 (different colors per instruction) | Two-color split: blue V_old vs orange V_new across all rollouts |

---

## Additional Design Notes

1. **Arrow discipline**: Left panel should have at most 6-7 arrows total (inputs → model → outputs, plus 3 compact flows below). Right panel should have 4-5 arrows (top-to-bottom pipeline plus one back to left). Cross-panel: exactly 2 arrows. Total figure: ~13-14 arrows maximum.

2. **The rollout split is the star**: The most eye-catching element should be the G rollout slots in Column 1 with their blue/orange version tags. This is what makes SCRL different — the reader should immediately see "oh, the rollouts are split between two skill versions."

3. **Compactness**: The INSPO figure fits a lot of detail cleanly. Ours should too, but the right panel can be more compact since our pipeline has fewer stages than INSPO's verification+self-reflection.

4. **Annotations at the bottom**: Below the entire figure, optionally include a one-line annotation: "Every K_e steps: right panel proposes V_new → left panel tests it → accept or reject → repeat"

5. **No equations in the figure** except the Thompson Sampling formula "V ~ TS(α, β)" and optionally "P(V_new) = clamp(..., ε, 1-ε)" in small text.

6. **Size**: NeurIPS `figure*`, full text width (~6.5 inches), height ~3.2 inches.
