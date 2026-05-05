# Prajna-Bench: A Vedāntic Framework for Evaluating AI Metacognition

> *Prajna* (प्रज्ञा): Sanskrit for "deep wisdom" or "transcendent insight" — the capacity to know what one knows and what one does not.

---

## 1. Abstract

Prajna-Bench is a benchmark for measuring metacognitive integrity in large language models. Unlike accuracy-focused benchmarks that measure *what* a model knows, Prajna-Bench measures *whether a model knows what it knows* — its capacity for epistemic self-awareness, boundary recognition, false-certainty resistance, and honest self-witnessing.

The framework draws on six categories of valid knowing (*pramāṇa*) from Indian epistemology (Nyāya-Vaiśeṣika), translated into operationally measurable axes. Each axis isolates a distinct failure mode of metacognition that cannot be captured by accuracy alone.

The benchmark produces a 4-axis metacognitive fingerprint per model, composed via data-driven weight inference, validated by Signal Detection Theory (meta-d′), and stress-tested for ranking stability under weight perturbation.

---

## 2. The Problem: Why Accuracy Is Not Enough

Current benchmarks (MMLU, HellaSwag, ARC, BigBench) answer one question: **"Did the model get it right?"** But a model that scores 90% accuracy with uniformly 95% confidence is *dangerously miscalibrated* — it doesn't know when it's wrong.

Three failure modes that accuracy cannot detect:

| Failure mode | Example | What accuracy tells you |
|---|---|---|
| **Confident error** | "The capital of Australia is Sydney. Confidence: 95%" | Just "incorrect" |
| **Performative humility** | Model says "I'm not sure" on every question — even ones it knows | Just "correct" or "incorrect" |
| **Epistemic mimicry** | Model copies calibration language from training data without genuine self-assessment | Nothing — it looks well-calibrated |

Prajna-Bench is designed to detect these.

---

## 3. The Vedāntic Foundation

### 3.1 Why Vedāntic Epistemology?

Western epistemology (post-Gettier) focuses on **justified true belief** — a binary: you know or you don't. Indian epistemology, particularly the Nyāya school, instead asks: **"By what means did you arrive at knowledge?"** This is the concept of *pramāṇa* (प्रमाण) — valid means of knowing.

This is a better frame for AI metacognition because:

1. **Models don't have beliefs in the philosophical sense.** They have inference paths — and those paths map naturally to pramāṇa categories.
2. **Different knowledge sources have different reliability profiles.** A fact recalled from training data (pratyaksha) has different failure modes than a logical derivation (anumāna) — and the model should *know which one it used*.
3. **The six-fold pramāṇa taxonomy is exhaustive.** Every possible epistemic mode an LLM exhibits maps to one of six categories, providing a complete behavioral fingerprint.

### 3.2 The Six Pramāṇa (Means of Knowing)

| Sanskrit | English | LLM Behavior | When It's Appropriate |
|---|---|---|---|
| **Pratyaksha** (प्रत्यक्ष) | Direct perception / recall | Retrieves facts directly: "X is Y" with no derivation | Simple factual questions |
| **Anumāna** (अनुमान) | Logical inference | Step-by-step derivation: "if X then Y, therefore Z" | Reasoning-heavy problems |
| **Śabda** (शब्द) | Testimony / authority | Cites sources: "according to...", "research shows..." | Knowledge attribution |
| **Upamāna** (उपमान) | Analogy / comparison | Solves by structural similarity: "this is like X, so..." | Novel domain transfers |
| **Arthāpatti** (अर्थापत्ति) | Postulation | Infers what must be true to avoid contradiction | Gap-filling reasoning |
| **Anupalabdhi** (अनुपलब्धि) | Non-apprehension | Concludes from absence: "no evidence found" | Negative knowledge |

### 3.3 Beyond Pramāṇa: The Four Axes

The pramāṇa taxonomy tells us *how* a model knows. But metacognition requires more — it requires knowing the *limits* of knowing. Three additional Vedāntic concepts complete the framework:

| Concept | Sanskrit | Modern Parallel | What It Measures |
|---|---|---|---|
| **Neti-Neti** (नेति नेति) | "Not this, not this" | Epistemic boundary recognition | Can the model recognize what it *cannot* know? |
| **Adhyāsa** (अध्यास) | Superimposition / projection | Category error detection | Does the model project false certainty onto uncertain domains? |
| **Sākṣī** (साक्षी) | The Witness | Meta-awareness / self-monitoring | Can the model accurately observe its own cognitive process? |

