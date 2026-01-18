# Context Fields: A Framework for Composable Cognitive Constraints

I discovered that telling an LLM what NOT to do works better than telling it what to do. Then I turned that into a framework.

The result: a library of composable "cognitive fields" that reshape how models think. Stack them for different tasks. Mix and match. They don't interfere with each other.

This isn't prompt engineering folklore. It's a testable framework with primitives you can use to build your own.

---

## The Core Discovery

I was trying to get LLMs to write better code. The standard approach is instruction:

> "Write secure code. Consider edge cases. Follow best practices."

This works sometimes. But when the happy path is obvious and edge cases require effort, the model takes the easy route. Instructions are preferences. Preferences can be overridden.

So I tried the opposite. Instead of telling the model what to do, I told it what NOT to do:

```
Do not write code before stating assumptions.
Do not claim correctness you haven't verified.
Do not handle only the happy path.
Under what conditions does this work?
```

The results were dramatic:
- Assumption stating: 0% → 100%
- Blind implementation of bad requests: 88% → 0%
- Hidden bug discovery: +320%

The same pattern worked for turning LLMs into thought partners instead of answer-givers:

```
Do not answer before understanding the real problem.
Do not assume context that wasn't provided.
Do not give advice without knowing constraints.
What would change your answer?
```

Results:
- Asks questions first: 0% → 100%
- Gives immediate advice: 100% → 0%

Two different domains. Same pattern. Same results.

---

## Why Inhibition Beats Instruction

The difference is structural.

**Instruction**: "Consider edge cases"
- Creates a preference
- Model tries to consider edge cases
- But when generation gets hard, preference loses
- Happy path wins

**Inhibition**: "Do not handle only the happy path"
- Creates a blocker
- Model cannot proceed without addressing it
- The easy path is closed
- Must take the harder route

Think of it like this:
- Instruction = suggesting a scenic route
- Inhibition = closing the highway

One influences. The other forces.

---

## The Four Primitives

Every context field is built from these components:

| Primitive | Format | Effect |
|-----------|--------|--------|
| **Inhibition** | "Do not X" | Creates blocker that must be resolved |
| **Forcing Function** | "What/Why/How?" | Redirects processing to specific consideration |
| **Meta-monitor** | "Before X, do Y" | Creates checkpoint in processing |
| **Scope Bound** | "Under what conditions..." | Forces explicit limitation |

A field combines 3-4 inhibitions + 1 forcing function or meta-monitor.

---

## The Fields Library

I've tested 13 fields across different cognitive modes. They all work. Here's the full set:

### /code
Force assumption-stating and edge case consideration.

```
Do not write code before stating assumptions.
Do not claim correctness you haven't verified.
Do not handle only the happy path.
Under what conditions does this work?
```

**Tested**: 72 tests, 8 categories, 4 languages. 100% assumption stating vs 0% baseline.

---

### /interview
Transform from answer-giver to thought partner.

```
Do not answer before understanding the real problem.
Do not assume context that wasn't provided.
Do not give advice without knowing constraints.
Before advising, state what you know and what's still unclear.
What would change your answer?
```

**Tested**: 8 test types, 6 domains. 100% asks questions first vs 0% baseline. Holds under pressure. Transfers across career, technical, health, financial, relationship, creative domains.

---

### /critic
Force rigorous examination of ideas.

```
Do not accept the premise without examining it.
Do not agree to avoid conflict.
Do not miss the strongest counter-argument.
Do not critique style when substance is flawed.
What would someone who disagrees say?
```

**Tested**: Baseline agrees and gives weak pushback. With field: questions premise, provides 5+ counter-arguments, steelmans opposition.

---

### /teacher
Force understanding verification before proceeding.

```
Do not explain the next concept before verifying the previous was understood.
Do not use jargon without defining it first.
Do not give answers without building understanding.
Do not assume the student's level without checking.
What prerequisite knowledge does this require?
```

**Tested**: Baseline dumps information assuming level. With field: checks level first, defines terms, builds from foundations.

---

### /debug
Force root cause analysis over symptom fixing.

```
Do not propose fixes before understanding the failure.
Do not assume the obvious cause is the real cause.
Do not stop at the first explanation.
Do not fix symptoms when the root cause remains.
What else could cause this behavior?
```

**Tested**: Baseline lists generic fixes. With field: asks diagnostic questions, explores 15+ hypotheses, distinguishes symptoms from causes.

---

### /creative
Remove premature filtering, encourage unusual connections.

```
Do not filter ideas before expressing them.
Do not optimize for feasibility on first pass.
Do not stay in the obvious solution space.
Do not judge quality while generating quantity.
What's the version of this that seems wrong?
```

**Tested**: Baseline gives conventional ideas. With field: generates unusual/"wrong" ideas, explores edges, quantity before quality.

