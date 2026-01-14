# Concept Vocabulary: Judgment Externalization

This repository documents experimental execution during research.
It does not represent a production system, live authority, or active enforcement.

---



**Repository Type**: Definitional (Open for Refinement)

---

## Purpose

This repository defines **core concepts** used in the Echo Judgment System.

**What This Contains**:
- Formal definitions of technical terms
- Distinction from related concepts
- Examples and counter-examples
- Conceptual boundaries

**What This Does NOT Contain**:
- Implementation details (those belong in Boundary Constitution)
- Execution evidence (those belong in Evidence repo)
- Design decisions (those belong in Constitution)

---

## Repository Philosophy

**This repo is OPEN for refinement** (unlike the other two).

**Reason**: Concepts evolve through discussion and critique.

**How to Contribute**:
1. Submit PR with proposed definition change
2. Include justification (why current definition is insufficient)
3. Provide examples showing need for refinement
4. Discussion in PR comments

**What Gets Accepted**:
- Increased precision (removes ambiguity)
- Better differentiation (clearer boundaries from related terms)
- Counter-examples (showing what concept is NOT)

**What Gets Rejected**:
- Marketing language ("revolutionary," "paradigm-shifting")
- Vague generalizations ("makes systems better")
- Implementation details (belongs in Constitution)

---

## Core Concepts

### Judgment Externalization Concepts

1. **[State-Driven Interlock](concepts/state-driven-interlock.md)**
   - Execution blocked until state transition
   - Not advisory, not behavioral - structural

2. **[Judgment Cannot Fire](concepts/judgment-cannot-fire.md)**
   - Decision authority structurally separated from execution
   - AI can interpret, cannot commit

3. **[Exit vs Bypass](concepts/exit-vs-bypass.md)**
   - Exit = cooperation (allowed)
   - Bypass = violation (blocked)

4. **[Execution Without Judgment](concepts/execution-without-judgment.md)**
   - Actions run, but decision authority is external
   - Executor has no opinion on "should this happen"

5. **[Unresolvable Under Ruleset](concepts/unresolvable-under-ruleset.md)**
   - STOP when no safe interpretation exists
   - Ambiguity is data, not failure

---

## Concept Format

Each concept file contains:

**Required Sections**:
- Definition (1-2 sentences, precise)
- What This Is NOT (contrast with related concepts)
- Key Properties (structural characteristics)
- Examples (concrete instances)
- Counter-Examples (what looks similar but isn't)
- Differentiation (from UX patterns, guardrails, etc.)

**Prohibited Sections**:
- "Benefits" or "Advantages" (not marketing material)
- Implementation guides (belongs in Constitution)
- Usage instructions (belongs in Constitution)

---

## Differentiation from Related Work

**vs. User Experience (UX) Patterns**:
- UX: Improve ease of use
- This: Enforce structural boundaries (may reduce ease of use)

**vs. Safety Guardrails**:
- Guardrails: Prevent bad outputs
- This: Prevent decisions from firing at all

**vs. Prompt Engineering**:
- Prompts: Behavioral guidance (AI cooperation required)
- This: State-based enforcement (AI cooperation not required)

**vs. Human-in-the-Loop (HITL)**:
- HITL: Human approves AI decision
- This: Human makes decision, AI executes

**vs. Explainable AI (XAI)**:
- XAI: AI explains its reasoning
- This: AI has no decision authority to explain

---

## Concept Dependencies

```
State-Driven Interlock
    └─> Judgment Cannot Fire
            └─> Execution Without Judgment
                    └─> Unresolvable Under Ruleset

Exit vs Bypass (orthogonal to above)
```

**Reading Order** (for newcomers):
1. State-Driven Interlock (foundation)
2. Judgment Cannot Fire (core claim)
3. Exit vs Bypass (boundary semantics)
4. Execution Without Judgment (executor role)
5. Unresolvable Under Ruleset (STOP philosophy)

---

## Citation

When citing concepts from this repository:

```
[Concept Name], Echo Judgment System Concept Vocabulary
Repository: https://github.com/Nick-heo-eg/echo-judgment-concept-vocabulary
Version: [commit hash or tag]
Accessed: [date]
```

**Why Version Matters**: Concepts may be refined over time. Cite specific version.

---

## Contribution Guidelines

**Propose New Concept**:
1. Must be distinct from existing concepts (not a synonym)
2. Must be structural (not behavioral or advisory)
3. Must be citation-worthy (used in research or design)

**Refine Existing Concept**:
1. Show ambiguity in current definition
2. Provide example where current definition fails
3. Propose more precise wording
4. Explain impact on dependent concepts

**Remove Concept**:
1. Show it's subsumed by another concept (redundant)
2. Show it's implementation detail (not conceptual)
3. Provide migration path (update dependent docs)

---

## Relationship to Other Repositories

**Boundary Constitution Repo** (https://github.com/Nick-heo-eg/EchoJudgmentSystem_v10-1):
- Uses concepts defined here
- Implementation of concepts
- References specific concept versions

**Execution Evidence Repo** (https://github.com/Nick-heo-eg/echo-judgment-execution-evidence):
- Uses concepts in incident analysis
- Cites concepts when describing violations

**This Repo** (Concept Vocabulary):
- Defines terms used by both
- Open for refinement (unlike Constitution)
- Versioned for citation stability

---

## Versioning

**Major Version** (e.g., v1.0 → v2.0):
- Definition fundamentally changed
- Incompatible with previous version
- Requires update to all references

**Minor Version** (e.g., v1.0 → v1.1):
- Definition refined (more precise)
- Compatible with previous understanding
- Existing references still valid

**Patch Version** (e.g., v1.0.0 → v1.0.1):
- Typo fixes, clarifications
- No semantic change
- References unchanged

**Current Version**: v1.0.0 (initial definitions)

---

## Non-Goals

**We do NOT**:
- Standardize terms across all AI safety research (too broad)
- Create taxonomy of all judgment-related concepts (too ambitious)
- Enforce terminology on other projects (not prescriptive)

**We DO**:
- Define terms as used in Echo Judgment System
- Provide precise definitions for citation
- Differentiate from superficially similar concepts

---

## Frequently Asked Questions

**Q: Can I propose a new concept?**

A: Yes, if it's structural (not behavioral), distinct (not a synonym), and citation-worthy.

---

**Q: Can I disagree with a definition?**

A: Yes. Submit PR with justification and alternative definition.

---

**Q: Are these definitions authoritative for other projects?**

A: No. These are definitions as used in Echo Judgment System. Other projects may use terms differently.

---

**Q: Why separate repo for definitions?**

A: Because concepts can be refined independently of implementation (Constitution) and evidence collection.

---

**Q: Can I use these definitions in my research?**

A: Yes. Cite specific version (commit hash or tag).

---

## Contact

**For Concept Clarifications**: Open issue with "Clarification:" prefix

**For Definition Refinements**: Submit PR with justification

**For New Concepts**: Open issue with "New Concept:" prefix (discuss before PR)

---

## Status

**Repository Type**: Concept Vocabulary

**Modification Rights**: Open for refinement (versioned)

**Current Concepts**: 5 (initial set)

**Current Version**: v1.0.0

**Last Structure Update**: 2025-12-20

---

## Final Statement

**This repository defines concepts precisely.**

**Precision enables critique.**

**Critique enables refinement.**

**Refinement strengthens understanding.**