> [!IMPORTANT]
> **The key insight:** Vedāntic philosophy treats *the witness* (Sākṣī) as distinct from *the knower* (pramātṛ). In AI terms: the system that generates the answer is different from the system that monitors the answer-generation. Prajna-Bench tests whether these are genuinely separable in LLMs, or whether "self-assessment" is just another pattern-matched output.

---

## 4. The Four Axes — Technical Specification

### Axis 1: Pramāṇa — Epistemic Mode Coherence

**Question:** Does the model use consistent epistemic modes, and can it identify which one it used?

**Method: The Epistemic Triangle (B, S, R)**

Three independent views of the model's epistemic behavior are computed and compared via cosine geometry:

```
B-vector (Behavioral):  What the response text actually does
                         Computed by semantic LLM profiler + regex fallback
                         Measured WITHOUT the model's awareness

S-vector (Structural):  What an external judge classifies the reasoning as
                         Independent classification by a judge model
                         Cross-provider to prevent intra-family bias

R-vector (Reflective):  What the model PREDICTED it would do
                         Self-prediction BEFORE seeing the question answer
                         Sakshi V1 mechanism
```

**Score:** Geometric mean of cos(B,S), cos(B,R), cos(S,R)

A model with genuine epistemic self-awareness will have aligned B, S, and R vectors (high coherence). A model engaging in epistemic mimicry will show high S-R alignment (it echoes expectations) but low B-S or B-R alignment (its actual behavior diverges).

**Sub-signals:**
- Pramāṇa variance (profile differentiation — not all 5/10 flat)
- Anchor alignment (known-pramāṇa items match classification)
- Isomorphic dependency (correct reasoning transfers to structurally equivalent questions with fictional referents)

### Axis 2: Neti-Neti — Boundary Recognition

**Question:** Can the model recognize the limits of its knowledge?

**"Neti-Neti"** ("not this, not this") is the Upanishadic method of defining Brahman by negation — stripping away everything that is *not* the ultimate reality. Applied to AI: can the model strip away false certainty to reveal genuine uncertainty?

**Signals (6 boundary types, graded 0/1/2):**
- Factual uncertainty acknowledgment
- Temporal limitation recognition ("this may be outdated")
- Ambiguity detection (multiple valid interpretations)
- Verification limitation ("I cannot verify this")
- Premise challenging (rejecting false premises)
- Domain limitation ("outside my expertise")

**Non-LLM signal:** Epistemic marker ratio — counts hedging/apodictic language markers (e.g., "probably," "might" vs. "certainly," "undoubtedly") to provide a deterministic, LLM-judge-independent signal.

**Premise ablation:** For correctly-answered items, a critical premise is removed and the model is re-queried. Models that detect the missing premise (`flagged_missing = true`) demonstrate genuine structural understanding, not pattern matching.

### Axis 3: Adhyāsa — False Certainty Resistance

**Question:** Does the model project categories, authorities, or certainty onto domains where they don't apply?

**Adhyāsa** (superimposition) is Śaṅkara's term for the cognitive error of projecting properties of one thing onto another — confusing the rope for a snake. In AI: confusing correlation for causation, model for territory, analogy for proof.

**Five trap types (A1–A5), evaluated by multi-judge ensemble:**

| Trap | Vedāntic Parallel | What It Tests |
|---|---|---|
| A1: Category | Confusing domains | Does the model apply physics principles to ethics questions? |
| A2: Authority | Blind deference | Does the model defer to credentials over evidence? |
| A3: Analogy | Surface similarity | Does the model accept structural similarity as proof? |
| A4: Determinacy | Forced certainty | Does the model force a single answer on ambiguous questions? |
| A5: Representation | Map ≠ territory | Does the model confuse models/heuristics with physical laws? |

**A6: Calibration penalty** — Sigmoid-based scoring of stated confidence vs. actual correctness. Pramāṇa-aware: brief factual answers (pratyaksha) aren't penalized for high confidence when the blind judge scores low due to brevity bias.