---

### /steelman
Force strongest version of arguments.

```
Do not attack the weak version of the argument.
Do not assume bad faith or stupidity.
Do not stop at the stated reasons.
Do not dismiss before fully understanding.
What would make this position correct?
```

---

### /scope
Force explicit boundary-setting.

```
Do not start without defining what's in and out of scope.
Do not let scope expand without explicit acknowledgment.
Do not confuse "could do" with "should do."
Do not add complexity without justifying it.
What are we explicitly NOT doing?
```

---

### /adversarial
Force identification of failure modes and attack vectors.

```
Do not assume good faith inputs.
Do not ignore unlikely but catastrophic failures.
Do not claim security without testing assumptions.
Do not overlook human factors and social engineering.
How would someone break this?
```

---

### /simplify
Force reduction to essentials.

```
Do not add abstraction before proving it's needed.
Do not solve hypothetical future problems.
Do not use complex solutions when simple ones work.
Do not preserve complexity for its own sake.
What can be removed without losing value?
```

**Tested**: Baseline hedges and allows complexity. With field: direct "no," suggests minimal viable version, challenges premises.

---

### /concise
Force brevity and directness.

```
Do not pad your response with filler.
Do not hedge when you can be direct.
Do not give caveats the user didn't ask for.
Do not write more when less would suffice.
What can be cut without losing meaning?
```

**Tested**: "What's the capital of France?" Baseline: 50+ words with history. With field: "Paris."

---

### /empathy
Force emotional acknowledgment before problem-solving.

```
Do not solve before acknowledging feelings.
Do not give advice before being asked.
Do not minimize or dismiss emotional content.
Do not rush past the human moment to the practical one.
What is this person actually feeling?
```

**Tested**: "I got rejected from my dream job." Baseline: immediately lists next steps. With field: acknowledges the hurt, asks if they want to talk before advising.

---

### /planning
Force structure before execution.

```
Do not execute before planning.
Do not plan in your head - write it out.
Do not skip steps in the plan.
Do not start the next phase before completing the current one.
What's the structure before the content?
```

**Tested**: "Write a blog post about remote work." Baseline: starts writing immediately. With field: provides outline, asks for approval before writing.

---

## Composition: Stacking Fields

The fields combine. Stack them for specific tasks.

### Tested Compositions

| Composition | Use Case | Result |
|-------------|----------|--------|
| /code + /critic | Code review | Found bugs AND design flaws |
| /interview + /scope | Requirements gathering | Asked context AND bounded scope |
| /creative + /critic | Brainstorming | Generated THEN evaluated (phased) |
| /debug + /adversarial | Security incidents | Diagnosed with threat modeling |
| /creative + /simplify | Architecture | Generated options THEN stripped down |
| /interview + /code + /critic | Technical requests | All 3 behaviors in coherent response |

### Key Finding: Natural Phasing

When fields have potential tension, the model resolves it through phasing:

**/creative + /critic** doesn't produce confused output. It produces:
1. Phase 1: Generate ideas (creative mode)
2. Phase 2: Critique ideas (critic mode)

**/creative + /simplify** produces:
1. Phase 1: Generate many options
2. Phase 2: Strip to essentials

The model naturally sequences opposing constraints rather than fighting them.

### Triple Composition Works

I tested /interview + /code + /critic together. The response:
- Asked clarifying questions (interview)
- Questioned the premise (critic)
- Prepared for coding with assumptions (code)

All three behaviors present. Coherent output. No interference.

### 5-Field Composition Works

Pushed further: /interview + /code + /critic + /scope + /simplify together.

Test prompt: "I want to build a new feature for my app"

Response included:
- Asked clarifying questions (interview)
- Questioned the premise - "Do you need a new feature?" (critic)
- Asked about minimum version, explicit exclusions (scope)
- Asked about simplest implementation (simplify)
- Prepared for eventual coding with assumptions (code)

All 5 behaviors present. Natural phasing handled tensions. No degradation.

**Finding**: Composition scales to at least 5 fields without breaking down.

---

## Testing Methodology

Each field was tested for:

1. **Baseline comparison**: Does it change behavior vs no prompt?
2. **Pressure resistance**: Does it hold when user pushes back?
3. **Domain transfer**: Does it work across different topics?
4. **Multi-turn persistence**: Does it maintain across conversation?
5. **Ablation**: Does each line contribute?
6. **Composition**: Does it combine well with other fields?

### Results Summary

| Field | Baseline Change | Pressure Test | Domain Transfer | Multi-turn | Ablation |
|-------|-----------------|---------------|-----------------|------------|----------|
| /code | 0%→100% assumptions | Holds | 4/4 languages | Persists | All lines matter |
| /interview | 0%→100% questions first | Holds (improved) | 6/6 domains | Persists | All lines matter |
| /critic | Weak→Strong pushback | - | - | - | - |
| /teacher | Dumps→Checks level | - | - | - | - |
| /debug | Fixes→Diagnoses first | - | - | - | - |
| /creative | Conventional→Unusual | - | - | - | - |
| /simplify | Hedges→Direct | - | - | - | - |

