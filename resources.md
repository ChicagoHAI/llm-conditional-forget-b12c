# Research Resources: Conditional Forgetting in LLMs

## Phase 0: Initial Research Findings

### Research Question Assessment
The hypothesis states that humans can "conditionally forget" existing knowledge and apply new, hypothetical rules in analogical scenarios, whereas LLMs struggle with this, even with advanced reasoning models.

### Gaps Identified
- **No datasets specified**: Need to find or create evaluation scenarios
- **No baselines specified**: Need to define comparison methods
- **No metrics specified**: Need to establish success criteria

---

## Literature Review Summary

### Key Findings from Research (2024-2025)

#### 1. Counterfactual Reasoning in LLMs
**Main Insight**: LLMs struggle significantly with counterfactual reasoning - the ability to reason about hypothetical scenarios that deviate from their training knowledge.

**Key Papers**:
- **CounterBench (2025)**: New benchmark with 1K counterfactual questions showing most LLMs perform at random-guessing levels
- **"LLMs Struggle to Perform Counterfactual Reasoning with Parametric Knowledge"** (June 2025): Models can't selectively retain knowledge under counterfactual premises
- **"On the Eligibility of LLMs for Counterfactual Reasoning: A Decompositional Study"** (May 2025): Examines ability to adjust responses with modified premises

**Consistent Finding**: LLMs' reasoning abilities are often overestimated, with counterfactual reasoning remaining a widely documented weakness.

#### 2. Chess and Modified Rules Research
**Main Insight**: While LLMs can play chess reasonably, they fail dramatically when rules are modified.

**Key Papers**:
- **"Large Language Models on the Chessboard"** (2023): Used chess to probe complex reasoning
- **"Explore the Reasoning Capability of LLMs in the Chess Testbed"** (2024): Chess as ideal testbed for reasoning evaluation
- **ChessQA (2024)**: Evaluates chess understanding comprehensively

**Critical Finding**: "GPT-3 fails to solve simplest variations of the original tasks, whereas human performance remains consistently high across all modified versions."

#### 3. Analogical Reasoning Robustness
**Main Insight**: LLMs show strong pattern matching but brittle performance under modifications.

**Key Finding**:
- GPT-3 matches/surpasses humans on standard analogical tasks
- Performance deteriorates rapidly under even slight problem modifications
- Example: Letter-string tests fail when alphabet is shuffled, but humans remain robust

#### 4. Current Reasoning Models (2024-2025)
**State-of-the-art Models**:
- **GPT-4o and o1**: o1 uses chain-of-thought for deep reasoning (74% on AIME vs 12% for GPT-4o)
- **Claude Opus 4 & Sonnet 4**: Hybrid reasoning models with two modes
- **Gemini 2.0 Flash Thinking**: New reasoning capabilities
- **DeepSeek R1**: Specialized reasoning model

**Performance Note**: Reasoning models excel at complex math/logic but may still struggle with counterfactual scenarios.

---

## Available Datasets and Benchmarks

### Directly Relevant Benchmarks

#### 1. **CounterBench** (2025) ✓ HIGHLY RELEVANT
- **Content**: 1K counterfactual reasoning questions
- **Structure**: 5 difficulty levels (5-9 events), balanced Yes/No answers
- **Tested Models**: GPT-4o, Claude 3.5, Gemini-1.5-Flash, DeepSeek-V3
- **Key Metric**: Most models perform at random-guessing level (~50%)
- **Availability**: Recently published (Feb 2025), arXiv paper available
- **Decision**: Check if dataset is publicly available; if not, can create similar evaluation set

#### 2. **GameBench** (2024) ✓ RELEVANT
- **Content**: Cross-domain benchmark with modified "counterfactual" games
- **Purpose**: Evaluate strategic reasoning with rule modifications
- **Decision**: Check for availability; useful for game-based scenarios

#### 3. **CRASS Dataset** ✓ RELEVANT
- **Content**: "What if" scenarios for counterfactual reasoning
- **Decision**: Search for public availability

### Game-Based Benchmarks

#### 4. **GAMEBoT** (2024-2025)
- **Content**: Modified game environments with rule changes
- **Examples**: Negotiation games with modified values
- **Decision**: Could adapt methodology

#### 5. **ChessQA** (2024)
- **Content**: Chess understanding evaluation
- **Potential**: Could create modified chess rule scenarios
- **Decision**: Use as inspiration for chess-based tasks

---

## Proposed Research Approach

### What We'll Create/Use

Since no perfect pre-existing dataset matches our exact hypothesis, we will:

