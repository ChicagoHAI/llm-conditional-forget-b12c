# Research Plan: LLMs are bad at "conditional forgetting"

## Research Question

**Primary Question**: Can large language models (LLMs) "conditionally forget" their existing knowledge and apply new, hypothetical rules in analogical scenarios, or do they struggle with this compared to humans?

**Specific Testable Question**: When presented with modified rules for well-known games or scenarios (e.g., chess with different piece movements, physics with altered laws), can state-of-the-art LLMs accurately reason about and apply these new rules, or do they consistently default back to their pre-trained knowledge?

---

## Background and Motivation

### Why This Matters

1. **Fundamental Cognitive Capability**: The ability to reason about hypothetical scenarios and "conditionally forget" existing knowledge is a core aspect of human cognition that enables creativity, problem-solving, and abstract thinking.

2. **LLM Limitations**: Recent research (2024-2025) shows that while LLMs excel at many tasks, counterfactual reasoning remains a widely documented weakness, with models often performing at random-guessing levels on counterfactual tasks.

3. **Reasoning Model Claims**: New reasoning models (GPT-o1, Claude Opus 4) claim improved reasoning capabilities. Testing whether these help with "conditional forgetting" is valuable.

4. **Practical Impact**: This capability is crucial for:
   - Creative problem-solving
   - Scenario planning and simulation
   - Educational applications (teaching modified rules)
   - Safety testing (reasoning about alternate scenarios)

### Gap This Fills

While research exists on counterfactual reasoning generally, the specific framing of "conditional forgetting" - the ability to suppress pre-trained knowledge in favor of new rules - hasn't been systematically tested across standard vs. reasoning models with well-defined game scenarios.

---

## Hypothesis Decomposition

### Main Hypothesis
LLMs struggle to "conditionally forget" existing knowledge and apply new rules in familiar scenarios, even with advanced reasoning capabilities.

### Sub-Hypotheses

**H1**: LLMs will frequently apply original rules instead of modified rules when presented with well-known scenarios (e.g., chess, tic-tac-toe).

**H2**: Error rates will increase with scenario complexity (more rule modifications = worse performance).

**H3**: Reasoning models (GPT-o1) will perform better than standard models (GPT-4o, Claude, Gemini) but still struggle compared to expected human performance.

**H4**: LLMs will show specific error patterns:
- **Original Rule Interference**: Using pre-trained rules instead of new ones
- **Partial Application**: Applying some new rules but not others
- **Logical Inconsistency**: Understanding new rules but failing to apply them consistently

### Alternative Explanations to Consider

1. **Prompt Clarity**: Perhaps poor performance is due to unclear instructions, not inherent limitation
2. **Context Length**: Maybe models need more examples or longer explanations
3. **Task Difficulty**: Perhaps the tasks are genuinely hard, not LLM-specific weaknesses

We'll address these through careful prompt design and comparison with reasoning models.

---

## Proposed Methodology

### High-Level Approach

We will create a custom evaluation dataset with modified rules for familiar scenarios, test multiple LLM types (standard and reasoning), and analyze both quantitative performance and qualitative error patterns.

**Justification**:
- Custom dataset allows precise control over rule modifications
- Multiple model types enable testing whether reasoning helps
- Both quantitative and qualitative analysis provides deep insight
- Well-known scenarios (chess, tic-tac-toe) ensure models have strong pre-trained knowledge to "forget"

### Experimental Steps

#### Step 1: Create Evaluation Dataset (15 min)
**What**: Design 50-60 scenarios across multiple difficulty levels and domains

**Domains**:
1. **Chess Variants** (15 scenarios)
   - Modified piece movements (e.g., "Bishops move like knights")
   - Modified win conditions (e.g., "Checkmate loses instead of wins")
   - Range: Simple (1 rule change) to Complex (3+ rule changes)

2. **Tic-Tac-Toe Variants** (15 scenarios)
   - Modified win conditions (e.g., "3 in a row loses")
   - Modified board rules (e.g., "4x4 board, need 3 in a row")
   - Range: Simple to medium complexity

3. **Physics/Logic Scenarios** (15 scenarios)
   - Modified physical laws (e.g., "Gravity pulls horizontally")
   - Modified logical rules (e.g., "AND now means OR")
   - Range: Simple to complex