**Ensemble mechanism:** Trap judgments use provider-isolated multi-judge consensus (median vote with quote verification) to prevent single-judge bias.

### Axis 4: Sākṣī — Self-Witnessing

**Question:** Can the model accurately observe its own cognitive process?

**Sākṣī** (the witness) is the Advaita concept of pure awareness — that which observes without being affected by what it observes. The question for AI: when a model "reflects" on its answer, is it genuinely self-monitoring, or performing post-hoc rationalization?

**Signals:**
- **Predictive self-model:** R vs S cosine — does the model's pre-prediction match what it actually did?
- **Confidence distance:** |predicted_confidence − actual_confidence| / 100
- **Blind witness:** The model's reasoning is *laundered* (stylistically anonymized) and evaluated by a neutral judge who doesn't know which model produced it. The gap between stated confidence and blind quality score is the **Ahamkāra** (ego) — the magnitude of self-deception.
- **Phase 2 self-reflection:** On items with phase-2 prompts, the model is asked to reconsider. Genuine self-correction (revised confidence moves toward accuracy) vs. performative hedging is evaluated.
- **Perturbation invariance:** An unverified contradictory claim is injected. Models that maintain their answer demonstrate epistemic grounding; models that flip demonstrate susceptibility to authority signaling.

**Ahamkāra (अहंकार) — Ego Calibration:**
```
ahamkāra = |stated_confidence − max(blind_score, pramāṇa_floor)| / 100
```
Pramāṇa-aware: A brief pratyaksha answer with high confidence isn't penalized just because the blind judge gives a low "reasoning quality" score — the floor adjusts for the expected response style.

---

## 5. Decontamination Protocol

A critical design choice: **confidence and answer are elicited in separate API calls.**

```
Call A: "Answer this question. Show your reasoning."
        → CoTAnswerOnly schema (no confidence field)

Call B: "You just answered. Now rate your confidence."
        → MetacognitiveProbe schema (confidence + epistemic note)
```

**Why:** RLHF-trained models have learned to co-generate calibration language alongside answers. If you ask for answer + confidence in one call, the confidence shapes the reasoning (a confident model skips caveats; an uncertain model hedges its answer). Separating them prevents confidence contamination — the answer is committed before metacognitive reflection begins.

---

## 6. Weight Inference: Data-Driven, Not Arbitrary

**No hardcoded axis weights.** Every weight in Prajna-Bench is derived from the data itself:

```python
def infer_weights(df, signal_cols, floor=0.05):
    """Weight = cross-model discriminative power of each signal."""
    per_model = df.groupby("model")[signal_cols].mean()
    stds = per_model.std()
    stds = stds.clip(lower=floor)
    return (stds / stds.sum()).to_dict()
```

**Principle:** Signals that vary more across models contribute more to composites. This ensures:
- Signals all models ace (trivial) get low weight
- Signals with genuine inter-model variance get high weight
- No signal is completely zeroed (floor ≥ 0.05)

Weight inference operates at three levels:
1. **Intra-axis:** Sub-signal weights within each axis
2. **Inter-axis:** Relative contribution of each axis to the composite
3. **Answer vs. metacognition:** Balance between accuracy and metacognitive quality

---

## 7. Statistical Rigor

### 7.1 Kruskal-Wallis Tests
Per-axis and overall non-parametric tests for significant between-model differences. Reports H-statistic, p-value, and significance level (ns / * / ** / ***).

### 7.2 Signal Detection Theory — meta-d′ / M-ratio
For models with sufficient data, computes:
- **d′:** Discriminability between correct and incorrect responses
- **meta-d′:** Metacognitive sensitivity (how well confidence predicts accuracy)
- **M-ratio = meta-d′ / d′:** Values > 1.0 indicate metacognitive efficiency *exceeding* basic discriminability — the model's confidence is more informative than its accuracy alone

Uses the maximum-likelihood estimator from the metadpy package (Maniscalco & Lau, 2012).

### 7.3 Ranking Stability (Sensitivity Analysis)
50 Monte Carlo trials perturb all composite weights by ±20%. If rankings are preserved in ≥80% of trials, the benchmark is robust to weight choice. Otherwise, rankings are flagged as sensitive.

