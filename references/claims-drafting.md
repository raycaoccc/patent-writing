# Claims Drafting Reference

## Core Principle

**Draft the broadest defensible independent claim first, then systematically narrow with dependent claims.**

## Claim Types

| Claim Type | Protects | Example Preamble |
|-----------|----------|-----------------|
| Method claim | Process/steps | "A method for..., comprising:" |
| System/apparatus claim | Physical implementation | "A system for..., comprising:" |
| Computer-readable medium | Stored instructions | "A non-transitory computer-readable medium storing instructions that, when executed by a processor, cause the processor to:" |
| Product claim | Physical product | "A device comprising:" |

## Independent Claim Structure

### Two-Part Form (CNIPA/EPO)
```
[Preamble], characterized in that [distinguishing features]
一种[发明名称]，其特征在于，[区别技术特征]
```

### US Open-Ended Form
```
A method for [purpose], comprising:
  [step a];
  [step b]; and
  [step c].
```

### Key Rules
1. Include ONLY essential features (every element = required for infringement)
2. Use functional language ("a processor configured to...") over structural specifics
3. Use superordinate concepts (上位概念) — "computing device" not "server"
4. Single sentence grammar
5. First mention: "a/an" (一种/一个), subsequent: "the/said" (所述)

## Dependent Claim Strategy

### Construction Techniques
- **"wherein said [element]..."** — further defines existing element
- **"further comprising..."** — adds new element
- Chinese: "根据权利要求X所述的[发明名称]，其特征在于，[附加技术特征]"

### Hierarchical Strategy (Defense in Depth)
```
Claim 1 (independent): Broadest scope — core invention
  Claim 2: Specifies component A (e.g., architecture)
  Claim 3: Specifies component B (e.g., training method)
    Claim 4 (depends on 3): Narrows B (e.g., loss function)
  Claim 5: Data preprocessing
  Claim 6: Hardware deployment
  Claim 7: Performance constraint

Claim 8 (independent): System claim mirroring Claim 1
  Claim 9-12: System-level dependent claims

Claim 13 (independent): Computer-readable medium claim
```

### Dependent Claim Categories
1. **Component specifics** — what type of X
2. **Parameter ranges** — numerical values, thresholds
3. **Alternative embodiments** — other ways to achieve same function
4. **Performance characteristics** — speed, accuracy
5. **Integration details** — connections to other systems
6. **Data characteristics** — input/output formats

## Transition Phrases (Critical)

| Phrase | Scope | Chinese | Use When |
|--------|-------|---------|----------|
| **"comprising"** | Open-ended | 包括/包含 | Default for most claims |
| **"consisting of"** | Closed | 由...组成 | Exact formulations |
| **"consisting essentially of"** | Semi-open | 基本上由...组成 | Intermediate |

**Default: ALWAYS use "comprising".**

## 5-Step Drafting Process

1. **Identify Core Innovation** — minimum distinguishing features
2. **Draft Broadest Independent Claim** — functional language, superordinate concepts
3. **Build Dependent Claim Tree** — meaningful narrowing, cover alternatives
4. **Mirror with Different Claim Types** — method → system → medium
5. **Review** — antecedent basis, terminology consistency, divided infringement

## AI/ML Claim Patterns

### Training Method Claim (Separate from Inference)
```
A method for training a neural network, comprising:
  receiving a training dataset comprising [specific data type];
  preprocessing the training dataset by [specific transformation];
  training a [specific architecture] neural network by:
    computing a [specific] loss function based on [specific criteria]; and
    updating network parameters using [specific optimizer]; and
  storing the trained neural network parameters.
```

### Inference Claim (Separate from Training)
```
A method for [specific technical task], comprising:
  receiving, by a processor, input data comprising [specific type];
  processing the input data through a trained [specific architecture]
    neural network to generate [specific output]; and
  [specific technical action] based on the [specific output].
```

### System Claim for AI
```
A system comprising:
  a memory storing a trained [specific] neural network model; and
  a processor coupled to the memory, configured to:
    receive input data comprising [specific type];
    execute the neural network model to generate [output]; and
    perform [specific technical action] based on the [output].
```

## Ten Deadly Sins of Claim Drafting

1. **Grammar violations** — Claims MUST be single sentences
2. **Claiming results instead of methods** — Patent the process, not the outcome
3. **Neglecting dependent claims** — They are your fallback defense
4. **Inconsistent terminology** — "motor" vs "drive mechanism"
5. **Overly broad claims** — Invites prior art rejection
6. **Overly narrow claims** — Allows easy design-around
7. **Wrong transition phrase** — "consisting of" when you meant "comprising"
8. **Using trademarks** — Use generic descriptions
9. **Means-plus-function traps** — Narrows to specification examples only
10. **Missing antecedent basis** — Every "the/said" needs a prior "a/an"

### AI-Specific Mistakes
- Mixing training and inference in one claim (divided infringement)
- Claiming abstract algorithms without technical anchoring
- Using only math formulas without technical context
- Failing to specify hardware/processor elements
- Claiming "using AI" without technical specificity

## Claim Count Guidelines

| Jurisdiction | Base Allowance | Recommendation |
|-------------|---------------|---------------|
| CNIPA | Fees per claim >10 | 15-25 claims, 2-3 independent |
| USPTO | 3 independent, 20 total | 20 claims, 3 independent |
| EPO | 15 claims base | 15-20 claims, 2-3 independent |
