# Motion System

> The motion language for UX Intelligence Engine.

Motion is the physical expression of intelligence.

When the system decides to assist a user, the interaction should feel:

- intentional
- calm
- lightweight
- predictable
- respectful

The goal is not to attract attention.

The goal is to communicate presence.

---

# Purpose

The Motion System defines:

- Animation principles
- Timing standards
- Easing behaviour
- Transition patterns
- Cursor movement
- Assistant appearance
- Feedback states
- Reduced motion alternatives

It does not define:

- When interventions happen
- Why interventions happen
- Plugin behaviour
- Decision logic

Those responsibilities belong to:

- Decision Engine
- Plugin System

---

# Motion Philosophy

UX Intelligence Engine follows three principles.

---

# 1. Presence Over Attention

Traditional UI animations attempt to capture attention.

UX Intelligence Engine aims to establish presence.

Example:

Bad:large popup + animation sound


Good:Subtle movement, clear affordance


---

# 2. Physics Over Effects

Motion should feel like a physical object.

Avoid:

- random animations
- excessive scaling
- unnatural movement
- unnecessary transitions

Prefer:

- spring behaviour
- natural acceleration
- smooth deceleration
- spatial consistency

---

# 3. Continuity Over Surprise

The user should understand where something came from.

Example:

Cursor assistant:

Pointer position
↓
Small movement
↓
Bubble follows
↓
Message appears


The user should never wonder:

"Where did this come from?"

---

# Motion Layers

The system uses four motion layers.

Micro Motion
↓
Component Motion
↓
Interaction Motion
↓
Scene Motion


---

# Micro Motion

Small feedback responses.

Examples:

- button hover
- cursor pulse
- icon movement
- state change

Characteristics:

Duration:

100ms - 200ms


Purpose:

Provide immediate feedback.

---

# Component Motion

Movement of individual UI elements.

Examples:

- assistant bubble
- tooltip
- floating action

Characteristics:

Duration:

200ms - 400ms


Purpose:

Communicate state transition.

---

# Interaction Motion

Complex user flows.

Examples:

- onboarding sequence
- error recovery
- guided assistance

Characteristics:

Duration:

400ms - 800ms


Purpose:

Create continuity.

---

# Scene Motion

Large environmental transitions.

Examples:

- page transitions
- immersive portfolio experiences
- storytelling moments

Characteristics:

Duration:

800ms+


Purpose:

Create narrative movement.

---

# Timing Principles

Motion timing should communicate importance.

| Interaction | Duration |
|---|---|
| Hover response | 100-150ms |
| Button feedback | 150-200ms |
| Tooltip appearance | 200-300ms |
| Cursor movement | 300-600ms |
| Assistant entrance | 300-500ms |
| Complex guidance | 500-800ms |

---

# Easing

Motion should avoid mechanical movement.

Preferred easing:

## Entrance

Object appearing:

Ease Out


Reason:

Objects enter quickly and settle naturally.

---

## Exit

Object disappearing:

Ease In


Reason:

Objects leave with less emphasis.

---

## Continuous Movement

Examples:

- cursor tracking
- floating elements

Use:

Spring physics


---

# Spring Motion

Spring motion should define:

- tension
- friction
- mass

Avoid:

- excessive bounce
- playful movement
- unstable positioning

The engine should feel intelligent, not animated.

---

# Ghost Cursor Motion

The Ghost Cursor represents assistance presence.

It should:

- follow naturally
- avoid exact pointer tracking
- maintain personal space
- reduce visual noise

Recommended behaviour:

User stops
↓
Cursor appears nearby
↓
Cursor waits
↓
Bubble fades in
↓
Interaction available


---

# Cursor Distance

The assistant should not overlap the user's pointer.

Recommended offset:

Desktop:

24px - 48px

Large screens:

48px - 72px


Mobile equivalent:

Use bottom or edge anchored surfaces.

---

# Appearance Sequence

Assistance should appear progressively.

Example:

Opacity: 0
↓
Cursor appears
↓
Opacity: 0.8
↓
Bubble appears
↓
Text fades in


Avoid immediate full appearance.

---

# Dismissal Motion

Disappearance should be quieter than appearance.

Example:

Entrance:

300ms

Exit:

200ms

The system should leave without demanding attention.

---

# Interaction States

Every animated element should support:

## Idle

No active interaction.

---

## Appearing

Element entering the interface.

---

## Active

User can interact.

---

## Processing

System is thinking/loading.

---

## Success

Task completed.

---

## Error

Recovery needed.

---

## Dismissed

Interaction removed.

---

# Reduced Motion

The system must respect:

prefers-reduced-motion


When enabled:

Replace:

Movement
↓
Fade

Replace:
Spring
↓
Instant transition

Replace:
Parallax

Parallax
↓
Static position

Accessibility overrides animation preference.

---

# Mobile Motion

Mobile interactions require different principles.

Avoid:

- cursor movement
- hover effects
- floating objects near touch targets

Prefer:

- bottom sheets
- edge indicators
- expandable cards
- contextual banners

---

# Performance Guidelines

Motion should maintain:
60 FPS

Avoid:

- layout-triggering animations
- excessive DOM movement
- continuous expensive calculations

Prefer animating:

- transform
- opacity

Avoid animating:

- width
- height
- position

---

# Testing Motion

Motion should be tested for:

## Usability

Does it communicate meaning?

## Accessibility

Does it respect user settings?

## Performance

Does it remain smooth?

## Perception

Does it feel intentional?

---

# Anti-Patterns

Avoid:

## Notification Behaviour

The system should not behave like:

- alerts
- banners
- advertisements

---

## Over Animation

Avoid:

- bouncing
- shaking
- pulsing constantly

---

## Decorative Motion

Every movement must communicate purpose.

---

# Future Extensions

Potential improvements:

- adaptive motion based on user preference
- personalized animation intensity
- physics-based cursor behaviour
- gesture-aware transitions
- spatial computing interactions

---

# Guiding Principle

Motion is not decoration.

Motion is communication.

A good animation explains:

- something appeared
- something changed
- something is available
- something is complete

The best motion is noticed once and understood forever.
