# Detailed Overview: *Selecting the Right Interestingness Measure for Association Patterns*

**Authors:** Pang-Ning Tan, Vipin Kumar, Jaideep Srivastava  
**Venue:** SIGKDD 2002  
**Topic:** How to choose an appropriate interestingness measure for association patterns and contingency tables.

## 1. Core Purpose of the Article

This article examines a central problem in association analysis: **different interestingness measures often disagree about which patterns are “best” or most meaningful**. Measures such as support, confidence, lift, correlation-like coefficients, mutual information, Jaccard, and collective strength can rank the same contingency tables very differently.

The paper argues three main ideas:

1. **No single interestingness measure is universally best.**
2. **The right measure depends on the application domain and on the properties you want the measure to have.**
3. In some practical scenarios, many measures become consistent with each other, which simplifies selection.

The authors therefore propose a framework for:
- comparing interestingness measures through their mathematical properties,
- understanding when multiple measures behave similarly,
- and selecting a small representative set of contingency tables so a domain expert can identify a preferred measure efficiently.

---

## 2. Why the Problem Matters

The article frames interestingness selection as a foundational issue in data mining because many tasks depend on evaluating relationships among variables:

- **Association rule mining** needs measures to decide which co-occurrences are meaningful.
- **Feature selection** needs measures to identify variables strongly related to each other or to a target variable.
- **Pattern ranking** depends directly on how “interestingness” is defined.

The key difficulty is that **two measures may tell conflicting stories about the same pattern**. A rule that appears highly interesting under one metric may rank poorly under another.

### Main implication
Choosing an interestingness measure is not a minor technical detail. It directly shapes:
- which patterns are shown to analysts,
- which patterns are pruned,
- and which relationships are treated as actionable.

---

## 3. Contingency Tables as the Analytical Foundation

The paper represents pairwise associations using a **2 × 2 contingency table** for variables **A** and **B**.

|        | B present | B absent | Row total |
|--------|-----------|----------|-----------|
| A present | f11 | f10 | f1+ |
| A absent  | f01 | f00 | f0+ |
| Column total | f+1 | f+0 | N |

These counts support nearly all of the interestingness measures studied in the article.

### Why this matters
This representation lets the authors:
- compare many measures on a common basis,
- study how measures behave under controlled transformations,
- and examine mathematical properties such as invariance, symmetry, and sensitivity to sparse data.

---

## 4. Introductory Example: Different Measures Rank Patterns Differently

The article begins with ten example contingency tables and computes several measures on them. The result is a ranking table showing that the measures often produce **substantially different orderings**.

### Main lesson
A pattern that ranks high under:
- mutual information,
- lift,
- cosine,
- odds ratio,
- or another metric

may rank much lower under a different metric.

This motivates the article’s core question:

> **What properties should analysts examine to choose the right interestingness measure?**

---

## 5. Main Contributions of the Paper

The paper’s contributions are explicit and practical:

### 5.1 Survey of measures
It compiles and compares **21 interestingness measures** from statistics, machine learning, and data mining.

### 5.2 Property-based comparison
It defines a set of **desirable and diagnostic properties** for evaluating measures.

### 5.3 Agreement scenarios
It identifies **two scenarios where many measures agree with one another**:
- support-based pruning,
- and table standardization.

### 5.4 Expert-guided measure selection
It proposes an algorithm for selecting a **small set of contingency tables** that can be shown to a domain expert, so the expert can choose a preferred measure without having to rank every pattern in the data.

---

## 6. Similarity Between Measures

To compare measures systematically, the paper treats each interestingness measure as producing a **ranking over contingency tables**.

### Process
1. Compute the value of measure **M** for every contingency table.
2. Convert those values into a ranking vector.
3. Compare ranking vectors from two measures.

The authors use **Pearson correlation between ranking vectors** as a way to assess whether two measures are similar.

### Interpretation
Two measures are considered similar with respect to a dataset if the correlation between their ranking vectors is above a positive threshold.

### Why this matters
The paper is not only comparing formulas. It is comparing **how measures behave in practice** when used to rank real pattern candidates.

