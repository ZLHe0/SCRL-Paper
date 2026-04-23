# Figure Specification: From Human-in-the-Loop to RL-in-the-Loop Skill Creation

## Overall Layout and Style

**Target venue**: NeurIPS 2026 (top ML conference). The figure must look professional, clean, and publication-ready.

**Dimensions**: Full-width figure spanning two columns. Aspect ratio approximately 2.2:1 (wide). Target resolution: 300 DPI, approximately 2100 x 950 pixels.

**Layout**: Two panels side-by-side, separated by a thin vertical divider or whitespace gap. Each panel occupies roughly half the width.
- **Left panel** title: "Human-in-the-Loop Skill Creation"
- **Right panel** title: "RL-in-the-Loop Skill Creation (Ours)"

**Visual relationship**: The two panels should mirror each other structurally — same vertical positions for analogous components, same flow direction — so the reader immediately sees the parallel. Use matching positions: the "feedback source" (human on the left, RL reward on the right) should be at the same vertical level. The "skill creation" box should be at the same level on both sides.

**Color palette**: Clean, muted academic colors. Suggested scheme:
- Left panel accent: **blue tones** (#2166AC, #92C5DE) — representing the human/interactive side
- Right panel accent: **orange/red tones** (#E65100, #F4A582) — representing the RL/automated side
- Shared/neutral elements: light gray backgrounds (#F5F5F5), dark gray text (#333333)
- Accept/success: green (#2E7D32)
- Boxes: white fill with colored borders, slight rounded corners (3-4px radius)
- Arrows: medium-weight (2px), with arrowheads, colored to match their source

**Typography**: Sans-serif font (Helvetica or similar). Panel titles in bold ~11pt. Box labels in regular ~9pt. Annotation text in ~7-8pt italic. All text must be legible at printed paper size (single-column width per panel).

**Background**: Pure white. No gradients, no drop shadows, no 3D effects. Flat, clean, modern academic style.

---

## Left Panel: Anthropic's Skill Creator (Human-in-the-Loop)

### Description
This panel shows how Anthropic's Skill Creator works: a human user interacts with an AI system that iteratively creates and improves skills. The flow is a cycle: the user has a task, the system creates skill versions, tests them blindly, the user reviews and gives feedback, and the cycle repeats.

### Components (top to bottom, with a cyclic flow)

**1. User icon** (top-left area)
- A simple person/user silhouette icon in blue
- Label: **"User"**
- Small annotation below: "Provides task intent + feedback"

**2. Skill Creator box** (center-left)
- A rounded rectangle, white fill with blue (#2166AC) border
- Label inside: **"Skill Creator"**
- Sub-label (smaller, gray): "Writes & refines skills"
- This box has two output arrows going down to two parallel skill version boxes

**3. Two parallel Skill Version boxes** (below Skill Creator, side by side)
- Two small rounded rectangles, same size, positioned horizontally next to each other
- Left box: light blue fill, label **"V_A"** (or "Version A")
- Right box: light blue fill, label **"V_B"** (or "Version B")
- A small annotation between/below them: "Two candidate versions"
- Each has a downward arrow leading to...

**4. Blind Comparator box** (below the two versions)
- A rounded rectangle with a slightly different styling (e.g., dashed border or distinct color like dark blue #1A237E) to indicate it's independent/blind
- Label: **"Blind Comparator"**
- Sub-label: "Judges outputs without knowing which version produced which"
- An eye-with-slash icon or a blindfold icon would be a nice touch (optional)
- Key visual: the two arrows from V_A and V_B enter this box, but the labels are hidden/anonymized (to convey "blind")

**5. Human Review** (below or to the right of comparator)
- The user icon reappears here, or an arrow loops back up to the user
- A box or region labeled: **"Human Review"**
- Sub-label: "Reviews results, selects winner, provides qualitative feedback"
- A clipboard or review icon (optional)

**6. Feedback arrow** (closing the loop)
- A curved or angled arrow going from Human Review back up to the Skill Creator box
- Label on arrow: **"Feedback"**
- This completes the cycle

### Flow summary (arrows)
```
User --[task intent]--> Skill Creator
Skill Creator --[writes]--> V_A, V_B (parallel)
V_A, V_B --[outputs]--> Blind Comparator
Blind Comparator --[results]--> Human Review (User)
Human Review --[feedback]--> Skill Creator (loop back)
```

### Annotations
- Somewhere in this panel (bottom or side), a small label: **"~318 LLM calls / cycle"** in gray italic
- Another annotation: **"Requires human involvement each iteration"**

---

## Right Panel: SCRL (RL-in-the-Loop)

### Description
This panel shows how SCRL automates the same process within RL training. The RL training loop (GRPO) generates rollouts; failed ones feed the skill creator; the skill creator proposes a new version; rollouts are split within each GRPO group between the new and old versions (analogous to blind testing); reward signals determine which version wins; and the policy is simultaneously updated. The key message: **same conceptual loop, but fully automated and zero-cost** (no additional rollouts).

### Components (mirroring the left panel's structure)

**1. RL Training Loop / GRPO box** (top area, same position as User on the left)
- A rounded rectangle with orange (#E65100) border
- Label: **"GRPO Training"**
- Sub-label: "Samples G rollouts per task"
- Icon: a circular arrow or gear icon to suggest an ongoing training process
- This box represents the source of rollouts — it is the analog of the "User" on the left

**2. Skill Creator box** (center, same vertical position as left panel's Skill Creator)
- A rounded rectangle, white fill with orange border
- Label: **"Skill Creator"**
- Sub-label: "Analyzer -> Analyst -> Author"
- Visually should be at the same position as the left panel's Skill Creator to show the parallel
- An arrow comes INTO this box from a "Failed Trajectories" element

**3. Failed Trajectories / Reservoir** (between GRPO and Skill Creator)
- A small cylinder or stack-of-documents icon in light orange
- Label: **"Failure Reservoir"**
- An arrow from GRPO Training down to this element, labeled "failed rollouts (r=0)"
- An arrow from this element to the Skill Creator

**4. Two parallel Version boxes** (below Skill Creator, same position as left panel)
- Two small rounded rectangles side by side
- Left box: label **"V_old"** (current version), lighter shade
- Right box: label **"V_new"** (candidate version), slightly bolder/highlighted
- Annotation: "Proposed version vs. current"

**5. Within-Group Split** (below the two versions — this is the analog of the Blind Comparator)
- THIS IS THE KEY ELEMENT. It should visually parallel the Blind Comparator on the left.
- Show a group of G small circles/dots (representing rollouts from one GRPO group), with approximately half colored in V_old's color and half in V_new's color
- Or: a rectangular region divided into two halves with small trajectory icons
- Label: **"Within-Group Rollout Split"**
- Sub-label: "Same task, same policy — only skill version differs"
- The visual should convey: from one group of rollouts, some go through V_old, some through V_new
- Annotation pointing to this: **"Controlled comparison (analogous to blind testing)"**

**6. Thompson Sampling / Accept-Reject** (below the split, same position as Human Review on left)
- A rounded rectangle with a green (#2E7D32) or orange border
- Label: **"Thompson Sampling"**
- Sub-label: "Beta-Bernoulli posteriors with adaptive discount"
- Two small elements inside or below: a checkmark icon labeled "Accept" and an X icon labeled "Reject"
- The reward signals (binary success/failure from each rollout) feed into this box

**7. Feedback arrow / Policy Update** (closing the loop — TWO arrows going back up)
- **Arrow 1** (the skill loop): From Thompson Sampling back to the Skill Creator, labeled **"Accept/Reject + history"** — this closes the skill evolution cycle
- **Arrow 2** (the policy loop): From the Within-Group Split region back up to GRPO Training, labeled **"Policy gradients"** — this represents the GRPO policy update happening from the SAME rollouts
- Arrow 2 is critical: it shows that the rollouts serve double/triple duty

### Flow summary (arrows)
```
GRPO Training --[rollouts]--> Failed Trajectories (subset with r=0)
Failed Trajectories --[reservoir]--> Skill Creator
Skill Creator --[proposes]--> V_new (alongside existing V_old)
V_old, V_new --[assigned to rollouts]--> Within-Group Split
Within-Group Split --[reward signals]--> Thompson Sampling
Thompson Sampling --[accept/reject]--> Skill Creator (loop back)
Within-Group Split --[GRPO gradients]--> GRPO Training (loop back, SIMULTANEOUS)
```

### Annotations
- Somewhere in this panel: **"3 LLM calls / cycle"** in gray italic (contrasting with 318 on the left)
- Another annotation: **"Zero additional rollouts"** (bold or highlighted)
- A third annotation near the "Policy gradients" arrow: **"Triple duty: same rollouts update policy, collect failures, and evaluate skills"**

---

## Visual Parallels to Emphasize

The figure's power comes from the structural mirror between left and right. Ensure these correspondences are visually obvious:

| Left (Human) | Right (RL) | Visual treatment |
|---|---|---|
| **User** provides task + feedback | **GRPO Training** provides rollouts + rewards | Same vertical position, same box size |
| **Skill Creator** writes versions | **Skill Creator** proposes versions | Same box, same position, same label |
| **V_A / V_B** candidates | **V_old / V_new** candidates | Same paired-box layout |
| **Blind Comparator** judges blindly | **Within-Group Split** controlled comparison | Same position; both convey "fair test" |
| **Human Review** selects winner | **Thompson Sampling** selects winner | Same position; both convey "decision" |
| **Feedback** loop arrow | **Accept/Reject** loop arrow | Same curved arrow shape |
| (nothing) | **Policy gradients** arrow (extra!) | This is the bonus — RL side has an extra loop |

The extra "Policy gradients" arrow on the right (which has no analog on the left) should be visually distinct — perhaps a **different color** (green?) or **dashed style** — to highlight that this is the unique advantage: the same rollouts that test skills also train the policy.

---

## Additional Design Notes

1. **Connecting element between panels**: Consider a subtle visual bridge between the two panels — perhaps light dashed horizontal arrows at the Skill Creator and Comparator/Split levels with labels like "analogous to" in very small gray text. Or simply rely on the structural mirror (preferred for cleanliness).

2. **No excessive detail**: The figure should be readable at a glance. Each box should have at most 2 lines of text (label + sub-label). The annotations are secondary and should be in smaller, lighter text.

3. **Panel titles**: Centered above each panel. Left: **"Human-in-the-Loop Skill Creation"** in blue. Right: **"RL-in-the-Loop Skill Creation (Ours)"** in orange. The "(Ours)" marker should be bold or slightly larger.

4. **Overall impression**: The reader should immediately understand: "Oh, the right side automates the left side by replacing the human with RL rewards, and as a bonus, the same rollouts also train the policy." This is the single takeaway.

5. **Do NOT include**: Equation numbers, algorithm pseudocode, detailed mathematical notation, specific hyperparameter values, or references to other figures. This is a conceptual overview figure.

6. **Aspect ratio reminder**: This is a `figure*` environment in NeurIPS LaTeX — it spans the full text width (~6.5 inches / 16.5 cm). Height should be approximately 3 inches / 7.5 cm. Not too tall — it shares the first page with the title, authors, abstract, and the start of the introduction text.
