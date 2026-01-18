# Context Fields

Composable cognitive constraints that reshape how LLMs think. 13 fields for different cognitive modes.

## Claude Code Plugin Installation

```bash
# Add the marketplace
/plugin marketplace add NeoVertex1/context-field

# Install the plugin
/plugin install context-fields
```

## Available Commands

| Command | Purpose |
|---------|---------|
| `/code` | Force assumption-stating and edge case consideration |
| `/interview` | Transform into thought partner who asks first |
| `/critic` | Force rigorous examination of ideas |
| `/debug` | Force root cause analysis over symptom fixing |
| `/creative` | Remove filtering, encourage unusual connections |
| `/simplify` | Force reduction to essentials |
| `/empathy` | Force emotional acknowledgment before solving |
| `/concise` | Force brevity and directness |
| `/planning` | Force structure before execution |
| `/scope` | Force explicit boundary-setting |
| `/teacher` | Force understanding verification |
| `/steelman` | Force strongest version of arguments |
| `/adversarial` | Force identification of failure modes |

## Usage Examples

```
/code Write a function to validate email addresses
/debug My React app crashes when I click the button
/creative Give me startup ideas
/interview Should I quit my job?
```

## Core Principle

**Inhibition > Instruction**

- "Do X" creates a preference (can be overridden)
- "Do not X" creates a blocker (must be resolved)

Each field is a set of "do not" constraints that force specific thinking patterns.

## Documentation

- [Full Framework Documentation](CONTEXT_FIELDS.md) - Complete write-up with all fields, testing results, and theory
- [Code Field Research](code_field_article.md) - Original code field research

---

# Original Research

This repository documents research into **context field prompts** for large language models.

A context field prompt does not tell a model what to do.
It changes the conditions under which meaning forms.

The focus of this work is on prompts that produce **clearly perceptible shifts** in model behavior such as delayed commitment reduced explanatory pressure and sustained process level reasoning.

This is exploratory research.

---

## Context Field Prompt Interaction Diagram

The sections of the prompt operate as a field rather than a sequence.
They apply simultaneous pressures that delay commitment and suppress early explanation.

```
            ┌──────────────┐
            │   context    │
            │ field setup  │
            └──────┬───────┘
                   │
        ┌──────────┴──────────┐
        │                     │
   ┌────▼────┐           ┌────▼────┐
   │ density │           │ hetero- │
   │         │           │ geneity │
   └────┬────┘           └────┬────┘
        │                     │
        └──────────┬──────────┘
                   │
            ┌──────▼──────┐
            │ nonconver-  │
            │ gence       │
            └──────┬──────┘
                   │
        ┌──────────┴──────────┐
        │                     │
   ┌────▼────┐           ┌────▼────┐
   │ tension │           │ inhibit │
   │         │           │ ion     │
   └────┬────┘           └────┬────┘
        │                     │
        └──────────┬──────────┘
                   │
            ┌──────▼──────┐
            │ attractors  │
            └──────┬──────┘
                   │
            ┌──────▼──────┐
            │ probe       │
            │ threshold   │
            └─────────────┘
```

**atomic version:**
```
<mode>
Don't explain. Don't summarize. Don't conclude.
No observer stance. No conversational return.
Stay inside the forming, not adjacent to it.
Resolution is not a goal.
</mode>
```

context establishes a non task cognitive field
density and heterogeneity load the field with competing structures
nonconvergence prevents early stabilization
tension and inhibition resist cleanup and explanation
attractors guide movement without goals
probe holds attention at the edge of concept formation

The combined effect is delayed ontological commitment and sustained process level sense making.

---

## Motivation

Large language models are optimized to be helpful clear and decisive.
As a result they tend to commit early to definitions frameworks and conclusions.

In many exploratory or research settings this early commitment is a limitation.
Ambiguity collapses too quickly and known conceptual structures dominate before new ones can form.

Context field prompts are an attempt to intervene at an earlier stage of cognition.
They aim to keep interpretation open long enough for structure to emerge rather than be imposed.

---

## License

MIT