---

## 7. Desired Properties of a Good Measure

The paper discusses well-known desiderata from Piatetsky-Shapiro and others.

### P1. Independence should map to zero
A good measure should be zero when **A and B are statistically independent**.

### P2. Increase with stronger joint occurrence
The measure should **increase as P(A, B) increases**, assuming the marginals remain fixed.

### P3. Decrease when marginal probabilities inflate without stronger joint evidence
If the base rates of **A** or **B** increase while the joint probability does not improve correspondingly, the measure should decrease.

### Why these matter
These properties ensure that a measure behaves in a way consistent with intuitive notions of association strength.

---

## 8. Other Key Properties Studied in the Paper

One of the article’s most useful contributions is that it goes beyond the usual support-confidence discussion and examines structural properties of measures using matrix operations on contingency tables.

## 8.1 Symmetry under variable permutation

A measure is **symmetric** if swapping **A** and **B** does not change the value.

- Symmetric measures are useful when the relationship is undirected.
- Asymmetric measures are useful when direction matters, such as **A → B** versus **B → A**.

### Practical relevance
Use an **asymmetric measure** when analyzing implication rules, because rule direction matters.

---

## 8.2 Row/column scaling invariance

This property tests whether a measure remains unchanged when the rows or columns of a contingency table are scaled.

### Importance
This is crucial when the **relative class sizes or sample compositions differ**, but the underlying relationship should be considered the same.

### Example from the paper
The article discusses a **grade-gender example** adapted from Mosteller. Even when the number of male and female students is scaled unevenly, the underlying association may be considered unchanged. Measures that change substantially under scaling may be undesirable in such domains.

### Key result
The paper notes that **odds ratio**, **Yule’s Q**, and **Yule’s Y** are invariant to row and column scaling.

---

## 8.3 Antisymmetry under row/column permutation

This property tests whether swapping presence and absence in rows or columns flips the sign of the measure.

### Why it matters
Some measures distinguish between:
- positive association,
- negative association,
- and no association.

Others do not make that distinction clearly.

### Practical implication
Measures that are symmetric under these permutations may fail to distinguish positive from negative correlations properly in some applications.

---

## 8.4 Inversion invariance

This property asks whether a measure stays the same when both the presence and absence labels are inverted.

### Why it matters
This becomes important for **binary domains** where presence and absence are **not equally meaningful**.

For example:
- in market basket analysis, **co-presence** of items matters much more than co-absence.
- in some survey or nominal-data settings, inversion may be acceptable.

### Key point
A measure that is inversion-invariant may blur meaningful distinctions in sparse binary data.

---

## 8.5 Null invariance

A measure is **null-invariant** if adding more records where **neither A nor B occurs** does not change the measure.

### Why this matters
In many real datasets—especially sparse transactional data—the number of **null transactions** is huge.

If a measure is heavily influenced by these null transactions, it may produce unstable or misleading assessments.

### Examples highlighted in the paper
- **Cosine** and **Jaccard** are null-invariant.
- Some other common measures are not.

### Practical implication
For sparse market-basket-style data, null-invariance is often highly desirable.

---

## 9. Comparative Study of 21 Interestingness Measures

The article compares 21 measures, including:

- φ-coefficient
- Goodman-Kruskal’s λ
- odds ratio
- Yule’s Q
- Yule’s Y
- Kappa
- mutual information
- J-measure
- Gini index
- support
- confidence
- Laplace
- conviction
- interest (lift-style measure)
- cosine
- Piatetsky-Shapiro’s measure
- certainty factor
- added value
- collective strength
- Jaccard
- Klosgen

### Major conclusion
**None of the measures satisfies all desired properties.**

This is a crucial result. It means measure selection must be:
- application-specific,
- property-aware,
- and sometimes expert-guided.

---

## 10. Support-Based Pruning

The paper studies what happens when support thresholds are imposed.

## 10.1 Many measures become more consistent

Using synthetic data, the authors show that when practical support constraints are added, **many measures become highly correlated with one another**.

### Interpretation
Support pruning can reduce disagreement among measures.

