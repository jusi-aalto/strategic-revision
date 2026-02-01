# strategic-revision

A Claude Code skill that produces dependency-mapped revision master plans from peer review reports, with computational DAG validation using NetworkX.

## Installation

1. Download the `strategic-revision.skill` file from this repository
2. In Claude Code, go to **Settings → Capabilities → Skills → Add** and upload the `.skill` file

## Input Requirements

The skill requires two inputs, placed in the working directory or provided when prompted:

1. **Peer review reports** — verbatim reports from each reviewer and the editor. Any format (PDF, Word, plain text). Provide the full unedited text; the skill traces every task back to a specific reviewer quote, so summaries are insufficient.
2. **Original manuscript** — the submitted version under review. The full manuscript is preferred; at minimum, the abstract and section structure are needed for dependency mapping.

## What It Does

Analyzes reviewer comments and the manuscript to produce a structured revision roadmap with:

- Atomic task extraction from each reviewer comment
- Classification by type (empirical, argumentative, structural, clarification, editorial)
- Dependency mapping as a directed acyclic graph
- Computational validation (acyclicity, parallel batches, critical path, bottleneck identification)
- Execution blocks with GO/NO-GO decision points

## Workflow

| Phase | Name | Purpose |
|-------|------|---------|
| 1 | Atomic Parsing | Extract every distinct request as a separate task |
| 2 | Classification | Tag each task by category |
| 3 | Dependency Mapping | Build the DAG of task dependencies |
| 3b | Structural Validation | Confirm the graph is acyclic before sequencing |
| 4 | Critical Path Sequencing | Group tasks into execution blocks |
| 5 | Risk & Conflict Resolution | Identify reviewer conflicts and process risks |
| 6 | Computational Optimization | Run full NetworkX analysis and refine the roadmap |

## Files

```
strategic-revision/
├── SKILL.md                              # Skill definition and workflow
├── scripts/
│   └── dag_validator.py                  # NetworkX validation script
└── references/
    ├── phases.md                         # Phases 1-5 (including 3b) instructions
    ├── phase6-dag-validation.md          # Phase 6 optimization instructions
    └── task-schema.md                    # JSON schema for revision_tasks.json
```

## Script Usage

```
python dag_validator.py revision_tasks.json                    # full analysis
python dag_validator.py revision_tasks.json --validate-only    # Phase 3b gate check
python dag_validator.py revision_tasks.json --task A1_control  # single task
python dag_validator.py revision_tasks.json --quiet            # JSON only
```

Requires Python 3 and `networkx` (`pip install networkx`).

## Trigger

Invoke with `/strategic-revision` or any request to create a revision plan with DAG validation from peer review reports.
