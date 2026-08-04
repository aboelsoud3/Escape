## Doc 16 — Documentation Structure

### Goal
Organize all project documentation in a clear, searchable hierarchy for easy discovery and context transfer.

### Directory Layout

```
Docs/
├── KB.md                     # Master knowledge base (architecture rules, design pillars)
├── engineering-plan/         # Docs 1–25: planning artifacts (current directory)
│   ├── 01-project-roadmap.md
│   ├── 02-engineering-workflow.md
│   ├── 03-phase-dependency-graph.md
│   └── ... (through 25)
├── design/                   # Game design docs, level sketches, GDD
├── research/                 # Tech stack evaluations, benchmark studies
├── build/                    # Released artifacts, compilation output
└── reference/                # Code samples, API references, tooling docs
```

### Document Indexing Strategy
- Files in `engineering-plan/` are numbered sequentially (`01-`, `02-`, …)
- Each file follows the header format: `## Doc N — TITLE`
- File names use lowercase kebab-case (e.g., `16-documentation-structure.md`)
- Each doc starts with a table of contents if > 4 sections

### Content Guidelines
- **KB.md**: Living document; append decisions, not rewrite full sections
- **Engineering Plan**: Indexed numerically; each doc self-contained but cross-referenced to KB and others
- **Design/Research/Reference**: Unnumbered directories grouped by function; sorted alphabetically within
- Use concise tables and bullet lists over long prose paragraphs

### Navigation
- `README.md` at project root (if created) or `Docs/` provides a summary link list to all planning docs
- Cross-references use numbered identifiers: see `15-build-pipeline.md` Section 3.2
- When Doc X depends on Doc Y for context, always include a `See also:` line
