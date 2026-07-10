# Decision Engine

> The Decision Engine is responsible for selecting a single UX action from multiple competing recommendations.

Plugins do not control the interface.

They observe, interpret and recommend.

The Decision Engine evaluates every recommendation against runtime policies and determines whether the interface should respond.

This separation ensures that intelligence remains explainable, deterministic and auditable.

---

# Responsibilities

The Decision Engine is responsible for:

- evaluating recommendations
- resolving conflicts
- enforcing runtime policies
- respecting accessibility settings
- applying cooldowns
- consulting AI providers (optional)
- selecting exactly one outcome

The engine never renders UI directly.

---

# Core Philosophy

A recommendation is not a command.

Every recommendation must earn execution.

The engine asks:

- Is this intervention necessary?
- Is it timely?
- Is it accessible?
- Has something similar already been shown?
- Is silence the better option?

---

# Decision Pipeline

Every recommendation passes through the same pipeline.

```
Plugin Recommendations

↓

Policy Validation

↓

Accessibility Validation

↓

Cooldown Validation

↓

Priority Resolution

↓

Confidence Evaluation

↓

AI Arbitration (optional)

↓

Final Decision

↓

Renderer
```

---

# Recommendation Model

Each plugin returns a structured recommendation.

```ts
interface Recommendation {
  pluginId: string

  action: string

  confidence: number

  priority: number

  payload: object

  expiresAt?: number
}
```

Recommendations are immutable once emitted.

---

# Confidence

Confidence represents how strongly a plugin believes an action should occur.

Range:

```
0.0 → 1.0
```

Example:

| Plugin | Confidence |
|---------|------------|
| Ghost Cursor | 0.82 |
| Reading Assistant | 0.58 |
| Exit Intent | 0.91 |

Confidence is not probability.

It is a measure of recommendation strength.

---

# Priority

Priority represents business importance.

Example:

| Plugin | Priority |
|---------|----------|
| Error Recovery | 100 |
| Accessibility | 95 |
| Exit Intent | 80 |
| Ghost Cursor | 60 |
| Reading Assistant | 40 |
| Analytics | 0 |

Priority alone does not determine the outcome.

---

# Runtime Policies

Policies define hard constraints.

Examples:

```
Maximum one intervention every 60 seconds.

Never interrupt typing.

Never interrupt drag operations.

Never animate when reduced motion is enabled.

Never cover active form controls.

Never display multiple interventions simultaneously.
```

Policies override recommendations.

---

# Accessibility Overrides

Accessibility has absolute precedence.

Example:

If:

```
prefers-reduced-motion = true
```

then:

```
Animated Cursor

↓

Rejected

↓

Static Hint

↓

Approved
```

Another example:

```
Keyboard Navigation

↓

Ghost Cursor

↓

Rejected

↓

Focus Ring Assistance

↓

Approved
```

Accessibility is never optional.

---

# Cooldowns

Recommendations may be blocked if cooldowns are active.

Example:

```
Ghost Cursor

↓

Triggered

↓

Cooldown

60 seconds

↓

Subsequent Recommendation

↓

Rejected
```

Cooldowns prevent intervention fatigue.

---

# Conflict Resolution

Multiple plugins may recommend different actions.

Example:

```
Ghost Cursor

↓

SHOW_CURSOR

Confidence 0.83

Priority 60


Reading Assistant

↓

SHOW_SUMMARY

Confidence 0.71

Priority 40


Exit Intent

↓

SAVE_PROGRESS

Confidence 0.88

Priority 80
```

The engine compares:

- policy compliance
- accessibility
- cooldown status
- priority
- confidence

Only one recommendation proceeds.

---

# Tie Breaking

If two recommendations are equally valid:

Resolution order:

1. Higher priority

2. Higher confidence

3. Earlier recommendation

4. Lower visual disruption

5. Do nothing

The final fallback is intentional silence.

---

# Silence as a Decision

No recommendation is always better than a poor recommendation.

Example:

```
Ghost Cursor

Confidence

0.51

Threshold

0.75

↓

No Action
```

The engine treats silence as a successful outcome.

---

# AI Arbitration

AI is optional.

If enabled:

```
Validated Recommendations

↓

LLM

↓

Ranking Suggestion

↓

Decision Engine

↓

Renderer
```

The LLM may:

- reorder
- reword
- suppress
- merge

It may not violate runtime policies.

---

# Decision Record

Every decision generates a record.

Example:

```json
{
  "decisionId": "...",
  "timestamp": "...",
  "selectedPlugin": "ghost-cursor",
  "selectedAction": "SHOW_CURSOR",
  "confidence": 0.82,
  "reason": "hesitation_detected",
  "suppressedRecommendations": [
    "reading-assistant"
  ]
}
```

Decision records support analytics and replay.

---

# Explainability

Every decision should be explainable.

The engine should answer:

- Why did this appear?
- Why was another recommendation ignored?
- Why did nothing happen?

Opaque behaviour is discouraged.

---

# Failure Handling

If the Decision Engine encounters invalid recommendations:

- discard malformed entries
- log diagnostics
- continue evaluating remaining recommendations

The engine should fail gracefully.

---

# Future Enhancements

Potential improvements include:

- adaptive confidence thresholds
- reinforcement learning
- user preference profiles
- contextual policy packs
- collaborative decision models
- cross-device reasoning

These enhancements must remain compatible with the existing pipeline.

---

# Guiding Philosophy

The purpose of the Decision Engine is not to maximize interventions.

It is to maximize relevance.

A quiet interface that intervenes at the right moment is more valuable than an active interface that constantly competes for attention.

Every decision should preserve the user's flow rather than redirect it.
