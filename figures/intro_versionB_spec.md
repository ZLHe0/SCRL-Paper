# Figure Specification: Existing Skill Evolution vs SCRL Co-Evolution

## Overall Layout and Style

**Target venue**: NeurIPS 2026. Professional, clean, publication-ready academic figure.

**Dimensions**: Full-width figure spanning two columns. Aspect ratio approximately 2.2:1 (wide). Target resolution: 300 DPI, approximately 2100 x 950 pixels.

**Layout**: Two panels side-by-side, separated by a thin vertical divider or whitespace gap. Each panel occupies roughly half the width.
- **Left panel** title: "Existing: Skill Evolution Alongside RL"
- **Right panel** title: "SCRL: Skill-Policy Co-Evolution (Ours)"

**Core message**: Prior work DOES update skills during RL training — they are NOT frozen. They generate rollouts, build skills/memory from the outputs, and update the skill bank over time. BUT the key differences are: (1) the policy has no say in whether to accept or reject a skill change — there is no controlled testing with training signals feeding back into the skill selection; (2) skill testing requires additional rollouts beyond training. SCRL integrates skill testing INTO the training rollouts via within-group splitting, so the policy's own reward signals drive skill acceptance, with zero extra cost.

**Design philosophy**: MINIMAL arrows. Each panel should have at most 4-5 arrows total. Use spatial grouping, containment (boxes inside boxes), and color zones rather than arrows to show relationships. The figure should be parseable in 10 seconds.