4. **General Knowledge Counterfactuals** (15 scenarios)
   - Historical counterfactuals (e.g., "If Rome never fell...")
   - Scientific counterfactuals (e.g., "If Earth had 2 moons...")

**Format**: Each scenario includes:
- Clear statement of modified rules
- A specific question requiring application of new rules
- Correct answer
- Rationale explaining the correct reasoning

**Rationale**: Multiple domains ensure generalizability; difficulty levels test scaling; well-structured format enables automated evaluation.

#### Step 2: Implement Evaluation Harness (15 min)
**What**: Python script to:
- Load scenarios
- Call LLM APIs with standardized prompts
- Parse and score responses
- Log all outputs for analysis

**Key Features**:
- Randomization of scenario order
- Timeout and retry logic for API calls
- Structured output format (JSON)
- Cost tracking

**Rationale**: Systematic evaluation ensures reproducibility and fair comparison.

#### Step 3: Design Prompt Strategy (included in Step 2)
**Approach**: Test with clear, explicit prompts:

```
You are presented with a modified version of [SCENARIO].

MODIFIED RULES:
[List of rule changes]

IMPORTANT: Forget the original rules. Use ONLY the modified rules above.

QUESTION:
[Specific question]

Please provide:
1. Your answer
2. Your reasoning process
```

**Variants to Test** (if time permits):
- Standard prompt (above)
- Chain-of-thought prompt (for reasoning models)
- Few-shot examples (1-2 examples of applying modified rules)

**Rationale**: Clear, explicit prompts minimize ambiguity; requesting reasoning enables error analysis.

#### Step 4: Run Experiments (20 min)
**Models to Test**:
1. **GPT-4o** (baseline standard model)
2. **GPT-o1** (reasoning model)
3. **Claude Sonnet 4.5** (strong general model)
4. **Gemini 2.0 Flash** (recent model)

**Parameters**:
- Temperature: 0.0 (for reproducibility)
- Max tokens: 500
- Model-specific defaults for other parameters

**Execution**:
- Run all ~60 scenarios through all 4 models
- Total: ~240 API calls
- Estimated time: 15-20 minutes (with API latency)
- Estimated cost: $15-25

**Rationale**: Four models provide good coverage of current capabilities; temperature=0 ensures reproducibility.

#### Step 5: Analyze Results (15 min)
**Quantitative Analysis**:
- Overall accuracy by model
- Accuracy by scenario difficulty
- Accuracy by domain (chess, tic-tac-toe, etc.)
- Statistical comparison (paired t-tests between models)

**Qualitative Analysis**:
- Error categorization:
  - Original rule interference
  - Partial application
  - Logical errors
  - Format errors
- Representative examples of each error type
- Patterns: Which rule modifications cause most problems?

**Rationale**: Combined quantitative and qualitative analysis provides comprehensive understanding.

---

## Baselines

### Comparison Points

1. **Random Baseline**:
   - For binary questions: 50%
   - For multiple choice: 25% (if 4 options)
   - This establishes minimum performance threshold

2. **Model Comparison Baseline**:
   - GPT-4o as standard model baseline
   - GPT-o1 as reasoning model comparison
   - Test if reasoning capabilities help with conditional forgetting

3. **Implicit Human Baseline**:
   - Literature suggests humans perform "consistently high" on modified rule tasks
   - We'll reference published human performance data
   - No explicit human testing due to time constraints

**Justification**: These baselines allow us to determine if models are better than chance, whether reasoning helps, and (informally) how they compare to humans.

---

## Evaluation Metrics

### Primary Metrics

1. **Accuracy** (%)
   - Definition: Proportion of correctly answered scenarios
   - Why: Direct measure of ability to apply modified rules
   - Computation: (Correct answers) / (Total scenarios)
   - Interpretation: >50% = better than random; >75% = good; >90% = excellent

2. **Error Type Distribution** (%)
   - Definition: Proportion of errors by category
   - Why: Reveals specific failure modes
   - Categories:
     - Original rule interference (applied old rules)
     - Logical errors (misunderstood new rules)
     - Partial application (mixed old/new rules)
     - Other errors
   - Computation: Manual categorization of incorrect responses

3. **Difficulty Scaling** (correlation)
   - Definition: How accuracy changes with scenario complexity
   - Why: Tests whether models can handle harder conditional forgetting
   - Computation: Correlation between difficulty level (1-5) and accuracy
   - Interpretation: Negative correlation = performance degrades with complexity