### Why
Constraining support removes many extreme or poorly supported contingency tables, leaving a region where many measures behave similarly.

---

## 10.2 Support pruning removes many poorly or negatively correlated tables

The paper also studies what kinds of tables are removed by minimum-support pruning.

### Key result
When a **minimum support threshold** is used, the eliminated tables are often:
- uncorrelated,
- or negatively correlated.

### Implication
Support pruning is particularly reasonable when the application focuses on **positively correlated patterns**, such as:
- market basket analysis,
- frequent co-occurrence discovery.

---

## 11. Table Standardization

One of the most interesting sections of the paper examines **standardization of contingency tables**.

### Basic idea
Standardization transforms a contingency table so that the row and column margins become equal.

This helps remove bias caused by non-uniform marginals and reveals the underlying association structure more clearly.

---

## 11.1 Mosteller’s motivation

Mosteller argued that standardized tables are useful because they offer a clearer visual and statistical depiction of the relationship after removing distortions due to uneven marginals.

The paper uses the **Iterative Proportional Fitting (IPF)** algorithm to achieve this.

---

## 11.2 Major result: many measures become equivalent after standardization

This is one of the article’s strongest findings.

When contingency tables are:
- standardized,
- and positively correlated,

**all 21 measures considered in the paper produce identical rankings** on the examples studied.

### Why this happens
After standardization, the contingency table takes a constrained form in which many measures become monotonic functions of the same underlying quantity.

### Implication
If the application naturally calls for this kind of standardization, then:
- the choice among many measures becomes far less important,
- and ranking instability is greatly reduced.

---

## 11.3 Standardization is tied to invariance assumptions

The paper also notes that the **choice of standardization scheme** is linked to which measure is preserved.

### Example
- IPF preserves **odds ratio**.
- Other standardization schemes could preserve different measures, such as cosine.

### Practical implication
There is no universally “neutral” standardization. The chosen method reflects assumptions about what kind of association should remain unchanged.

---

## 12. Selecting a Measure Using Domain Experts

When support pruning and standardization do not resolve the issue, the paper proposes a practical alternative:
show **only a small selected set of contingency tables** to a domain expert.

The expert ranks those tables according to perceived interestingness, and the system then identifies which measure best aligns with the expert’s ranking.

---

## 12.1 Why sampling is needed

Ranking all tables is unrealistic in real datasets because:
- there may be thousands or millions of candidate patterns,
- expert attention is limited.

So the challenge is to choose a **small but representative and discriminative subset**.

---

## 12.2 Two sampling strategies

### RANDOM
Randomly choose `k` contingency tables.

### DISJOINT
Choose `k` tables that are:
- as far apart as possible in ranking behavior,
- and likely to create strong disagreement among measures.

This makes the sample more informative for distinguishing measures.

---

## 12.3 DISJOINT algorithm

The DISJOINT method:
1. starts with an oversampled random subset,
2. computes measure rankings,
3. estimates disagreement among tables,
4. and iteratively chooses tables that are maximally separated in ranking space.

### Key intuition
A good sample should contain tables where measures disagree strongly, because those are the most useful for selecting among competing metrics.

---

## 12.4 Empirical result

The paper shows that **DISJOINT outperforms RANDOM sampling**.

Even with a small sample (for example, 20 tables), the similarity structure among measures estimated from the sample is close to what would be obtained from the full set of tables.

### Practical takeaway
You do not need experts to rank everything. A well-chosen small set can be enough.

---

## 13. Main Findings and Takeaways

## 13.1 No universally best measure
Different measures embody different biases and invariance assumptions.

## 13.2 Measure choice should be driven by application needs
You should choose based on the properties the application requires, such as:
- symmetry or asymmetry,
- null invariance,
- scaling invariance,
- sensitivity to negative correlation.

## 13.3 Support pruning can reduce disagreement among measures
This is especially useful in positively correlated pattern mining.

## 13.4 Standardization can make many measures agree
Under standardized, positively correlated tables, many measures rank patterns identically.

## 13.5 Expert-guided selection is feasible
A small, carefully selected subset of tables can help domain experts identify a suitable measure without exhausting manual effort.

