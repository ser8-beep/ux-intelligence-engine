# Architecture

> UX Intelligence Engine is an event-driven runtime for adaptive user interfaces.

Unlike traditional UI frameworks that react only to explicit user actions, UX Intelligence Engine introduces an intelligence layer capable of interpreting interaction patterns and deciding whether the interface should respond.

The engine is intentionally modular. Every behavior—from a ghost cursor to onboarding assistance—is implemented as a plugin operating on shared event streams.

---

# Design Goals

The architecture is guided by five principles.

## 1. Events are the Source of Truth

No component owns user state.

Instead, every interaction is emitted as an immutable event.

Examples include:

- Pointer movement
- Scroll
- Touch
- Keyboard input
- Validation errors
- Network failures
- Focus changes
- Visibility changes

Higher-level behaviors are derived from combinations of these events rather than being hardcoded.

---

## 2. Intelligence is Separate from Presentation

Rendering should never contain business logic.

Instead:

```
User Interaction

↓

Events

↓

Intent Detection

↓

Decision

↓

Rendering
```

This separation allows the same reasoning engine to power:

- React
- Next.js
- Vue
- Framer
- Native applications

without modification.

---

## 3. Plugins are Independent

Every capability is implemented as a plugin.

Examples:

Ghost Cursor

Reading Assistant

Accessibility Companion

Error Recovery

Session Intelligence

Exit Intent Detection

Plugins communicate only through shared events.

No plugin directly calls another plugin.

This prevents hidden dependencies.

---

## 4. Confidence-Based Intervention

The engine does not intervene simply because a timer expires.

Instead it computes confidence.

Example:

```
User stops moving

↓

Repeated hover over same button

↓

Scrolls up

↓

Scrolls down

↓

Long pause

↓

Confidence:
0.81

↓

Offer assistance
```

If confidence remains below the configured threshold:

The engine intentionally does nothing.

Silence is considered a valid UX decision.

---

## 5. Deterministic Runtime

Although plugins may use AI reasoning, the runtime itself remains deterministic.

Given the same event history, the engine should produce the same output unless an AI policy explicitly overrides it.

This ensures:

- reproducibility
- debugging
- analytics
- testing

---

# Runtime Layers

The runtime is divided into six independent layers.

```
┌────────────────────────────┐
│ User Interface             │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│ Event Collection Layer     │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│ Intent Detection Layer     │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│ Plugin Runtime             │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│ Decision Engine            │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│ Renderer                   │
└────────────────────────────┘
```

---

# Layer Responsibilities

## Event Collection

Responsible for normalizing platform-specific events.

Desktop:

- Mouse
- Keyboard
- Scroll
- Window Focus

Mobile:

- Touch
- Gesture
- Scroll
- Viewport

Tablet:

- Hybrid pointer model

Output:

```
NormalizedEvent
```

---

## Intent Detection

Intent Detection converts raw events into behavioral signals.

Example:

Raw events:

```
mousemove

mousemove

pause

scroll

pause

mousemove
```

Become:

```
HesitationDetected
```

Other intent signals include:

- Reading
- Searching
- Exploration
- Confusion
- Exit Intent
- Task Completion

Intent Detection contains no UI logic.

---

## Plugin Runtime

The runtime executes every registered plugin.

Each plugin receives:

```
Current Context

+

Recent Events

+

Derived Intent Signals
```

Plugins never mutate global state.

Instead they produce recommendations.

Example:

```
SHOW_CURSOR

confidence = 0.82
```

---

## Decision Engine

Multiple plugins may recommend different actions.

Example:

Ghost Cursor

↓

SHOW_CURSOR

Reading Plugin

↓

SUPPRESS

Error Plugin

↓

OPEN_ASSISTANT

The Decision Engine resolves conflicts using:

- priority
- confidence
- cooldown rules
- accessibility settings
- user preferences

Only one final decision reaches the renderer.

---

## Renderer

Rendering is intentionally "dumb."

It knows only:

```
Render this.

Hide this.

Animate this.
```

It never asks why.

This keeps rendering portable across frameworks.

---

# Event Flow

A complete interaction follows this lifecycle.

```
User pauses

↓

Idle Event

↓

Intent Detection

↓

"Hesitation"

↓

Ghost Cursor Plugin

↓

Decision Engine

↓

Renderer

↓

Cursor Appears
```

No plugin directly manipulates the DOM.

---

# Plugin Lifecycle

Every plugin implements:

```
Initialize

↓

Subscribe

↓

Receive Events

↓

Produce Recommendation

↓

Cooldown

↓

Dispose
```

Plugins are stateless where possible.

Persistent data belongs to Session Context.

---

# Session Context

Session Context stores information shared across plugins.

Examples:

- Session duration
- Returning visitor
- Current page
- Active task
- Previous interventions
- Accessibility preferences
- Device type

Plugins may read context.

Only the Session Manager may update it.

---

# AI Integration

The LLM is **not** the runtime.

Instead it is an optional reasoning module.

```
Events

↓

Intent Detection

↓

Plugin Recommendations

↓

LLM (optional)

↓

Decision Engine
```

The AI cannot bypass runtime constraints.

Examples:

AI cannot:

- ignore cooldowns
- violate accessibility
- repeatedly interrupt users

The runtime remains authoritative.

---

# Mobile Architecture

Mobile replaces cursor-specific interactions with contextual surfaces.

Examples:

Desktop:

Ghost Cursor

↓

Floating Bubble

Mobile:

Bottom Sheet

↓

Floating Action Pill

↓

Context Banner

Intent detection remains identical.

Only rendering changes.

---

# Accessibility

Accessibility policies are enforced before rendering.

Examples:

If:

```
prefers-reduced-motion = true
```

Animations are replaced with fades.

If:

```
Keyboard Navigation
```

Ghost Cursor is disabled.

Focus guidance becomes primary.

Accessibility is not implemented as a plugin.

It is a runtime constraint.

---

# Extension Points

Developers may extend the engine by adding:

- Intent Detectors
- Plugins
- Renderers
- Analytics Providers
- AI Providers
- SDK Adapters

No modification to the runtime core should be required.

---

# Non-Goals

UX Intelligence Engine intentionally does **not** provide:

- Chatbot interfaces
- Analytics dashboards
- Design systems
- Component libraries
- Authentication
- CMS functionality

It is an orchestration engine.

---

# Guiding Philosophy

Traditional interfaces ask:

> "What just happened?"

UX Intelligence Engine asks:

> "What is the user trying to do?"

That distinction changes how interfaces behave.

Rather than reacting to clicks, the engine reasons about intent—and only intervenes when doing so meaningfully reduces friction.
