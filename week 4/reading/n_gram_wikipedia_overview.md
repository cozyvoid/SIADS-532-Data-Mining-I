# Detailed Overview: Wikipedia Page — **N-gram**

**Source page:** Wikipedia article on **n-gram**  
**URL referenced by user:** `https://en.wikipedia.org/wiki/N-gram`

---

## 1. Core Definition

An **n-gram** is a sequence of **n adjacent symbols** in a specific order. Depending on the domain, those symbols can be:

- **letters** (including punctuation and spaces),
- **syllables**,
- **words**,
- **phonemes** from speech,
- or **base pairs** in genomic data.

The page frames n-grams as a very general sequence concept used across computational linguistics, speech processing, information retrieval, and biology.

---

## 2. Naming Conventions

The article explains the standard naming scheme based on the size of the sequence:

- **1-gram** → **unigram**
- **2-gram** → **bigram**
- **3-gram** → **trigram**
- larger values are referred to as **four-gram**, **five-gram**, and so on.

The page also notes that in **computational biology**, related sequence units are often called **k-mers**, especially for polymers, oligomers, and genomic sequences.

When the items are words, n-grams may also be called **shingles**.

---

## 3. Why N-grams Matter in NLP

The article highlights a key motivation for n-grams in **natural language processing (NLP)**:

A standard **bag-of-words** representation ignores word order, but n-grams reintroduce at least some local ordering information.

That means n-grams can capture patterns such as:

- common adjacent words,
- short phrase structure,
- local syntax,
- recurring character patterns,
- and context windows that bag-of-words models would miss.

This is one of the main reasons n-grams became foundational in language modeling and text analysis.

---

## 4. Historical Note

The page mentions that **Claude Shannon**, in **1951**, discussed n-gram models of English. This is an important historical reference because it connects n-grams to early probabilistic language modeling and information theory.

The Wikipedia page gives two example outputs inspired by Shannon-style modeling:

- a **3-gram character model** output,
- and a **2-gram word model** output.

These examples illustrate that even simple local probability assumptions can generate text that is somewhat English-like, though often still nonsensical.

---

## 5. Examples Across Disciplines

One of the most useful parts of the page is a table showing how n-grams apply in different fields.

## 5.1 Protein sequencing
- Unit: **amino acid**
- Example sequence: a chain such as `Cys-Gly-Leu-Ser-Trp`
- 1-grams are single amino acids
- 2-grams are adjacent amino-acid pairs
- 3-grams are adjacent amino-acid triples

## 5.2 DNA sequencing
- Unit: **base pair / base symbol**
- Example sequence: `AGCTTCGA`
- 1-grams are single bases
- 2-grams are adjacent pairs like `AG`, `GC`, `CT`
- 3-grams are adjacent triples like `AGC`, `GCT`, `CTT`

## 5.3 Character n-gram language models
- Unit: **character**
- Example sequence: `to_be_or_not_to_be`
- Character n-grams include overlapping windows such as:
  - 1-grams: `t`, `o`, `_`, `b`, ...
  - 2-grams: `to`, `o_`, `_b`, `be`, ...
  - 3-grams: `to_`, `o_b`, `_be`, ...

## 5.4 Word n-gram language models
- Unit: **word**
- Example sequence: `to be or not to be`
- Word n-grams include:
  - 1-grams: `to`, `be`, `or`, `not`, ...
  - 2-grams: `to be`, `be or`, `or not`, ...
  - 3-grams: `to be or`, `be or not`, `or not to`, ...

This section is useful because it shows that the concept is not limited to text: it is really about **adjacent sequence fragments**, regardless of the underlying symbol type.

---

## 6. Markov Model Connection

The page explicitly links n-grams to **Markov models**.

In the examples table, the “order of resulting Markov model” is shown as:

- unigram → order 0
- bigram → order 1
- trigram → order 2

This reflects a standard interpretation:

- a unigram model assumes no dependence on previous items,
- a bigram model conditions on one previous item,
- a trigram model conditions on two previous items.

This is a central conceptual bridge between raw sequence fragments and probabilistic language modeling.

