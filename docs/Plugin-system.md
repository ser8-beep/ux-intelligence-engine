# Plugin System

> Everything is a plugin.

UX Intelligence Engine intentionally ships with very little built into the core runtime.

Instead, nearly every observable behaviour is implemented as an independent plugin.

Ghost Cursor.

Reading Assistant.

Error Recovery.

Exit Intent.

Accessibility Helpers.

Analytics.

Even future AI-powered capabilities are expected to exist as plugins rather than core functionality.

This philosophy keeps the runtime small, predictable and extensible.

---

# Design Principles

The plugin system is built around six principles.

## 1. Single Responsibility

A plugin should solve one problem.

Good:

- Ghost Cursor
- Error Recovery
- Reading Assistant

Bad:

- "Everything Assistant"

Plugins should remain focused.

---

## 2. Isolation

Plugins never communicate directly.

Instead:

```
Plugin A

↓

Event Bus

↓

Plugin B
```

This prevents hidden coupling and circular dependencies.

---

## 3. Predictability

Plugins never manipulate the UI directly.

Instead they publish recommendations.

```
SHOW_CURSOR

confidence = 0.84
```

The Decision Engine decides whether that recommendation becomes reality.

---

## 4. Composability

Multiple plugins should be able to operate simultaneously.

Example:

```
Ghost Cursor

+

Reading Assistant

+

Analytics

+

Accessibility
```

Each performs a different responsibility.

---

## 5. Replaceability

Developers should be able to remove any plugin without affecting the runtime.

Removing Ghost Cursor should not impact:

- Analytics
- Error Recovery
- Session Intelligence

---

## 6. Runtime Safety

Plugins should never be capable of crashing the engine.

Plugin failures are isolated.

---

# Plugin Lifecycle

Every plugin follows the same lifecycle.

```
Install

↓

Initialize

↓

Register

↓

Receive Events

↓

Produce Recommendations

↓

Cooldown

↓

Dispose
```

---

# Plugin Contract

Every plugin must implement a common interface.

```ts
interface UXPlugin {

    id: string

    version: string

    priority: number

    initialize()

    subscribe()

    execute()

    dispose()

}
```

Plugins should remain stateless whenever possible.

Persistent information belongs inside Session Context.

---

# Registration

Plugins register themselves during application startup.

```
Runtime

↓

Plugin Manager

↓

Plugin Registry

↓

Active Plugin List
```

Registration should be deterministic.

Plugin load order must never affect runtime behaviour.

---

# Discovery

The runtime discovers plugins through the Plugin Registry.

Each plugin provides metadata.

Example:

```json
{
    "id": "ghost-cursor",

    "version": "1.0",

    "author": "Open Source",

    "capabilities": [
        "cursor",
        "hint"
    ]
}
```

Metadata enables inspection before execution.

---

# Plugin Context

Each execution receives a read-only context.

Example:

```
Current Session

Current Page

Device

Accessibility Settings

Recent Events

Derived Intent

Runtime Configuration
```

Plugins must never mutate context directly.

---

# Plugin Output

Plugins never render.

They emit recommendations.

Example:

```json
{
    "action": "SHOW_CURSOR",

    "confidence": 0.83,

    "payload": {

        "message": "Need a hint?",

        "style": "ghost"

    }
}
```

The renderer never receives plugin instances.

Only approved decisions.

---

# Priority

Some recommendations are more important than others.

Example priority order:

```
Error Recovery

↓

Accessibility

↓

Exit Intent

↓

Ghost Cursor

↓

Reading Assistant

↓

Analytics
```

Priority influences conflict resolution but does not guarantee execution.

Confidence and runtime policies are also considered.

---

# Cooldowns

Plugins may define cooldown periods.

Example:

```
Ghost Cursor

60 seconds

Reading Assistant

90 seconds

Error Recovery

0 seconds
```

Cooldowns prevent repetitive interventions.

---

# Dependencies

Plugins should avoid dependencies on one another.

If required, dependencies must be declared explicitly.

Example:

```
Accessibility Plugin

↓

Ghost Cursor Plugin
```

Meaning:

Ghost Cursor requires accessibility settings before rendering.

Circular dependencies are prohibited.

---

# Permissions

Plugins request capabilities.

Example:

```
Read Events

Read Context

Publish Recommendations

Publish Analytics

Read Accessibility
```

Plugins cannot request unrestricted runtime access.

---

# Configuration

Every plugin exposes configuration options.

Example:

```json
{
    "enabled": true,

    "confidenceThreshold": 0.75,

    "cooldown": 60000
}
```

Configuration should never require source code modification.

---

# Execution Model

Each incoming event triggers the following process.

```
Receive Event

↓

Filter

↓

Evaluate

↓

Generate Recommendation

↓

Return
```

Plugins should execute quickly.

Long-running work should be delegated to background services.

---

# Recommendation Confidence

Recommendations include confidence scores.

Example:

```
Ghost Cursor

↓

0.81

Reading Assistant

↓

0.62

Exit Intent

↓

0.91
```

Confidence allows the Decision Engine to choose the most appropriate intervention.

---

# Plugin Categories

The engine recognises several plugin types.

## Intent Plugins

Produce behavioural signals.

Examples:

- Hesitation Detector
- Reading Detector
- Exploration Detector

---

## Interaction Plugins

Recommend UI interventions.

Examples:

- Ghost Cursor
- Reading Assistant
- Error Recovery

---

## Context Plugins

Enrich session information.

Examples:

- Device Detection
- Returning Visitor
- Accessibility

---

## Analytics Plugins

Observe runtime behaviour.

Examples:

- Telemetry
- Session Replay
- Metrics

---

## AI Plugins

Integrate reasoning models.

Examples:

- LLM Decision Provider
- Message Generator
- Intent Summariser

---

# Failure Handling

Plugin failures are isolated.

If one plugin throws an exception:

```
Ghost Cursor

↓

Failure

↓

Runtime Logs Error

↓

Remaining Plugins Continue
```

The engine must continue operating.

---

# Testing

Every plugin should provide:

- Unit tests
- Configuration validation
- Performance benchmarks
- Failure scenarios

Plugins should be testable without rendering UI.

---

# Versioning

Plugins follow Semantic Versioning.

```
Major

Breaking Changes

Minor

New Features

Patch

Bug Fixes
```

The runtime validates compatibility during registration.

---

# Future Extensions

Planned capabilities include:

- Hot plugin reloading
- Remote plugin registry
- Marketplace support
- Permission sandboxing
- WebAssembly plugins
- Visual plugin debugger
- Plugin dependency graph

---

# Guiding Philosophy

The runtime should not become more complex as new capabilities are added.

Instead, complexity should move outward into independent plugins.

If a behaviour can be removed without affecting the rest of the engine, it probably belongs in a plugin.