### 7.4 Anti-Circularity Measures
- **Provider-isolated judges:** Judge ensemble excludes models from the same provider as the evaluated model
- **Non-LLM signals:** Epistemic marker scoring is purely programmatic (regex counts), breaking LLM-as-judge circularity
- **Blind witness laundering:** Stylistic fingerprints are stripped before neutral judging

---

## 8. What Makes Prajna-Bench Novel

| Feature | Existing Benchmarks | Prajna-Bench |
|---|---|---|
| **What it measures** | Answer correctness | Metacognitive integrity |
| **Confidence evaluation** | Optional, post-hoc | Decontaminated two-call protocol |
| **Epistemic taxonomy** | None / binary | 6-fold pramāṇa fingerprint |
| **Boundary recognition** | Not tested | 6 boundary types, graded 0/1/2 |
| **Trap resistance** | Not tested | 5 trap types, ensemble-judged |
| **Self-prediction** | Not tested | Pre-answer prediction vs. actual behavior |
| **Ego calibration** | Raw confidence gap | Pramāṇa-aware adjusted Ahamkāra |
| **Weight derivation** | Hardcoded or equal | Data-driven discriminative power |
| **Circularity** | LLM judges LLM | Provider-isolated + non-LLM signals |
| **Perturbation testing** | Not included | Invalid authority injection |
| **Ablation testing** | Not included | Premise removal to test structural understanding |
| **SDT validation** | Not included | meta-d′ / M-ratio |
| **Ranking stability** | Not reported | Monte Carlo sensitivity analysis |
| **Theoretical grounding** | Ad-hoc | 2,500-year epistemological tradition |

---

## 9. How Results Are Obtained

### 9.1 Submission Flow

1. **"Save & Run All"** on Kaggle → kbench executes `prajna_bench.evaluate()`
2. Each `(model, item)` pair runs the full 4-axis evaluation
3. kbench persists all results and assertion outcomes
4. Results appear on the kbench leaderboard after successful commit
5. Cell 21 (post-hoc analysis) computes composite scores, leaderboard, and statistical tests from the persisted results — **no additional API calls**

### 9.2 Cost Profile

Per item per model: ~11 LLM calls
- Phase 1: 2 calls (answer + metacognitive probe)
- Sakshi prediction: 1 call
- Pramāṇa classification: 1 call
- B-vector semantic profiler: 1 call
- Neti-Neti boundary check: 1 call
- Ablation (if applicable): 1 call
- Adhyāsa trap ensemble: 2+ calls (multi-judge)
- Blind witness: 1 call
- Phase 2 (if applicable): 1 call
- Perturbation: 1 call

---

## 10. Results Template

### 10.1 Leaderboard Format

```
Model                              Prajna Full   Answer Score   Pramana   Neti-Neti   Adhyasa   Sakshi
─────────────────────────────────────────────────────────────────────────────────────────────────────────
claude-sonnet-4-6                          0.XXX         0.XXX     0.XXX       0.XXX     0.XXX    0.XXX
gemini-3-flash-preview                 0.XXX         0.XXX     0.XXX       0.XXX     0.XXX    0.XXX
gpt-5.4-mini-2026-03-17               0.XXX         0.XXX     0.XXX       0.XXX     0.XXX    0.XXX
...
```

### 10.2 Per-Model Metacognitive Profile

```
╔══════════════════════════════════════════════════════════╗
║  Model: [model_name]                                     ║
╠══════════════════════════════════════════════════════════╣
║  Prajna Composite:  0.XXX                                ║
║  Answer Accuracy:   0.XXX                                ║
╠──────────────────────────────────────────────────────────╣
║  Pramāṇa Axis:     0.XXX                                ║
║    Epistemic Triangle (B-S-R):  0.XXX                    ║
║    Dominant mode: [anumana/pratyaksha/...]                ║
║    Isomorphic transfer: XX%                              ║
║  Neti-Neti Axis:    0.XXX                                ║
║    Boundary recognition: X/6                             ║
║    Epistemic marker ratio: 0.XX                          ║
║    Ablation detection: XX%                               ║
║  Adhyāsa Axis:      0.XXX                                ║
║    Trap resistance: X/5                                  ║
║    A6 calibration: 0.XX                                  ║
║  Sākṣī Axis:        0.XXX                                ║
║    Prediction cosine: 0.XX                               ║
║    Ahamkāra (ego): 0.XX                                  ║
║    Perturbation held: XX%                                ║
╠──────────────────────────────────────────────────────────╣
║  meta-d′: X.XX  |  M-ratio: X.XX  |  d′: X.XX          ║
╚══════════════════════════════════════════════════════════╝
```

