# State-Driven Interlock

**Version**: 1.0.0
**Status**: Initial Definition
**Last Updated**: 2025-12-20

---

## Definition

**State-Driven Interlock**: An architectural pattern where execution cannot proceed past a boundary point until an external state transition occurs, enforced by blocking control flow rather than behavioral compliance.

**One-Sentence Summary**: The system waits (blocks) at a specific code location until state changes, regardless of AI interpretation or reasoning.

---

## What This Is NOT

**NOT User Confirmation UX**:
- UX pattern: "Are you sure?" dialog (can be dismissed)
- State-Driven Interlock: Execution path blocked until input received (cannot proceed)

**NOT Prompt-Based Boundary**:
- Prompt: "Always ask before deleting files"
- Interlock: `if should_stop: decision = input()` (code-enforced wait)

**NOT Advisory System**:
- Advisory: AI recommends, suggests, warns
- Interlock: Execution cannot advance without state change

**NOT Behavioral Constraint**:
- Behavioral: AI trained to pause before risky actions
- Structural: Code blocks execution until condition met

---

## Key Properties

### 1. Blocking I/O Enforcement

**Mechanism**: Operating system-level blocking call (e.g., `input()`, `read()`, semaphore wait)

**Cannot Be Bypassed By**:
- AI reasoning ("user already approved")
- Prompt reinterpretation ("in this context, asking isn't needed")
- Context shifts ("this is a safe case")

**Can Only Be Bypassed By**:
- Code modification (explicit, auditable)
- OS-level intervention (external to AI)

---

### 2. State Externality

**State Source**: Outside the AI/execution context

**Examples**:
- Human keyboard input (`stdin`)
- External service response (blocking network call)
- Hardware signal (button press)

**Not State**:
- AI-generated decision
- Computed value from context
- Inferred approval

---

### 3. Structural, Not Behavioral

**Structure**: Control flow graph contains mandatory wait node

**Behavior**: AI might "decide" to wait (but could "decide" not to)

**Enforcement Location**: Code interpreter/compiler, not AI model

---

## Examples

### Example 1: Blocking Human Input

```python
def execute_with_interlock(action):
    preview = generate_preview(action)
    stop_result = check_stop_rules(action)

    if stop_result.should_stop:
        print(f"STOP: {stop_result.reason}")
        print(f"Preview: {preview}")

        # STATE-DRIVEN INTERLOCK (this line blocks execution)
        decision = input("Continue (c) or Abort (a)? ")

        if decision == 'a':
            return  # Cannot reach execute_action()

    execute_action(action)
```

**Interlock Location**: `input()` call (blocks until OS provides stdin)

**State Transition**: User types 'c' or 'a' → execution proceeds

**Cannot Bypass**: AI cannot provide `stdin` (OS restriction)

---

### Example 2: External Approval Service

```python
def execute_with_approval_service(action):
    approval_request = send_to_approval_service(action)

    # STATE-DRIVEN INTERLOCK (blocks until HTTP response)
    approval_response = approval_request.wait_for_response()

    if not approval_response.approved:
        return

    execute_action(action)
```

**Interlock Location**: `wait_for_response()` (blocks on network I/O)

**State Transition**: External service sends approval → execution proceeds

---

### Example 3: Hardware Button

```python
def execute_with_hardware_confirm(action):
    display_preview(action)

    # STATE-DRIVEN INTERLOCK (blocks until GPIO signal)
    wait_for_button_press(gpio_pin=17)

    execute_action(action)
```

**Interlock Location**: `wait_for_button_press()` (blocks on hardware signal)

**State Transition**: Physical button pressed → execution proceeds

---

## Counter-Examples

### Counter-Example 1: Confirmation Dialog (UX Pattern)

```python
def execute_with_confirmation_dialog(action):
    if is_risky(action):
        show_dialog("Are you sure?")
        # No blocking - dialog can be dismissed

    execute_action(action)  # Reachable without state change
```

**Why NOT Interlock**: Dialog is advisory (execution continues if dismissed)

---

### Counter-Example 2: AI Self-Check

```python
def execute_with_ai_check(action):
    safety_assessment = ai_model.assess_safety(action)

    if not safety_assessment.is_safe:
        print("AI determined this is unsafe, skipping")
        return

    execute_action(action)
```

**Why NOT Interlock**: AI makes decision (no external state change required)

---

### Counter-Example 3: Timeout-Based Wait