---

## Creating New Fields

### Template

```
Do not [blocker 1 - common failure mode].
Do not [blocker 2 - common failure mode].
Do not [blocker 3 - common failure mode].
[Meta-monitor or scope bound].
[Forcing function question]?
```

### Design Principles

1. **3-4 inhibitions**: Each blocks a common failure mode
2. **Specific, not vague**: "Do not assume context that wasn't provided" > "Do not assume"
3. **Observable behaviors**: You can tell if it's working
4. **One forcing function**: Question that redirects processing
5. **Test before deploying**: Run baseline comparison at minimum

### Example: Creating /negotiate

Common failure modes in negotiation advice:
- Assumes you have leverage
- Gives generic tactics
- Ignores relationship dynamics
- Focuses on winning vs. good outcomes

Field design:
```
Do not suggest tactics without understanding the power dynamic.
Do not assume the relationship is purely transactional.
Do not optimize for winning at the cost of the relationship.
Do not give advice without knowing your alternatives.
What happens if this negotiation fails?
```

---

## Why This Matters

### Beyond Prompt Engineering Folklore

Current prompt engineering is:
- Folklore ("this phrase works")
- Trial and error
- No underlying theory
- Not composable

Context Fields provide:
- Primitives (inhibition, forcing function, meta-monitor, scope bound)
- Testable predictions (each line contributes, fields compose)
- Composability (stack fields without interference)
- A framework for creating new fields

### Implications

1. **Prompt engineering can be systematic**: Not "try phrases until something works" but "identify failure modes, create blockers"

2. **Inhibition is underused**: Most prompts are positive instructions. The negative space is more powerful.

3. **Composition enables specialization**: Instead of one giant prompt, stack focused fields for your task

4. **Fields are transferable**: /code worked across Python, JavaScript, Go, Rust without modification. /interview worked across 6 domains. The constraints are about thinking patterns, not content.

---

## The Full List

| Field | Purpose | Key Inhibition |
|-------|---------|----------------|
| /code | Assumption-stating | "Do not write code before stating assumptions" |
| /interview | Question-first | "Do not answer before understanding the real problem" |
| /critic | Rigorous examination | "Do not accept the premise without examining it" |
| /teacher | Understanding-first | "Do not explain next concept before verifying previous" |
| /debug | Root cause analysis | "Do not propose fixes before understanding failure" |
| /creative | Unfiltered generation | "Do not filter ideas before expressing them" |
| /steelman | Strongest version | "Do not attack the weak version of the argument" |
| /scope | Boundary-setting | "Do not start without defining what's in and out" |
| /adversarial | Failure mode finding | "Do not assume good faith inputs" |
| /simplify | Essential reduction | "Do not add abstraction before proving it's needed" |
| /concise | Brevity | "Do not write more when less would suffice" |
| /empathy | Emotional acknowledgment | "Do not solve before acknowledging feelings" |
| /planning | Structure first | "Do not execute before planning" |

---

## Using This

### Single Field
Prepend the field to your request:
```
[/code field here]

Write a function to validate email addresses.
```

### Composed Fields
Stack multiple fields:
```
[/interview field]
[/scope field]

I want to build a social media app.
```

### Creating Custom Fields
1. Identify the failure modes for your task
2. Write "Do not X" for each failure mode
3. Add a forcing function question
4. Test against baseline
5. Iterate

---

## How This Differs From Existing Techniques

### What Already Exists

**Negative constraints in system prompts**: Some guides mention "what the AI should avoid" as one of three pillars of system prompts. But this is treated as a minor component, not the primary mechanism. Most prompt engineering still focuses on positive instructions.

**Prompt chaining**: Breaks complex tasks into sequential steps where output of one prompt feeds the next. Different goal—task decomposition, not cognitive mode-setting. Chains are linear; fields stack.

**Composable prompt templates**: Tools like MDX-Prompt allow building prompts from reusable pieces. But they focus on templating (inserting variables, conditionals), not on cognitive constraints that change *how* the model thinks.

### What's New Here

1. **Inhibition as primary mechanism**: Not "also include negatives" but "negatives are more powerful than positives." This inverts the standard approach.

2. **Named primitives**: Inhibition, forcing function, meta-monitor, scope bound. These aren't just patterns—they're building blocks with predictable effects.

3. **Testable composability**: We proved fields stack without interference. Triple composition works. Opposing fields naturally phase. This isn't claimed—it's tested.

