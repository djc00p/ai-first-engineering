# 🏗️ AI-First Engineering

[![ClawHub Skill](https://img.shields.io/badge/ClawHub-Skill-blue)](https://clawhub.ai/djc00p/ai-first-engineering) [![Agent Skill](https://img.shields.io/badge/Agent-Skill-blue)](#) [![AI-First](https://img.shields.io/badge/AI--First-8A2BE2.svg)](https://en.wikipedia.org/wiki/Software_development_process) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**AI-First Engineering** is an operating model for teams where AI agents generate a large share of implementation output.

It answers a question that traditional engineering process docs don't: when the majority of your code is machine-written, what changes? Not the tools — the **process, the architecture, the review standards, and how you hire**. The answer is that everything shifts toward explicitness, because an AI cannot read the unwritten convention that a human teammate absorbs by osmosis.

---

## 💡 The Core Shift

> **AI can implement fast. Humans must specify clearly.**

An ambiguous spec doesn't become fast work when a model writes it — it becomes *fast wrong work*, and you find out at review time when it's expensive to change. So the leverage moves upstream: into planning, evals, architecture, and review discipline.

### Five Pillars

1. **Invest in planning quality** — ambiguous specs cause AI-generated code to fail; write clear acceptance criteria first
2. **Raise eval coverage** — AI code requires higher test standards; regression coverage is mandatory for touched domains
3. **Shift review focus** — review for behavior, security, data integrity, and failure handling; let automation handle style
4. **Design agent-friendly architecture** — explicit boundaries, stable contracts, typed interfaces, deterministic tests
5. **Evaluate hiring signals** — decomposition skill, measurable criteria definition, prompt quality, risk control discipline

---

## 🏛️ Architecture for AI Teams

AI systems struggle with **implicit conventions, hidden coupling, and ambiguous contracts**. Explicit architecture makes generated code better *and* easier to review.

### 1. Explicit Boundaries Over Implicit Conventions

**❌ Implicit — breaks AI:**

```python
# Somewhere in module_a.py:
def calculate_price(items):
    return sum(i.price for i in items)  # assumes all items have .price

# Somewhere else, module_b.py:
result = calculate_price(my_items)  # assumes items are always non-empty
```

The AI generates code that violates these unstated assumptions. Reviewers find out after deployment.

**✅ Explicit — works with AI:**

```python
from typing import List, Protocol

class PricedItem(Protocol):
    price: float

def calculate_price(items: List[PricedItem]) -> float:
    """Calculate total price of items.

    Args:
        items: Non-empty list of items with .price attribute

    Returns:
        Total price in currency units

    Raises:
        ValueError: If items list is empty
    """
    if not items:
        raise ValueError("items must not be empty")
    return sum(i.price for i in items)
```

The AI generates code that respects the contract. Reviewers verify against the contract, not against assumptions nobody wrote down.

### 2. Stable Contracts

**❌ Unstable:** a function returning "either a result dict or None or a list" — callers have to guess.

**✅ Stable:** always returns the same structured type.

```python
from dataclasses import dataclass

@dataclass
class ProcessResult:
    success: bool
    event_id: str
    errors: List[str]

def process_event(event: Event) -> ProcessResult:
    ...
```

### 3. Typed Interfaces

Type hints everywhere. AI generates better code when types are explicit — without them, it guesses.

```python
# Bad: no type info, AI guesses
def get_user(id):
    ...

# Good: type info explicit, AI knows what's expected
def get_user(id: str) -> Optional[User]:
```

---

## 🔍 Code Review in AI-First Teams

Review shifts from *how it looks* to *what it does*:

```text
Behavior regressions: Did the change break existing functionality?
Security assumptions: Input validation, permission checks, sensitive data handling
Data integrity: Constraints, rollback safety, concurrent access
Failure handling: Network calls, database errors, timeouts, degraded modes
Rollout safety: Feature flags, backward compatibility, canary deploy strategy
```

**Automation is the baseline.** Style, formatting, and lint issues are solved by formatters and linters, not by review cycles. Spending human attention on them is waste.

**Deterministic tests only.** AI can't debug intermittent failures — a flaky test is not a test, it's a coin flip. Explicit error paths matter for the same reason: AI struggles with ambiguous error handling.

---

## 🧪 Testing Standard Raise

* **Regression coverage for every touched domain** — required, not optional
* **Explicit edge-case assertions** — AI may miss corner cases
* **Integration checks for interface boundaries** — behavior across module lines

Generated code needs *more* test coverage than hand-written code, not less. The implementation is cheap; the verification is what carries the risk.

---

## 👥 Hiring Signals for AI-First Engineers

### Strong signals

* **Decomposes ambiguous work cleanly** → clear, testable units
* **Defines measurable acceptance criteria** → no scope creep, clear done condition
* **Produces high-signal prompts and evals** → AI generates better code from better specs
* **Enforces risk controls under delivery pressure** → doesn't skip security or testing for speed

### Weak signals

* *"Move fast and break things"* mindset
* Writing code without clear specs or acceptance criteria
* Skipping regression tests to save time
* Vague PR descriptions (*"fixed bugs," "refactored stuff"*)

---

## 🚀 Quick Start

```text
1. Define acceptance criteria before any implementation
2. Require regression coverage for every touched domain
3. Move style review to automation; keep human review for risk
4. Make boundaries, contracts, and types explicit in the codebase
5. Evaluate candidates on decomposition and criteria-definition skill
```

---

## 📚 References

* `references/process-shifts.md` — detailed planning, evals, and review guidance
* `references/architecture-guide.md` — designing systems for AI code generation
* `references/testing-standards.md` — regression coverage, edge-case testing, integration checks

---

**Adapted from [everything-claude-code](https://github.com/affanm/everything-claude-code) by [@affaan-m](https://github.com/affaan-m) (MIT).**

**Published by [@djc00p](https://github.com/djc00p)**
