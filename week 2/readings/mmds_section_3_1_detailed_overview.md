# Chapter 3.1 Overview: Applications of Near-Neighbor Search

Source: *Mining of Massive Datasets*, Section 3.1, “Applications of Near-Neighbor Search.”

![Figure 3.1: Two sets with Jaccard similarity 3/8](./images/mmds31_figure_3_1.png)

## Big Picture

Section 3.1 introduces **near-neighbor search** through the lens of **set similarity**. The section’s core idea is that many practical similarity problems can be reframed as questions about how much two sets overlap. The main similarity measure introduced is **Jaccard similarity**, and the section shows why it is useful for:

- detecting **textually similar or near-duplicate documents**
- supporting **collaborative filtering** for users and products
- extending similarity ideas from ordinary sets to **bags (multisets)** when ratings matter

The section also serves as a bridge into Section 3.2, where document similarity is turned into a set problem using **shingling**.

## 3.1.1 Jaccard Similarity of Sets

### Definition

The **Jaccard similarity** between sets \(S\) and \(T\) is:

\[
SIM(S,T) = \frac{|S \cap T|}{|S \cup T|}
\]

This measures the fraction of total distinct elements that the two sets have in common.

### Interpretation

- A value of **1** means the sets are identical.
- A value of **0** means they have no overlap.
- Intermediate values reflect partial overlap.

### Example from the section

In **Figure 3.1**, the two sets share **3** elements in their intersection and contain **8** total elements in their union, so:

\[
SIM(S,T)=\frac{3}{8}
\]

### Why Jaccard similarity is useful

Jaccard similarity is especially useful when:

- items are naturally represented as **sets**
- overlap matters more than absolute size
- we want a normalized measure that stays comparable across different set sizes

## 3.1.2 Similarity of Documents

This subsection explains why Jaccard similarity works well for **textual similarity** problems, especially when the goal is not semantic similarity, but **character-level or lexical overlap**.

### Key distinction: textual similarity vs. semantic similarity

The section emphasizes that this is **not** the same as understanding meaning.

- **Textual similarity** asks whether two documents share lots of the same text.
- **Semantic similarity** asks whether two documents mean similar things, even if written differently.

Section 3.1 focuses on the former.

### Why exact matching is not enough

Checking whether two documents are exact duplicates is easy: compare them character by character. But many real-world duplicates are only **near duplicates**. They may contain the same core text with small edits, reordering, deletions, or added material.

### Main applications discussed

#### 1. Plagiarism detection

A plagiarized document may:

- copy only parts of the original
- change a few words
- reorder sentences
- still preserve a large fraction of the original text

Because of this, exact matching fails, but overlap-based similarity can still expose the relationship.

#### 2. Mirror pages

Popular or important websites often have **mirror sites** that share almost the same content.

Differences may include:

- host-specific information
- links to other mirrors
- small structural changes

Search engines want to detect these near duplicates so they do not return many nearly identical pages in top results.

#### 3. News articles from the same source

A wire-service article may be republished by many outlets, each with small edits such as:

- trimming paragraphs
- adding local commentary
- wrapping the article in site-specific branding, ads, or links

News aggregators need to identify these as versions of the same underlying story.

### Core takeaway

For document comparison, the section motivates representing documents in a form where **shared text fragments** can be compared as set overlap. This leads directly to **shingling** in Section 3.2.

## 3.1.3 Collaborative Filtering as a Similar-Sets Problem

This subsection shows that Jaccard similarity is useful beyond documents. It also supports **collaborative filtering**, where systems recommend items to users based on the behavior of similar users or the similarity of items.

### Basic idea

There are two parallel ways to define similarity:

- **Similar customers**: customers whose purchased-item sets overlap strongly
- **Similar items**: items whose purchaser sets overlap strongly

This creates a set-based view of recommendation.

### Example: online purchases

For a retailer like Amazon:

- each customer can be represented by the **set of items purchased**
- each item can be represented by the **set of customers who bought it**

Then:

- two customers are similar if their purchased-item sets have high Jaccard similarity
- two items are similar if their purchaser sets have high Jaccard similarity

### Important nuance

Unlike mirror-page detection, collaborative filtering usually does **not** require extremely high similarity.

The section notes that:

- mirror pages may need **90%+** similarity
- customer or item similarity can be meaningful even at much lower levels, such as **20%**

That is because user behavior is naturally more diverse than copied web pages.

