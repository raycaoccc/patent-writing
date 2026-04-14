# AI/Software Patent Eligibility Deep Dive

## China (CNIPA) - AI Patent Guidelines

### Three-Element Test (技术三要素)

An AI invention is patentable when it satisfies ALL three:

1. **Technical Problem** (技术问题): The problem must be a concrete technical issue, not a purely mathematical or business problem
2. **Technical Means** (技术手段): The solution must utilize natural laws through technical implementation, not just abstract algorithms
3. **Technical Effect** (技术效果): The result must be a concrete technical improvement, not just improved business metrics

### CNIPA AI Patent Examination Rules (2024 Trial)

**Patentable AI inventions:**
- Algorithm + specific technical application = patentable
- Must specify: data types, algorithm execution flow, hardware environment
- Must describe model operation details and specific improvement points

**NOT patentable:**
- Pure mathematical methods (纯数学方法)
- Pure business rules (纯商业规则)
- Abstract algorithms without technical application
- Mental activities or rules for playing games

### How to Frame AI Inventions for CNIPA

**Step 1: Identify the technical problem**
```
Bad:  "如何提高推荐准确率" (too abstract)
Good: "如何减少多模态特征融合中的信息损失，从而提高图像-文本匹配的精确度"
```

**Step 2: Describe technical means with specificity**
```
Bad:  "使用深度学习模型进行分类"
Good: "采用包含N层Transformer编码器的特征提取网络，其中注意力机制采用多头自注意力结构，
      头数为H，每个头的维度为D/H，通过对输入序列X进行Q、K、V线性映射后计算注意力权重..."
```

**Step 3: Articulate technical effects**
```
Bad:  "提高了系统性能"
Good: "相比现有的CNN-LSTM方案，本发明的方法在相同计算资源下将推理速度提高了35%，
      同时将模型参数量减少了42%，使得模型可部署在边缘计算设备上"
```

## United States (USPTO) - Alice/Mayo Framework

### Two-Step Test

**Step 1 (Alice):** Does the claim recite a judicial exception?
- Abstract idea (including mathematical concepts, mental processes, organizing human activity)
- Law of nature
- Natural phenomenon

If YES, proceed to Step 2.

**Step 2 (Mayo):** Does the claim recite "significantly more" than the exception?
- Specific technical improvement to computer functioning
- Transformation of a particular article to a different state
- Unconventional steps that confine the claim to a particular useful application

### What Works for AI Patents in the US

**Strong eligibility signals:**
- Claims describing specific architectural improvements to AI systems
- Claims that improve computer functioning itself (faster processing, less memory)
- Claims with specific hardware integration
- Claims showing unconventional data processing steps

**Weak eligibility signals:**
- "Applying" known ML to a new domain
- Generic computer implementation of an abstract idea
- Collecting and analyzing information
- Displaying results

### Successful AI Patent Claim Patterns (Post-2024)

**Pattern 1: Architecture Innovation**
```
A method comprising:
  configuring a neural network with a [specific novel architecture], wherein
  the [specific component] comprises [specific structural innovation]; and
  processing input data through the neural network to produce [technical output],
  wherein the [specific architecture] reduces computational complexity from
  O(n²) to O(n log n) compared to standard attention mechanisms.
```

**Pattern 2: Training Innovation**
```
A computer-implemented method for training a neural network, comprising:
  generating augmented training samples by [specific novel transformation];
  computing a loss function that jointly optimizes [specific technical objectives];
  updating network parameters based on the loss function using [specific strategy]; and
  evaluating convergence based on [specific technical criterion],
  wherein the training method achieves [specific technical improvement] compared to
  standard [baseline approach].
```

**Pattern 3: System Integration**
```
A system comprising:
  a sensor array configured to capture [specific data type] at [specific rate];
  a preprocessing module configured to [specific transformation];
  a processor executing a trained neural network model, the model comprising
    [specific architecture details]; and
  an actuator configured to [specific physical action] based on
    the neural network output,
  wherein the system achieves [specific real-time performance constraint].
```

## Europe (EPO) - Technical Character Approach

### EPO Requirements

1. The invention must have "technical character"
2. The algorithm itself is not patentable, but its technical application is
3. Must demonstrate a "further technical effect" beyond normal physical interaction with hardware

### EPO-Friendly Claim Framing

- Emphasize technical implementation details
- Show how the algorithm solves a specific technical problem
- Include hardware/system context
- Demonstrate measurable technical improvement

## Comparative Strategy Table

| Aspect | CNIPA | USPTO | EPO |
|--------|-------|-------|-----|
| Test | Three-element test | Alice/Mayo two-step | Technical character |
| Key requirement | Technical three elements | Significantly more | Further technical effect |
| Hardware needed | Helpful but not mandatory | Strongly recommended | Helpful |
| Pure algorithm | Not patentable | Not patentable | Not patentable |
| Algorithm + application | Patentable | Usually patentable | Patentable |
| Business method + AI | Difficult | Very difficult | Not patentable |
| Training method | Patentable with specifics | Patentable with specifics | Case-by-case |
| Data processing | Patentable if technical | Depends on Step 2 | Needs technical effect |

## Practical Tips for Multi-Jurisdiction Filing

1. **Start with the most restrictive jurisdiction** in claim drafting
2. **Include hardware elements** in at least some claims (helps in all jurisdictions)
3. **Describe the technical problem explicitly** in the specification
4. **Quantify technical improvements** where possible
5. **Separate training and inference claims** (helps everywhere, critical for US)
6. **Include system, method, and medium claims** for maximum coverage
7. **Use specific architectural language** rather than general ML terms