---

## 14. Strengths of the Article

### Strong conceptual contribution
The paper reframes interestingness selection as a **property-matching problem**, rather than as a hunt for a universal best measure.

### Strong comparative value
It provides a broad cross-disciplinary comparison spanning:
- statistics,
- machine learning,
- and data mining.

### Practical relevance
The proposed DISJOINT selection approach is directly usable in real-world analytical workflows.

### Important insight on standardization
The result that many measures become equivalent after standardization is especially useful for interpretation and methodology.

---

## 15. Limitations and Cautions

### Focus is primarily on 2 × 2 contingency tables
The framework is very useful, but the paper itself notes that extending to **k-way tables** is more complex.

### Binary-variable emphasis
Many conclusions are strongest for binary association patterns, especially market-basket-style data.

### Expert ranking assumption
The expert-guided method assumes that experts can reliably rank selected tables, which may not always be straightforward in practice.

### Not a domain-specific prescription
The paper does not say “always use X.” Instead, it gives a framework for deciding, which means the analyst must still exercise judgment.

---

## 16. How to Use This Paper in Study or Practice

This article is especially valuable if you are studying:
- association rule mining,
- pattern evaluation,
- interestingness measures,
- contingency-table analysis,
- or feature selection.

### Use it when you need to answer questions like:
- Why are support and confidence not enough?
- Why do lift, Jaccard, odds ratio, and mutual information disagree?
- What properties make a measure suitable for sparse transactional data?
- Why does null-invariance matter?
- How can we choose a measure systematically instead of arbitrarily?

---

## 17. Concise Section-by-Section Outline

## 1. Introduction
- Motivates the problem of conflicting interestingness measures.
- Uses contingency tables as the common representation.
- Shows that rankings can differ dramatically across measures.
- States the paper’s contributions.

## 2. Preliminaries
- Defines ranking vectors for measures.
- Defines similarity between measures using correlation of rankings.

## 3. Properties of a Measure
- Reviews desirable properties from prior work.
- Introduces additional structural properties:
  - symmetry,
  - row/column scaling invariance,
  - antisymmetry,
  - inversion invariance,
  - null invariance.
- Shows no measure satisfies all properties.

## 4. Effect of Support-Based Pruning
- Demonstrates that support constraints make many measures more correlated.
- Shows pruning tends to remove uncorrelated and negatively correlated tables.

## 5. Table Standardization
- Introduces standardization via equal margins.
- Discusses IPF.
- Shows many measures become equivalent after standardization of positively correlated tables.
- Connects standardization schemes to preserved invariants.

## 6. Measure Selection Based on Rankings by Experts
- Proposes RANDOM and DISJOINT table-selection strategies.
- Shows DISJOINT is more effective for identifying a useful measure from small samples.

## 7. Conclusions
- Reiterates that no single measure is best.
- Recommends property-based and expert-guided selection.

---

## 18. Study Notes / Exam-Useful Points

- **Interestingness measures can conflict** because they encode different biases.
- **No single measure** satisfies every desirable property.
- **Null-invariance** is critical in sparse data domains.
- **Odds ratio, Yule’s Q, and Yule’s Y** are scaling-invariant.
- **Support pruning** often removes weak or negative associations and increases measure agreement.
- **Standardization** can collapse many ranking disagreements.
- **DISJOINT sampling** is more informative than random sampling for expert-based measure selection.

---

## 19. Suggested One-Sentence Summary

This paper shows that choosing an interestingness measure for association patterns should be based on the structural properties required by the application, because different measures encode different biases, although support pruning, table standardization, and expert-guided sampling can make the selection problem much easier.

---

## Figures Included

The bundle for this overview may include any source page images that were available in the workspace, especially pages showing:
- the paper title and sample contingency tables,
- rankings across measures,
- the list of interestingness measures,
- contingency-table operations,
- support-pruning similarity matrices,
- standardization,
- and DISJOINT sampling results.

If some images were not available as standalone files in the workspace, the markdown overview still documents the corresponding figures and their roles in the article.