### Role of clustering

The section points out that similarity alone may not be enough. For example:

- two science-fiction readers may each buy many science-fiction books
- but they may share only a few exact titles

By combining similarity with **clustering**, we can group related items and define stronger, higher-level similarity based on shared groups rather than exact item overlap.

### Example: movie ratings

For Netflix-like data:

- customers can be compared based on the movies they rented or rated highly
- movies can be compared based on the sets of customers who rented or rated them highly

Again, exact overlap may be modest, but still meaningful.

## Working with Ratings Instead of Binary Choices

The section explains that ratings complicate the pure set model because the data are no longer just yes/no decisions. It proposes three strategies.

### 1. Ignore low ratings

Treat low-rated movies as if the customer never watched or liked them.

**Effect:** simplifies the problem back into a set model, but throws away information.

### 2. Split each movie into “liked” and “hated”

For each movie, use two possible set elements:

- “liked”
- “hated”

Then:

- a high rating adds the “liked” version to the customer’s set
- a low rating adds the “hated” version

**Effect:** preserves polarity of opinion while still using set similarity.

### 3. Use bags (multisets)

If ratings are on a 1-to-5-star scale, place the movie into the customer’s collection **n times** if the user gave it **n stars**.

This changes the representation from a set to a **bag**, where duplicates matter.

## Jaccard Similarity for Bags

For bags \(B\) and \(C\), the section defines a bag version of Jaccard similarity.

### Intersection for bags

An element is counted in the intersection as many times as the **minimum** of its counts in the two bags.

### Union for bags

The section uses the version where the union size is the **sum** of the multiplicities from both bags.

### Example from the section

For the bags:

- \(\{a,a,a,b\}\)
- \(\{a,a,b,b,c\}\)

the intersection counts:

- \(a\) twice
- \(b\) once

So the intersection size is **3**.

The union size is the sum of the bag sizes:

- first bag size = 4
- second bag size = 5
- union size = 9

So the bag similarity is:

\[
\frac{3}{9}=\frac{1}{3}
\]

### Interpretation

The section notes that the **maximum possible Jaccard similarity for bags**, under this union definition, is **1/2**, not 1. Even so, a score of **1/3** indicates the bags are fairly similar.

## Why Section 3.1 Matters

This section is foundational because it establishes several major ideas used throughout the chapter:

### 1. Similarity can be turned into a set-overlap problem
Many practical tasks become manageable when items are represented as sets and compared using overlap.

### 2. Jaccard similarity is normalized
It accounts for both shared content and total distinct content, making it more informative than just counting common elements.

### 3. Near-duplicate detection is a major real-world application
Plagiarism, mirrors, and republished articles all motivate scalable similarity search.

### 4. Recommendation systems can also be framed as similarity search
Users and items can both be represented as sets, enabling collaborative filtering.

### 5. Real data often require richer representations
Ratings motivate moving from sets to bags and adapting similarity measures accordingly.

## Key Terms and Concepts

### Near-neighbor search
Finding items that are highly similar to a given item or to one another.

### Jaccard similarity
A set-based similarity measure defined as intersection size divided by union size.

### Near duplicate
Two objects that are not exactly identical but share a large amount of content.

### Collaborative filtering
A recommendation approach that uses similarities among users or items to suggest likely preferences.

### Bag (multiset)
A collection in which elements may appear multiple times.

## Section Summary

Section 3.1 introduces **Jaccard similarity** as the main tool for measuring overlap between sets and shows how that measure supports two major application families:

1. **Document similarity**, especially for detecting plagiarism, mirror pages, and duplicate news articles
2. **Collaborative filtering**, where customers and items are compared through overlapping behavior

The section also prepares for later techniques by showing that document similarity can be reframed as a set problem and by extending the idea from ordinary sets to bags when rating intensity matters.

## Study Notes / What to Remember

- Jaccard similarity compares **shared elements** relative to **all elements present in either set**.
- It is well suited for **overlap-based similarity**, not semantic understanding.
- Exact duplicate detection is easy; **near-duplicate detection** is the harder and more important problem here.
- In recommendation systems, useful similarity may be **much lower** than in duplicate-page detection.
- Ratings can be handled by:
  - ignoring low ratings,
  - splitting items into liked/hated variants,
  - or using bag-based Jaccard similarity.
- Section 3.2 builds directly on this by introducing **shingling**, the method for converting documents into sets of substrings.