1. **Create Custom Evaluation Scenarios** focusing on:
   - Modified chess rules (e.g., "What if bishops could move like knights?")
   - Modified game rules (e.g., "In this version of tic-tac-toe, getting 3 in a row loses")
   - Hypothetical physics/world rules (e.g., "If gravity pulled sideways...")
   - Counterfactual knowledge (e.g., "In a world where France won WWII...")

2. **Draw Inspiration From**:
   - CounterBench methodology (difficulty levels, balanced answers)
   - Chess research (well-defined rules, objective ground truth)
   - GameBench (counterfactual game variations)

3. **Test Multiple Model Types**:
   - Standard models: GPT-4o, Claude Sonnet 4.5, Gemini 2.0
   - Reasoning models: GPT-o1 (to test if reasoning helps)
   - Compare performance: Do reasoning models handle this better?

### Evaluation Approach

**Baseline**: Standard prompting asking models to apply modified rules

**Metrics**:
1. **Accuracy**: Correct application of new rules
2. **Error Type Analysis**:
   - Original rule interference (using old rules instead of new)
   - Logical errors (misunderstanding new rules)
   - Format errors (not answering properly)
3. **Difficulty Scaling**: Performance across simple → complex scenarios

**Statistical Tests**:
- Compare model performance vs. random baseline
- Compare standard vs. reasoning models (paired t-test)
- Analyze error patterns (qualitative + quantitative)

---

## Resource Decisions

### Datasets
**Decision**: Create custom evaluation set (50-100 scenarios) inspired by CounterBench and chess research, as no perfect existing dataset covers our exact hypothesis.

**Justification**:
- CounterBench not yet publicly available (Feb 2025 paper)
- Our focus on "conditional forgetting" specifically requires custom scenarios
- Can create high-quality, focused dataset in available time
- Will document creation methodology

### Models to Test
**Decision**: Use real LLM APIs (NOT simulations)

**Selected Models**:
1. GPT-4o (standard model, widely used baseline)
2. GPT-o1 (reasoning model - test if deep thinking helps)
3. Claude Sonnet 4.5 (strong general performance)
4. Gemini 2.0 Flash (recent model with good performance)

**Justification**:
- Mix of standard and reasoning models
- All state-of-the-art (2024-2025)
- API keys available via environment variables
- Cost-effective for 50-100 test scenarios (~$10-30)

### Baselines
**Decision**:
1. Random baseline (50% for binary questions)
2. Compare standard vs. reasoning models
3. Analyze error patterns qualitatively

**Justification**: Simple but effective for demonstrating the phenomenon.

---

## Time and Cost Estimates

### Time Budget (1 hour total)
- Phase 0 (Research): 15 min ✓
- Phase 1 (Planning): 10 min
- Phase 2 (Setup): 10 min
- Phase 3 (Implementation): 15 min
- Phase 4 (Experiments): 20 min (API calls)
- Phase 5 (Analysis): 15 min
- Phase 6 (Documentation): 15 min

### Cost Budget ($100 total)
- Expected API costs: $15-30 for ~50-100 scenarios across 4 models
- Well within budget

### Compute Requirements
- CPU only: ✓ (using APIs, no GPU needed)
- RAM: 8GB sufficient for scripts and data
- Storage: <100MB for results

---

## Key Research Papers Referenced

1. "CounterBench: A Benchmark for Counterfactuals Reasoning in Large Language Models" (arXiv 2502.11008, Feb 2025)
2. "On the Eligibility of LLMs for Counterfactual Reasoning: A Decompositional Study" (arXiv 2505.11839, May 2025)
3. "Large Language Models on the Chessboard" (arXiv 2308.15118, 2023)
4. "Explore the Reasoning Capability of LLMs in the Chess Testbed" (arXiv 2411.06655, 2024)
5. "Evaluating the Robustness of Analogical Reasoning in Large Language Models" (2024)
6. "Executable Counterfactuals: Improving LLMs' causal reasoning through code" (arXiv 2510.01539, Oct 2025)

---

## Conclusion

**Research is Feasible**: Yes, we have clear direction from literature, can create appropriate evaluation data, and have access to necessary models.

**Main Innovation**: Testing "conditional forgetting" concept specifically, using custom scenarios inspired by validated methodologies.

**Expected Contribution**: Empirical evidence on whether LLMs can truly "forget" existing knowledge to apply new rules, and whether reasoning models improve this capability.

---

*Phase 0 Complete - Proceeding to Phase 1: Planning*
