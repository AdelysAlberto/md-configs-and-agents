---
name: market-research
description: Market research, competitive analysis, feature parity benchmarks, user pain validation, and product positioning. Use when analyzing market opportunities, researching alternatives, or benchmarking competitors.
license: MIT
compatibility: opencode
metadata:
  domain: discovery
  methodology: competitor-benchmarking
---

# Market Research & Competitor Analysis Standards

Comprehensive guide for deductive market research, competitor landscape mapping, feature parity analysis, and problem-solution validation.

## 1. Core Research Invariants

1. **Evidence over Assumptions**: Never state market demands or competitor capabilities without verifiable documentation, live product testing, or direct public sources.
2. **Focus on Pain, Not Just Features**: Differentiate between surface-level features and the root problem users are paying or struggling to solve.
3. **Actionable Gaps**: Research must culminate in clear, defensible differentiation opportunities rather than passive summaries.
4. **Structured Artifact Output**: Findings should be compiled cleanly into `artifacts/market_research.md`.

---

## 2. Competitor Benchmarking Matrix

When analyzing competitors, build a structured comparison table:

| Dimension | Direct Competitor A | Indirect Competitor B | Our Proposed Solution |
| :--- | :--- | :--- | :--- |
| **Core Value Prop** | "All-in-one CRM" | "Simple spreadsheet" | "Specialized real-time route engine" |
| **Target Audience** | Enterprise sales teams | Small businesses | High-density logistics couriers |
| **Pricing Model** | Seat-based ($50/user/mo) | Free / freemium | Usage-based per completed route |
| **Key Strengths** | Rich ecosystem, integrations | Zero learning curve | Millisecond latency, offline-first |
| **Critical Weaknesses**| High complexity, slow mobile | No routing optimization | Younger ecosystem |
| **Market Gap** | Slow navigation UI in field | Cannot handle turn-by-turn | Native performance + turn-by-turn HUD |

---

## 3. Problem-Solution Validation Framework

Follow the deductive validation cycle:

1. **User Pain Identification**:
   - What friction does the user encounter today?
   - What workarounds do they currently hack together?
   - What is the tangible cost of this pain (time lost, money lost, cognitive fatigue)?
2. **Solution Hypothesis**:
   - What is the minimal feature that solves 80% of this pain?
   - Why have existing solutions failed to address it?
3. **Defensibility & Unfair Advantage**:
   - What makes our technical or product approach unique (e.g. offline vector tiles, DIP provider adapters)?

---

## 4. Market Research Document Structure (`artifacts/market_research.md`)

```markdown
# Market & Competitor Research: [Project / Feature Name]

## 1. Executive Summary & Market Opportunity
High-level synthesis of findings and primary opportunity window.

## 2. Target Audience Personas
- **Primary Persona**: Goals, daily workflows, core frustrations.
- **Secondary Persona**: Secondary stakeholders and beneficiaries.

## 3. Competitive Landscape
- Tier 1 Direct Competitors.
- Tier 2 Indirect & Alternative Solutions.
- Feature Parity & Gap Matrix.

## 4. Key Differentiators & UVP (Unique Value Proposition)
The single compelling reason users will switch or adopt this solution.

## 5. Strategic Recommendations & Roadmap Inputs
Priority feature recommendations for the Product Requirements Document (PRD).
```
