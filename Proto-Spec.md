# Spec Format for LLM-Powered Development

This template defines a **standard format** for writing specifications that LLMs can consume to generate, validate, or extend software artifacts. It is **domain-agnostic** and can be applied to any feature, component, or system.

---

## Metadata

* **Iteration**: number (e.g., Iteration 1, Iteration 2).
* **Date**: ISO timestamp.
* **Title**: Short descriptive name of the feature.
* **Intent**: build | refactor | fix | spike.

## Context

* **Problem Statement**: Why this feature or change is needed.
* **Scope**: What is explicitly in scope.

## Requirements

* **Defaults**: Any initial states, defaults, or assumptions.
* **Features**: Bullet list of expected capabilities.
* **Constraints**: Performance, security, a11y, i18n, etc.
* **Persistence**: What state is saved, how, and where.
* **Import/Export**: Supported formats and versioning rules.

## UI Specification

Describe surfaces, layouts, and actions. Include structure of major elements (buttons, lists, modals, forms) and their intended interactions.

## State Model

* **Entities**: Core data structures with fields and constraints.
* **Derived State**: Computed values.
* **Identifiers**: Runtime-created entities must use UUID v4.

## Commands & Events

* **Commands**: Imperative actions triggered by user/system.
* **Events**: Domain events recording state changes.

## Acceptance Criteria

Scenarios in Gherkin-like style:

* **AC-1**: Given , When , Then .
* **AC-2**: …

## Schemas

Define data payloads, if needed, using JSON Schema or simple field descriptions.

## Non-Goals

* Explicitly list what will **not** be delivered in this iteration.

## Changelog

* Iteration 1 → 2: list changes.
* Iteration 2 → 3: list changes.

---

### Usage

When prompting an LLM:

1. Fill this template with the feature description.
2. Number the iteration.
3. Provide schema + acceptance criteria for validation.
4. Use **Non-Goals** and **Changelog** to scope and track evolution.
