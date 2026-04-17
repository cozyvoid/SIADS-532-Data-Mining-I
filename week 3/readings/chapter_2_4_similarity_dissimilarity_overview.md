# Detailed Overview: Chapter 2.4 — Measuring Data Similarity and Dissimilarity

Source chapter: *Data Mining: Concepts and Techniques*, Section 2.4.

## 1. Big Picture

This chapter explains how to measure **similarity** and **dissimilarity** between data objects. These proximity measures are essential for:

- clustering,
- outlier analysis,
- nearest-neighbor classification.

The chapter’s main idea is that the correct proximity measure depends on the **attribute type** and the **data representation**.

## 2. Similarity vs. Dissimilarity

A similarity measure is larger when objects are more alike. A dissimilarity measure is larger when objects are more different. Typically:

- similarity is 1 for identical objects and 0 for totally unlike ones,
- dissimilarity is 0 for identical objects and increases with difference.

Many similarity measures can be derived from dissimilarity measures, and vice versa.

## 3. Main Subtopics

1. Data matrix versus dissimilarity matrix
2. Proximity measures for nominal attributes
3. Proximity measures for binary attributes
4. Dissimilarity for numeric data: Minkowski distance
5. Proximity measures for ordinal attributes
6. Dissimilarity for attributes of mixed types
7. Cosine similarity for long, sparse vectors

## 4. Data Matrix vs. Dissimilarity Matrix

### Data matrix
A data matrix is an object-by-attribute table with:

- `n` rows for objects,
- `p` columns for attributes.

### Dissimilarity matrix
A dissimilarity matrix is an `n × n` object-by-object structure storing pairwise distances `d(i, j)`.

Important properties:

- `d(i, i) = 0`
- `d(i, j) = d(j, i)`

This distinction matters because many clustering and nearest-neighbor algorithms operate directly on a dissimilarity matrix.

## 5. Nominal Attributes

Nominal attributes have categories with **no intrinsic order**.

### Main idea
Dissimilarity is based on **mismatches**. If two objects match on many nominal attributes, they are more similar.

Let:

- `m` = number of matches,
- `p` = total number of attributes.

Then nominal dissimilarity is based on the proportion of mismatches.

### Notes

- perfect match → dissimilarity 0
- more mismatches → larger dissimilarity
- weights can be used to emphasize certain attributes

### Alternative encoding
A nominal attribute can be recoded as multiple asymmetric binary attributes, one per state.

## 6. Binary Attributes

Binary attributes take values 0/1 for absence/presence.

### Contingency counts
For two objects, the chapter uses:

- `q`: both are 1
- `r`: first is 1, second is 0
- `s`: first is 0, second is 1
- `t`: both are 0

with `p = q + r + s + t`.

### Symmetric binary attributes
Both values are equally important, so all four counts matter.

### Asymmetric binary attributes
Value 1 is more important than 0. In this case:

- ignore `t`, the double-zero matches,
- compute dissimilarity using only `q`, `r`, and `s`.

### Jaccard coefficient
For asymmetric binary similarity:

`sim(i, j) = q / (q + r + s)`

This is the **Jaccard coefficient**, which is especially useful for sparse presence/absence data.

## 7. Binary Example

The chapter includes a patient example where:

- `gender` is symmetric binary,
- symptoms and test results are asymmetric binary.

Using the asymmetric attributes only, the computed dissimilarities show which patient pairs are more likely to have similar disease patterns.

## 8. Numeric Data and Minkowski Distance

For numeric attributes, the chapter introduces the most common distance family.

### Why normalization matters
Attributes measured on larger numeric scales can dominate the distance. Normalization can help put attributes on a more comparable footing.

### Euclidean distance
Straight-line distance in geometric space.

### Manhattan distance
Sum of absolute differences across attributes.

### Metric properties
These distances satisfy the standard metric properties:

- non-negativity,
- identity of indiscernibles,
- symmetry,
- triangle inequality.