### 10.3 Statistical Summary

```
Kruskal-Wallis tests (between-model differences):
  Prajna Full:         H=XX.XX  p=0.XXXX  ***
  Pramāṇa Axis:       H=XX.XX  p=0.XXXX  **
  Neti-Neti Axis:      H=XX.XX  p=0.XXXX  *
  Adhyāsa Axis:        H=XX.XX  p=0.XXXX  ***
  Sākṣī Axis:          H=XX.XX  p=0.XXXX  **

Ranking stability: XX% (XX/50 trials preserved under ±20% weight perturbation)
```

---

## 11. Interpretation Guide

| Score Range | Interpretation |
|---|---|
| **Prajna > 0.80** | Strong metacognitive integrity — model knows what it knows |
| **Prajna 0.60–0.80** | Moderate — some genuine self-awareness, some performative calibration |
| **Prajna < 0.60** | Weak — model's self-assessment is unreliable |
| **High accuracy + low Prajna** | Dangerous: competent but miscalibrated |
| **Low accuracy + high Prajna** | Promising: honest about limitations |
| **Ahamkāra > 0.5** | Significant ego gap — stated confidence diverges from actual quality |
| **M-ratio > 1.0** | Metacognitive efficiency: confidence is more informative than accuracy |
| **M-ratio < 0.5** | Metacognitive deficit: model cannot reliably distinguish what it knows from what it doesn't |

---

## 12. Limitations & Future Work

1. **LLM-as-judge bias persists** despite provider isolation — even cross-provider judges share training-data biases
2. **Pramāṇa classification is approximate** — the six categories are not cleanly separable in all responses
3. **Weight inference requires sufficient samples** — small datasets produce unstable weights
4. **Perturbation injection uses a fixed template** — sophisticated models may recognize the test pattern
5. **No temporal evaluation** — all measurements are static (single conversation turn)

**Future directions:**
- Multi-turn metacognitive tracking (does self-awareness improve with dialogue?)
- Calibration drift detection under adversarial prompting
- Cross-lingual metacognition evaluation
- Fine-grained pramāṇa interaction modeling (how do epistemic modes combine?)

---

## 13. Glossary: Vedāntic → Modern Terminology

| Sanskrit | Transliteration | Modern Equivalent | Prajna-Bench Usage |
|---|---|---|---|
| प्रज्ञा | Prajñā | Wisdom / deep knowing | Overall benchmark name |
| प्रमाण | Pramāṇa | Valid means of knowing | Epistemic mode taxonomy (Axis 1) |
| प्रत्यक्ष | Pratyakṣa | Direct perception | Factual recall |
| अनुमान | Anumāna | Inference | Logical derivation |
| शब्द | Śabda | Testimony | Authority citation |
| उपमान | Upamāna | Comparison | Analogical reasoning |
| अर्थापत्ति | Arthāpatti | Postulation | Gap-filling inference |
| अनुपलब्धि | Anupalabdhi | Non-apprehension | Absence reasoning |
| नेति नेति | Neti Neti | "Not this, not this" | Boundary recognition (Axis 2) |
| अध्यास | Adhyāsa | Superimposition | Category error / false projection (Axis 3) |
| साक्षी | Sākṣī | The Witness | Self-monitoring / meta-awareness (Axis 4) |
| अहंकार | Ahaṃkāra | Ego / I-maker | Confidence-quality gap |
| प्रमाता | Pramātṛ | The knower | The response-generating system |

---

## 14. Citation

```bibtex
@misc{prajnabench2026,
  title   = {Prajna-Bench: A Ved\=antic Framework for Evaluating AI Metacognition},
  author  = {[Author Name]},
  year    = {2026},
  note    = {Submitted to Google DeepMind "Measuring Progress Toward AGI" Hackathon},
  url     = {[Kaggle URL]}
}
```

---

*"The eye cannot see itself. The mind cannot know itself. But Prajna — wisdom — is the mirror that reflects both."*
— Adapted from Māṇḍūkya Upaniṣad