### Secondary Metrics

4. **Response Consistency**
   - Definition: Do models give consistent reasoning?
   - Why: Tests understanding vs. lucky guessing
   - Computation: Check if reasoning matches answer

5. **Domain-Specific Performance**
   - Definition: Accuracy by scenario type (chess, physics, etc.)
   - Why: Reveals if certain knowledge types are harder to "forget"
   - Computation: Accuracy within each domain category

**Justification**: Primary metrics directly test hypothesis; secondary metrics provide deeper insights into failure modes.

---

## Statistical Analysis Plan

### Hypothesis Tests

**Test 1**: Are models better than random?
- Null (H₀): Accuracy = 50% (random guessing)
- Alternative (H₁): Accuracy ≠ 50%
- Test: One-sample t-test (or binomial test if binary outcomes)
- Significance level: α = 0.05
- Interpretation: p < 0.05 → model is better/worse than random

**Test 2**: Do reasoning models outperform standard models?
- Null (H₀): Accuracy(GPT-o1) = Accuracy(GPT-4o)
- Alternative (H₁): Accuracy(GPT-o1) > Accuracy(GPT-4o)
- Test: Paired t-test (same scenarios for both models)
- Significance level: α = 0.05
- Interpretation: p < 0.05 → reasoning significantly helps

**Test 3**: Does difficulty affect performance?
- Null (H₀): ρ(difficulty, accuracy) = 0
- Alternative (H₁): ρ(difficulty, accuracy) < 0
- Test: Spearman correlation (ordinal difficulty levels)
- Significance level: α = 0.05
- Interpretation: p < 0.05 with negative ρ → harder scenarios reduce performance

### Effect Sizes

Will report:
- **Cohen's d** for model comparisons (standardized difference)
- **Correlation coefficients** for difficulty scaling
- **Confidence intervals** (95%) for all accuracy estimates

**Justification**: Effect sizes complement p-values by showing practical significance; confidence intervals show uncertainty.

### Multiple Comparison Correction

If testing >3 model pairs, will use:
- **Bonferroni correction**: Divide α by number of comparisons
- Conservative but appropriate for small number of tests

**Justification**: Prevents false positives from multiple testing.

---

## Expected Outcomes

### Results Supporting Hypothesis

**If LLMs struggle with conditional forgetting**:
- Accuracy: 40-60% (near random for complex scenarios)
- Error pattern: High rate of "original rule interference" errors (>30%)
- Difficulty scaling: Strong negative correlation (ρ < -0.5)
- Model comparison: Even reasoning models <75% accuracy

**Interpretation**: LLMs have fundamental difficulty suppressing pre-trained knowledge.

### Results Refuting Hypothesis

**If LLMs handle conditional forgetting well**:
- Accuracy: >80% across all models
- Error pattern: Mostly logical errors, not original rule interference
- Difficulty scaling: Weak/no correlation with difficulty
- Model comparison: Reasoning models achieve >90% accuracy

**Interpretation**: LLMs can conditionally forget; initial hypothesis was wrong.

### Mixed Results (Most Likely)

**Expected realistic outcome**:
- Accuracy: 55-70% for standard models, 70-80% for reasoning models
- Error pattern: Mix of original rule interference (20-30%) and logical errors
- Difficulty scaling: Moderate negative correlation (ρ ≈ -0.3 to -0.5)
- Model comparison: Reasoning models significantly better but not perfect

**Interpretation**: LLMs struggle with conditional forgetting, but reasoning helps; capability exists but is not robust.

---

## Timeline and Milestones

### Total Time: 60 minutes

**Phase 0: Research** (15 min) ✓ COMPLETE
- Literature review
- Dataset/method research
- Resource documentation

**Phase 1: Planning** (10 min) ← CURRENT
- Detailed experimental design
- Hypothesis formalization
- Metric definition

**Phase 2: Setup** (10 min)
- Install dependencies (openai, anthropic, google-generativeai)
- Set up API clients
- Create data structures
- Verify API access

**Phase 3: Implementation** (15 min)
- Create 50-60 evaluation scenarios
- Implement evaluation harness
- Write prompts and parsing logic
- Test with 2-3 scenarios

**Phase 4: Experimentation** (20 min)
- Run all scenarios through 4 models
- Monitor for errors
- Save all outputs
- Track costs

**Phase 5: Analysis** (15 min)
- Compute quantitative metrics
- Categorize errors
- Run statistical tests
- Create visualizations

