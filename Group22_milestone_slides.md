# Milestone Presentation — Slide Content Draft
## Group 22 — SemEval-2026 Task 5
### Convert to .pptx before submission!

---

## Slide 1: Title

**SemEval-2026 Task 5: Rating Plausibility of Word Senses in Ambiguous Sentences through Narrative Understanding**

Group 22 — CS445 Milestone Presentation

April 13, 2026

---

## Slide 2: Task Description

**What is the task?**

- Given a 5-sentence story containing an ambiguous word (homonym), predict how plausible a given word sense is (rating 1–5)
- Each homonym has 2 senses rated independently
- This is NOT binary WSD — it is graded plausibility prediction

**System Pipeline:**

```
Input: [5-sentence story] + [homonym] + [sense definition]
         ↓
       System
         ↓
Output: Plausibility rating (1–5)
```

---

## Slide 3: AmbiStory Dataset

| | Train | Validation | Test (Locked) |
|---|---|---|---|
| Samples | 2,280 (79.5%) | 588 (20.5%) | 930 |
| Unique Homonyms | 220 | — | — |
| Stories | 1,140 | — | — |
| Annotators/sample | 5 | 5 | 5 |

**Fields per entry:** homonym, precontext (3 sentences), sentence (with homonym), ending, sense definition, example_sentence, 5 human ratings, average, stdev

**Test set is locked — never used during training.**

---

## Slide 4: EDA Highlights (1/2)

**Rating Distribution:**
- Mean rating: 3.14, roughly symmetric across 1–5
- Mean annotator stdev: 0.95 → moderate disagreement (confirms graded nature of task)

**[Include histogram of average rating distribution]**

**Paired Sense Analysis:**
- 1,140 story pairs (each with 2 sense ratings)
- Spearman r = −0.607 (p < 0.001) → when one sense is more plausible, the other tends to be less so
- Both senses can be plausible simultaneously — not always a clear "right" sense

**[Include Sense 1 vs Sense 2 scatter plot]**

---

## Slide 5: EDA Highlights (2/2)

**Story Statistics:**
- Average story length: ~50 words across 5 sentences
- 220 unique homonyms in training set

**Nonsensical Flags:**
- ~10% of samples flagged as nonsensical by at least 1 annotator
- Flagged samples have significantly lower mean ratings

**Train vs Validation:**
- Distributions are consistent (KS test confirms similarity)

**[Include top homonyms bar chart + correlation heatmap]**

---

## Slide 6: Related Papers

**3 papers from approved venues:**

1. **GlossBERT** (Huang et al., **EMNLP** 2019)
   - BERT + sense definitions for WSD → directly applicable encoding strategy

2. **Knowledge Graph WSD** (Bevilacqua & Navigli, **ACL** 2020)
   - Integrates WordNet structure → informs sense relation modeling

3. **Gloss-Informed Bi-Encoders** (Blevins & Zettlemoyer, **ACL** 2020)
   - Bi-encoder for context-gloss matching → efficient alternative approach

*Additional reference (not counted in the 3 — venue not on approved list):*
- **AmbiStory Dataset** (Gehring & Roth, *SEM 2025) — Task organizer paper; LLM benchmarks show fine-tuning >> zero-shot

---

## Slide 7: Proposed Approach

**Baseline:** TF-IDF + Ridge Regression

**Main Model:** BERT/RoBERTa fine-tuning
- Input: `[CLS] story [SEP] sense_definition [SEP]` → regression head → rating (1–5)

**Novel Contributions:**
1. **Joint Sense Comparison** — Cross-encoder scoring both senses together to capture relative plausibility
2. **Narrative-Aware Encoding** — Sentence-level segment IDs (1–5) or hierarchical encoder to model the story's narrative flow

**Evaluation:** Validation set + 5-fold CV on train (seed=42); test set locked until final evaluation

**Metrics:** accuracy@std, Spearman correlation

**[Include pipeline diagram]**

---

## Slide 8: Workload Division & Next Steps
*Workloads are divided equally across 5 members by technical module.*

| Person | Module | Milestone (Done) | Next Steps (Final) |
|---|---|---|---|
| Person 1 | Data & EDA | Dataset download, EDA notebook, locked_test setup | Data pipeline, augmentation |
| Person 2 | Literature (Papers 1-2) | Found & summarized GlossBERT + Bevilacqua papers | Implement GlossBERT-style encoding |
| Person 3 | Literature (Paper 3+) | Found & summarized Blevins + organizer paper | Implement bi-encoder comparison |
| Person 4 | Methodology Design | Drafted baseline + novel contribution plan | Implement baseline + novel contributions |
| Person 5 | Report & Presentation | Wrote report (Sec 1,2,4), created slides | Final report, final presentation |

**Timeline:**
- Apr 14–20: TF-IDF baseline + BERT fine-tuning setup
- Apr 21–27: Main model training + joint sense comparison
- Apr 28–May 4: Narrative-aware encoding + ablation studies
- May 5–18: Evaluation, final report, final presentation

---

## Notes for Presenter (Person 5)

- Total time: 5 minutes STRICT (+ 5 min Q&A)
- Pace: ~40 seconds per slide
- All 5 team members MUST attend
- Rehearse at least once before presentation
- Bring printed backup of slides
