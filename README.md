# Exploratory Testing Skill for Claude Code

A Claude Code skill that enables structured, heuristic-driven exploratory testing sessions with automatic MCOASTER report generation.

## What it does

Invoke `/exploratory-testing` with a test charter and Claude will:

1. Execute an exploratory testing session guided by industry-standard heuristics
2. Automatically produce a full **MCOASTER report** at the end — no prompting needed
3. Log every bug found with ID, severity, criterion, evidence, and a suggested fix
4. Deliver a prioritised recommendations list

## Heuristics included

| Mnemonic | Purpose |
|----------|---------|
| FEW HICCUPPS | Test oracles — how to recognise a problem |
| SFDIPOT | Product coverage — what areas to explore |
| MCOASTER | Session reporting structure |
| I SLICED UP FUN | Mobile app testing |
| FCC CUTS VIDS | Application touring strategies |
| RCRCRC | Regression test prioritisation |
| FAILURE | Error handling evaluation |
| RIMGEA | Bug advocacy and reporting |
| CRUSSPIC STMPL | Non-functional quality attributes |
| + more | Data attack cheat sheet, data type heuristics, wisdom rules |

## Installation

```
/plugin install exploratory-testing@danashby/Exploratory-Testing-Skill
```

## Usage

```
/exploratory-testing "Explore the search screen, with WCAG 2.2 AA in mind, to discover accessibility risks"
```

Use the charter format:
> **"Explore [target], with [resources/constraints], to discover [information about risks]"**

## Output format

Every session produces:
- MCOASTER report sections (Mission, Coverage, Obstacles, Audience, Status, Techniques, Environment, Risks)
- Numbered bug list (BUG-01, BUG-02...) with severity, criterion, evidence, and fix
- Summary table
- Prioritised recommendations

## License

MIT