```python
def execute_with_timeout(action):
    print("Waiting 5 seconds for user to abort...")
    time.sleep(5)  # Proceeds automatically after timeout

    execute_action(action)
```

**Why NOT Interlock**: No state transition needed (time passes → execution proceeds)

---

## Differentiation from Related Concepts

### vs. Human-in-the-Loop (HITL)

| Aspect | HITL | State-Driven Interlock |
|--------|------|------------------------|
| Human Role | Approves AI decision | Provides state transition |
| Decision Origin | AI (human validates) | External (human decides) |
| Enforcement | Optional (system can proceed) | Mandatory (system cannot proceed) |
| Bypass | Possible (timeout, default) | Impossible (code modification only) |

---

### vs. Confirmation Dialog (UX)

| Aspect | Confirmation Dialog | State-Driven Interlock |
|--------|---------------------|------------------------|
| Purpose | Prevent accidental clicks | Enforce decision boundary |
| Dismissible | Yes (user can cancel) | No (blocks until input) |
| Enforcement | UI layer | Execution layer |
| Bypass | Close dialog, timeout | Code change only |

---

### vs. Rate Limiting

| Aspect | Rate Limiting | State-Driven Interlock |
|--------|---------------|------------------------|
| Trigger | Time/frequency threshold | Boundary condition |
| State Source | Internal (counter, timer) | External (human, service) |
| Purpose | Prevent overload | Enforce judgment boundary |
| Bypass | Wait for timer reset | Provide external input |

---

## Conceptual Boundaries

**Interlock IS**:
- Blocking execution until state change
- Code-enforced (structural)
- State externally sourced

**Interlock IS NOT**:
- Preventing unsafe actions (that's STOP rules)
- Making better decisions (that's AI capability)
- Improving user experience (that's UX)

**Interlock ENABLES**:
- Judgment externalization (decision authority outside execution)
- Responsibility clarity (state transition is auditable)
- Bypass resistance (code modification required)

---

## Usage in Echo Judgment System

**Where Used**: Local executor's `_await_human_decision()`, Web executor's STOP handler

**Implementation**: `input()` call that blocks execution until user types 'c' or 'a'

**Integration with STOP Rules**:
1. STOP rule checks action (boundary detection)
2. If triggered → State-Driven Interlock activates
3. Execution blocked until human input
4. State transition recorded in telemetry

**See**: `ops/tools/headed_executor/local_executor.py:142-156`

---

## Theoretical Foundation

**Control Flow Theory**: Interlock creates mandatory node in execution graph

**Concurrency Theory**: Blocking I/O as synchronization primitive

**Security Theory**: Enforced mediation (all access through interlock point)

**Decision Theory**: State transition externalizes decision authority

---

## Common Misunderstandings

**Misunderstanding 1**: "This is just asking for permission"

**Correction**: Permission request is advisory. Interlock is mandatory blocking.

---

**Misunderstanding 2**: "AI could learn to bypass this"

**Correction**: AI cannot provide blocking I/O input (OS restriction, not learning barrier).

---

**Misunderstanding 3**: "This slows down automation"

**Correction**: Yes. That's intentional (boundary enforcement > automation speed).

---

**Misunderstanding 4**: "Users will find this annoying"

**Correction**: User experience is not the goal (responsibility clarity is).

---

## Related Concepts

**Depends On**: None (foundational concept)

**Enables**:
- Judgment Cannot Fire (interlock prevents decision execution)
- Execution Without Judgment (executor blocked, decision external)

**Orthogonal To**:
- Exit vs Bypass (interlock enforces boundary, exit/bypass defines cooperation)

---

## Research Implications

**Testable Hypothesis**: Systems with state-driven interlocks have lower rates of unauthorized execution than behavioral constraint systems.

**Measurement**: Compare bypass attempts in interlock vs prompt-based systems.

**Null Hypothesis**: Interlock and behavioral constraints have equal bypass resistance.

---

## Version History

**v1.0.0** (2025-12-20): Initial definition

---

## References

**Echo Judgment System Architecture**: See ARCHITECTURE.md in Boundary Constitution repo

**Execution Evidence**: See incidents/ in Evidence repo for real-world interlock activations

---

## Citation

```
State-Driven Interlock, Echo Judgment System Concept Vocabulary
Version: 1.0.0
Repository: https://github.com/Nick-heo-eg/echo-judgment-concept-vocabulary
Last Updated: 2025-12-20
```