---

## 7. Example N-grams from the Google N-gram Corpus

The article includes sample **3-grams** and **4-grams** from the Google n-gram corpus.

### 3-gram examples
- `ceramics collectables collectibles`
- `ceramics collectables fine`
- `ceramics collected by`
- `ceramics collectible pottery`
- `ceramics collectibles cooking`

### 4-gram examples
- `serve as the incoming`
- `serve as the incubator`
- `serve as the independent`
- `serve as the index`
- `serve as the indication`
- `serve as the indicator`

These examples show how large corpora can be mined to extract frequent phrase fragments and their counts.

---

## 8. Main Conceptual Takeaways from the Page

The Wikipedia page communicates several key ideas clearly:

### 8.1 N-grams are local context windows
An n-gram captures a short span of adjacent symbols, so it preserves **local sequence structure**.

### 8.2 N-grams are flexible
They can be defined over many kinds of symbols:
- letters,
- words,
- phonemes,
- amino acids,
- DNA bases.

### 8.3 N-grams are useful because they preserve order
Unlike bag-of-words features, n-grams record **which items occur next to each other**.

### 8.4 N-grams support probabilistic modeling
They are closely tied to **Markov assumptions** and sequence prediction.

### 8.5 N-grams are used far beyond NLP
The article explicitly points to uses in:
- computational linguistics,
- information retrieval,
- speech processing,
- computational biology.

---

## 9. Related Terms and Linked Topics

The page’s “See also” and navigation sections point to related concepts, especially in NLP. Notable linked topics include:

- **Bigram**
- **Trigram**
- **Bag-of-words**
- **Natural language processing**
- **Word n-gram language model**
- **Google Books Ngram Viewer**

These links suggest that the n-gram concept sits at the center of a broader family of text and sequence analysis methods.

---

## 10. Practical Interpretation

If you are learning or teaching the concept, the page supports this practical interpretation:

An n-gram is a way to convert a sequence into overlapping adjacent chunks of fixed length. That transformation is useful because those chunks can then be:

- counted,
- compared,
- indexed,
- modeled probabilistically,
- or used as features in machine learning systems.

For example:

- in text classification, word or character n-grams become features;
- in language modeling, n-grams define conditional probability contexts;
- in genomics, k-mers summarize adjacent nucleotide patterns;
- in authorship or style analysis, character n-grams capture stylistic habits.

---

## 11. Strengths of the Page

The page is concise, but it does a few things especially well:

- It gives a **clear general definition**.
- It explains the **naming convention** cleanly.
- It provides **cross-domain examples**, not just NLP examples.
- It connects n-grams to **bag-of-words limitations**.
- It links the concept to **Markov models** and historical language modeling work.

---

## 12. Limits of the Page

Because it is a short Wikipedia entry, the page does **not** go deeply into:

- smoothing methods for n-gram language models,
- sparsity problems for large `n`,
- backoff or interpolation,
- practical tokenization issues,
- statistical estimation details,
- or modern comparisons with neural language models.

So it works best as a **definition-and-orientation page**, not a full technical treatment.

---

## 13. Quick Study Summary

### Definition
An n-gram is a sequence of `n` adjacent symbols.

### Common names
- 1 → unigram
- 2 → bigram
- 3 → trigram

### Why useful
They preserve **local order information**.

### Common domains
- NLP
- speech
- information retrieval
- genomics / biology

### Core modeling connection
N-grams correspond naturally to **Markov-style sequence models**.

### Typical examples
- character trigrams,
- word bigrams,
- DNA 3-mers,
- amino-acid sequence fragments.

---

## 14. Suggested One-Sentence Summary

The Wikipedia page presents n-grams as fixed-length adjacent sequence fragments that are widely used to capture local order in language, speech, biological sequences, and other symbolic data.

---

## 15. Note on Images / Figures

The page contains an examples table (“Figure 1”) showing unigram, bigram, and trigram examples across several domains, including proteins, DNA, character language models, and word language models. In this markdown overview, that figure has been summarized in text rather than embedded as an image.