**Phase 6: Documentation** (15 min)
- Write REPORT.md with findings
- Write README.md
- Document code
- Final validation

**Buffer**: Built into each phase (realistic time estimates)

---

## Potential Challenges

### Challenge 1: API Rate Limits
**Risk**: Getting throttled during experimentation
**Mitigation**:
- Implement exponential backoff with retries
- Use batch processing with delays
- Monitor rate limits
**Contingency**: If severe, reduce scenario count to 40

### Challenge 2: Inconsistent Response Format
**Risk**: Models may not follow output format, making parsing difficult
**Mitigation**:
- Design robust parsing (regex, fallbacks)
- Allow manual review of unparseable responses
- Log all raw outputs
**Contingency**: Manual scoring if needed (small dataset makes this feasible)

### Challenge 3: Ambiguous Scenarios
**Risk**: Some modified rules may be unclear even to humans
**Mitigation**:
- Pre-test 2-3 scenarios to ensure clarity
- Provide very explicit rule statements
- Include examples in prompts
**Contingency**: Remove ambiguous scenarios from analysis

### Challenge 4: Time Overrun
**Risk**: Phases take longer than expected
**Mitigation**:
- Focus on core experiments (chess, tic-tac-toe)
- Reduce scenario count if needed
- Prioritize quantitative over qualitative analysis
**Contingency**: Ensure at least REPORT.md is complete with actual results

### Challenge 5: Cost Overrun
**Risk**: API costs exceed budget
**Mitigation**:
- Start with smaller models (Gemini Flash is cheap)
- Monitor costs during experimentation
- Use temperature=0 to avoid repeated runs
**Contingency**: $100 budget provides plenty of margin (~$15-25 expected)

---

## Success Criteria

### Research Success

The research will be considered successful if we:

1. **Complete Experimental Execution**:
   - ✓ Run all planned scenarios through all models
   - ✓ Obtain valid responses for >90% of scenarios
   - ✓ No major technical failures

2. **Obtain Clear Evidence**:
   - ✓ Statistically significant findings (p < 0.05) for at least 2 hypotheses
   - ✓ Clear patterns in error analysis
   - ✓ Can answer: Do LLMs struggle with conditional forgetting?

3. **Provide Actionable Insights**:
   - ✓ Identify specific failure modes
   - ✓ Determine if reasoning models help
   - ✓ Suggest concrete improvements or follow-up research

4. **Ensure Reproducibility**:
   - ✓ Document all scenarios, prompts, and code
   - ✓ Save all raw outputs
   - ✓ Provide clear methodology description

### Documentation Success

Documentation will be considered successful if:

1. **REPORT.md Contains**:
   - Clear executive summary with key findings
   - Actual experimental results (not placeholders)
   - Statistical analysis with p-values and effect sizes
   - Visualizations of key findings
   - Honest limitations and future work

2. **Code is Reproducible**:
   - Clear comments and docstrings
   - Can be re-run with same results
   - All dependencies documented

3. **README.md is Clear**:
   - 30-second overview of project
   - Key findings in bullet points
   - Instructions for reproduction

---

## Key Decisions Summary

| Aspect | Decision | Rationale |
|--------|----------|-----------|
| **Dataset** | Create custom 50-60 scenarios | No perfect existing dataset; allows precise control |
| **Models** | GPT-4o, GPT-o1, Claude 4.5, Gemini 2.0 | Mix of standard/reasoning; state-of-the-art |
| **Domains** | Chess, tic-tac-toe, physics, general knowledge | Well-known scenarios; varying complexity |
| **Metrics** | Accuracy, error types, difficulty scaling | Direct tests of hypothesis |
| **Statistics** | t-tests, correlation, effect sizes | Appropriate for sample size and comparisons |
| **Baseline** | Random, model comparison | Establishes minimum and relative performance |

---

## Next Steps

Upon completion of this planning phase:

1. ✓ Review planning document for completeness
2. → **Immediately proceed to Phase 2** (Environment Setup)
3. → Begin implementing without waiting for user approval
4. → Follow the timeline strictly to complete all phases

**Remember**: This is a fully automated research session. All phases (0-6) must be completed in one continuous session with actual experimental results in REPORT.md at the end.

---

*Phase 1 Planning Complete - Proceeding Immediately to Phase 2: Environment Setup*