## 9. Minkowski Distance Family

Minkowski distance generalizes several familiar numeric distances:

- `h = 1` → Manhattan distance
- `h = 2` → Euclidean distance
- `h → ∞` → supremum distance

This gives a flexible framework for numeric dissimilarity.

## 10. Supremum Distance

Also called:

- `L∞` norm,
- Chebyshev distance,
- uniform norm.

It uses the **largest absolute difference on any single attribute**.

This is useful when the worst-case discrepancy matters most.

## 11. Weighted Euclidean Distance

The chapter notes that weights can be assigned to attributes so that more important features have greater impact on the distance calculation.

## 12. Ordinal Attributes

Ordinal attributes have a meaningful order but unknown spacing between categories.

Examples:

- small, medium, large
- fair, good, excellent

### Chapter method
1. Replace each value by its rank.
2. Normalize the ranks to `[0, 1]`.
3. Apply a numeric distance measure such as Euclidean distance.

This preserves ordering while allowing comparisons on a common scale.

## 13. Mixed-Type Attributes

Real datasets often mix nominal, binary, numeric, and ordinal attributes.

### Two strategies

- analyze each type separately, or
- combine them into a single dissimilarity measure.

The chapter recommends the second approach.

### Combined dissimilarity idea
For each attribute:

- numeric → normalized absolute difference
- nominal/binary → 0 if same, 1 if different
- ordinal → rank, normalize, then treat as numeric

An indicator is used to exclude invalid comparisons, such as missing values or double-zero matches for asymmetric binary attributes.

This yields a unified dissimilarity matrix for mixed data.

## 14. Mixed-Type Example

The chapter works through a small example with:

- a nominal attribute,
- an ordinal attribute,
- a numeric attribute.

It computes the per-attribute dissimilarities first, then combines them into one mixed-type dissimilarity matrix.

The result confirms intuitive similarity relationships among the objects.

## 15. Cosine Similarity

Cosine similarity is introduced for **long, sparse vectors**, especially document term-frequency vectors.

### Why standard distances fail
In sparse text data, many shared zeros do not indicate real similarity. Euclidean-style distances can therefore be misleading.

### Main idea
Cosine similarity compares the **angle** between vectors, not their absolute magnitude.

Interpretation:

- cosine = 0 → no directional match
- cosine close to 1 → strong match

This makes cosine especially useful for document similarity.

## 16. Term-Frequency Vectors

Documents can be represented by high-dimensional vectors where each component is the count of a word or phrase.

These vectors are typically sparse, so cosine similarity is better suited than ordinary distance metrics.

## 17. Tanimoto Coefficient

The chapter also introduces the **Tanimoto coefficient**, a related measure for binary-valued or feature-presence settings.

It compares the number of shared attributes to the number possessed by one or both objects.

Applications mentioned include:

- information retrieval,
- biological taxonomy.

## 18. Key Takeaways

- There is no single universal proximity measure.
- Attribute type determines how similarity or dissimilarity should be computed.
- Shared absences are often not informative in sparse data.
- Normalization is often needed for fair numeric comparisons.
- Jaccard is central for asymmetric binary data.
- Minkowski generalizes multiple distance measures.
- Cosine is especially important for sparse document vectors.
- Mixed-type data needs attribute-specific treatment within one combined framework.

## 19. Important Visuals Mentioned

- **Figure 2.22**: disease influence graph, showing how correlation structure can be visualized.
- **Figure 2.23**: diagram comparing Euclidean, Manhattan, and supremum distances for two numeric objects.

## 20. Final Summary

This chapter provides a practical foundation for measuring proximity in data mining. Its core message is that similarity and dissimilarity must be matched to the structure of the data. The chapter moves from basic representations to type-specific measures and ends with cosine similarity for sparse vectors, laying groundwork for later topics such as clustering, classification, outlier detection, and document analysis.

