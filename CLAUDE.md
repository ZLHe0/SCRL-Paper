# NeurIPS Manuscript

This directory is a **separate git repository** (`SCRL-Paper`) connected to Overleaf for collaborative editing.

- Remote: `https://github.com/ZLHe0/SCRL-Paper.git`
- Synced with Overleaf via GitHub integration
- **Do not** track this directory in the parent `agentic_evo_rl` repo (it is gitignored at `docs/manuscript/`)

## Workflow

- Pull from `origin/main` before editing to pick up Overleaf changes.
- After editing locally, commit and push to `origin/main` so Overleaf syncs.
- Compile with `pdflatex -interaction=nonstopmode scrl.tex` (run twice for cross-references).