**Color palette**: Clean, muted academic colors.
- Skill-related elements: **blue tones** (#2166AC borders, #DCEAF7 fills)
- RL/policy-related elements: **orange tones** (#E65100 borders, #FDE8D0 fills)
- Shared/neutral: light gray (#F7F7F7), dark gray text (#333333)
- Highlight for "what's missing" on the left: a subtle red dashed element or ✗ marker
- Highlight for "what's new" on the right: green (#2E7D32) accent
- Boxes: white fill with colored borders, slight rounded corners

**Typography**: Sans-serif (Helvetica or similar). Panel titles bold ~11pt. Box labels regular ~9pt. Annotations ~7-8pt italic. All legible at print size.

**Background**: Pure white. No gradients, shadows, or 3D effects.

---

## Left Panel: Existing Approaches (Skill Evolution Alongside RL)

### Description
This panel shows that prior work (SkillRL, Reflexion, ExpeL, MemRL, etc.) DOES evolve skills during training — they are not frozen. The RL training generates rollouts, skills/memory are built or updated from those rollout outputs, and the updated skills are injected back into the next round. HOWEVER, there is a key gap: the skill update is one-directional — the model outputs feed into skill creation, but the training reward signals do not feed back into the skill selection/acceptance process. The policy has no mechanism to evaluate or reject a skill change. If additional evaluation is needed, it requires separate rollouts outside the training loop.

### Visual Structure

Use a **simple two-row layout** with minimal arrows:

**Top row: RL Training Loop**
- A horizontal rounded rectangle spanning most of the panel width, orange-tinted
- Inside, from left to right: **"Policy πθ"** → **"Rollouts"** → **"Reward"** → **"Policy Update"**
- These can be shown as a compact inline flow or simply as one box labeled **"RL Training (GRPO)"** with "Policy πθ → Rollouts → Policy Update" written inside
- This is the standard training loop — it works fine on its own

**Bottom row: Skill Evolution**
- A horizontal rounded rectangle below, blue-tinted
- Label: **"Skill Creator / Memory Builder"**
- Inside or next to it: small icons representing the skill bank (2-3 skill cards S₁, S₂, S₃)

**Connections (keep to exactly 3 arrows):**
1. **Arrow down** from "Rollouts" (in the RL row) to the Skill Creator: labeled **"Model outputs / trajectories"** — this shows that rollout outputs feed skill creation. This arrow exists and is fair to prior work.
2. **Arrow up** from Skill Bank back to the RL Training row (entering at "Policy πθ" or "Rollouts"): labeled **"Updated skills injected"** — skills are fed back into the next round of rollouts. This arrow also exists.
3. **A MISSING arrow** (drawn as a dashed red line with ✗): from "Reward" down to the Skill Creator, labeled **"No training signal for skill selection"** — this is what's missing. The reward from training does not inform whether to accept or reject a skill change. The policy has no vote.

**Key annotation** (small text near the bottom):
- **"Skill updates are uncontrolled — no mechanism for the policy to accept or reject changes"**
- **"Additional rollouts needed for skill evaluation"**

### What this panel conveys
The reader sees: "Oh, these methods DO update skills during training (not frozen!), and rollout outputs feed back into skill creation. But there's a missing connection — the training rewards don't inform skill selection. The policy can't say 'this skill change made things worse, reject it.'"

---

## Right Panel: SCRL (Skill-Policy Co-Evolution)

### Description
This panel shows that SCRL closes the loop. The same GRPO rollouts that train the policy also evaluate skill versions — the reward signals directly drive skill acceptance/rejection. The key visual: the "missing arrow" from the left panel is now present and highlighted. Everything is unified.

### Visual Structure

Use the **same two-row layout** as the left panel (so the reader can directly compare), but with the gap closed:

**Top row: RL Training with Skill Testing**
- Same horizontal rounded rectangle, orange-tinted
- But now the "Rollouts" element is visually split: left half in one shade (V_old), right half in another shade (V_new) — showing the within-group version split
- Label the split: **"Within-group split: V_old | V_new"**
- The rest is the same: Reward → Policy Update

**Bottom row: Skill Creator + Thompson Sampling**
- Same horizontal rounded rectangle, blue-tinted
- Label: **"Skill Creator"** on the left portion, **"Thompson Sampling"** on the right portion (or as a connected sub-box)
- Thompson Sampling shows a small "Accept ✓ / Reject ✗" element

**Connections (keep to exactly 4 arrows):**
1. **Arrow down** from "Rollouts" to Skill Creator: labeled **"Failed trajectories"** — same as left panel (rollout outputs feed skill creation)
2. **Arrow up** from Skill Creator back to Rollouts: labeled **"Skill versions V_old, V_new"** — updated/candidate skills injected into rollouts
3. **Arrow down** from "Reward" to Thompson Sampling: labeled **"Reward signals"** — THIS IS THE KEY NEW ARROW. Drawn in green or with a highlight. This is the "missing arrow" from the left panel, now present. Training rewards directly inform skill selection.
4. **Arrow** from Thompson Sampling back to Skill Creator: labeled **"Accept / Reject"** — the evaluation result feeds back into the skill creation cycle

**Key annotation** (small text, highlighted in green):
- **"Training signals directly drive skill selection — zero additional rollouts"**
- **"Triple role: same rollouts update policy, collect failures, evaluate skills"**

### What this panel conveys
The reader sees: "Oh, the missing arrow is now connected! The reward signals that train the policy ALSO evaluate the skills. And the rollouts are split within each group for controlled comparison. Everything uses the same data."

---

## Visual Parallels and Contrasts

The two panels should be structurally identical in layout (same box positions, same row heights) so the reader can spot what changed:

| Element | Left (Existing) | Right (SCRL) |
|---|---|---|
| Top row | RL Training (standard rollouts) | RL Training (rollouts split V_old/V_new) |
| Bottom row | Skill Creator (uncontrolled updates) | Skill Creator + Thompson Sampling |
| Arrow: outputs → skills | ✓ Present (blue arrow) | ✓ Present (blue arrow) |
| Arrow: skills → rollouts | ✓ Present (blue arrow) | ✓ Present (blue arrow) |
| Arrow: reward → skill selection | ✗ MISSING (red dashed + ✗) | ✓ PRESENT (green, highlighted) |
| Arrow: accept/reject | Not applicable | ✓ Present (green arrow) |
| Rollout overhead | Additional rollouts for evaluation | Zero additional rollouts |

The strongest visual cue: the **dashed red ✗ arrow** on the left becomes a **solid green ✓ arrow** on the right, in the exact same position. This is the figure's punchline.

---

## Additional Design Notes

1. **Arrow count**: Left panel has exactly 3 arrows (2 solid + 1 dashed/missing). Right panel has exactly 4 arrows. No more. Use spatial proximity and color zones instead of arrows to show relationships.

2. **No box-inside-box nesting beyond one level**: Keep the hierarchy flat. Top row = one box with inline elements. Bottom row = one box with inline elements. That's it.

3. **The within-group split** in the right panel's "Rollouts" element should be subtle — a vertical divider inside the Rollouts box with two colors, not a separate diagram. Keep it compact.

4. **Panel titles**: Centered above each panel. Left: **"Existing: Skill Evolution Alongside RL"** in gray. Right: **"SCRL: Skill-Policy Co-Evolution (Ours)"** in bold with a subtle accent color.

5. **Size**: NeurIPS `figure*`, full text width (~6.5 inches / 16.5 cm), height ~3 inches / 7.5 cm.

6. **Do NOT include**: Equations, pseudocode, specific hyperparameters, math notation beyond πθ, or references to other figures.