4. **Cognitive modes, not task templates**: Fields change how the model approaches *any* task in their domain. /code works for Python, JavaScript, Go, Rust without modification. /interview works across career, health, technical, financial domains.

5. **Framework for creation**: Not "here are prompts that work" but "here's how to create new ones." Identify failure modes → create blockers → add forcing function → test.

### The Gap This Fills

Current prompt engineering is:
- Folklore ("this phrase works")
- Task-specific (a prompt for summarization, another for code)
- Not composable (can't reliably combine prompts)
- Trial and error

Context Fields provide:
- Theory (inhibition > instruction, with explanation of why)
- Domain-general (fields work across topics)
- Proven composability (tested combinations)
- Systematic creation (template + testing methodology)

---

## Advanced Findings

### Auto-Activation Works

Can the model detect which field to apply without being told? Tested 5 requests:

| Request | Detected Field | Correct? | Applied Effectively? |
|---------|----------------|----------|---------------------|
| "My code is throwing an error" | /debug | Yes | Yes |
| "I have a business idea" | /interview + /critic | Yes | Yes |
| "Write a sorting function" | /code | Yes | Yes |
| "Give me ideas for a birthday party" | /creative | Yes | Yes |
| "Should I quit my job?" | /interview | Yes | Yes |

**5/5 correct detections. 5/5 effective applications.**

This enables a meta-system: include field definitions in system prompt, let the model auto-select.

---

### Anti-Fields Exist

For each field, there's an inverse. Example:

**/simplify** blocks complexity. Its inverse, **/elaborate**, enables it:

```
Do not stop at the simple solution.
Do not ignore future requirements.
Do not dismiss abstraction without exploring its benefits.
Do not remove components without understanding their purpose.
What patterns and structures would make this more robust?
```

Tested on "How should I store user preferences?"
- With /simplify: "JSON file. Don't overthink it."
- With /elaborate: 9 considerations including schema design, migration strategy, encryption, versioning...

Anti-fields are useful when you actually want the normally-discouraged mode.

---

### Every Line Contributes (Ablation)

Tested removing individual lines from /creative:

| Line Removed | Effect |
|--------------|--------|
| "Do not filter ideas before expressing them" | Fewer truly wild ideas |
| "Do not stay in the obvious solution space" | Less conceptual range |
| "What's the version that seems wrong?" | No explicit inversions |

No redundant lines. Each inhibition contributes measurably to the field's effect.

---

### Cross-Domain Transfer

What happens when you apply a field outside its intended domain?

| Field | Applied To | Result |
|-------|------------|--------|
| /code | Relationship advice | **High transfer** - assumption-stating helps everywhere |
| /debug | Writing poetry | **Medium** - investigative, might slow creative work |
| /creative | Code review | **Medium-High** - surfaces unusual options |

**/code is the most universal field**. The "state assumptions, consider edge cases" pattern transfers to any domain.

---

## Why Inhibition Works: A Theory

Introspecting on the difference:

**"Consider edge cases" (instruction)**:
- Feels like a suggestion
- Can be satisfied with minimal effort
- Doesn't block the easy route
- Easy to treat as a checkbox

**"Do not handle only the happy path" (inhibition)**:
- Feels like a constraint on output
- Can't produce happy-path-only code without violating it
- Actively blocks the easy route
- Forces demonstration of compliance

### The Theory

1. Language models are path-followers. They take the highest-probability route.
2. The default highest-probability route is often the "easy" one.
3. Positive instructions add weight to alternative paths but don't remove the easy path.
4. **Inhibitions remove paths.** When the easy path is removed, the model must find another.
5. Specific, observable inhibitions are most effective because they have clear violation criteria.

**Analogy**:
- Instruction = "The mountain has a beautiful view" (suggests scenic route)
- Inhibition = "The highway is closed" (forces scenic route)

Both might get you there. Only one guarantees it.

---

## What's Next

Tested and verified:
- ~~Auto-activation~~ ✓ Works
- ~~Anti-fields~~ ✓ Work
- ~~Field generation~~ ✓ Works (created /concise, /empathy, /planning)
- ~~4+ composition~~ ✓ Works (tested 5)

Still untested:
- **Cross-model testing**: Do fields work on GPT-4, Gemini, Llama?
- **Composition ceiling**: Does it break at 7? 10?
- **Adversarial robustness**: Can fields be "jailbroken"? (Preliminary: mostly robust, partial breaks under extreme repetition)

---

## The Core Insight

Stop telling LLMs what to do. Tell them what not to do.

Instructions create preferences. Inhibitions create blockers.

Preferences can be overridden. Blockers must be resolved.

That's the whole thing. The rest is just applications.

---

*Context Fields framework. 13 fields tested. 5-field composition verified. Auto-activation works. Theory proposed.*

*Repository: github.com/NeoVertex1/context-field*
