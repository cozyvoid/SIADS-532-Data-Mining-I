# Detailed Overview: Chapter 11 — Dimensionality Reduction

Source chapter: *Mining of Massive Datasets*, Chapter 11, “Dimensionality Reduction.”

---

## 1. Chapter Purpose and Big Idea

This chapter explains how a **large matrix** can often be replaced by one or more **smaller matrices** that preserve most of the useful structure of the original data. This process is called **dimensionality reduction**. It is motivated by the fact that many important datasets can be represented as matrices, such as the Web transition matrix, utility matrices for recommendation systems, and adjacency-like matrices for social networks. The reduced representation is cheaper to store, faster to manipulate, and still useful for tasks like prediction, approximation, and querying. fileciteturn8file0

The chapter develops dimensionality reduction through three major tools:

1. **Eigenvalues and eigenvectors**
2. **Principal-component analysis (PCA)**
3. **Singular-value decomposition (SVD)**
4. **CUR decomposition** for sparse matrices fileciteturn8file0

---

## 2. Main Structure of the Chapter

The chapter progresses in a logical sequence:

- It begins with the linear algebra foundation of **eigenvalues** and **eigenvectors**.
- It then uses these ideas to explain **principal-component analysis**, where data points are rotated into directions of maximum variance.
- It next introduces **singular-value decomposition**, a more general and powerful matrix factorization method.
- Finally, it presents **CUR decomposition**, which is especially useful when the original matrix is sparse and we want the reduced representation to stay sparse as well. fileciteturn8file0

---

## 3. Eigenvalues and Eigenvectors

## 3.1 Definition

For a square matrix `M`, a scalar `λ` is an **eigenvalue** and a nonzero vector `e` is an **eigenvector** if:

`Me = λe`

That means multiplying the vector by the matrix changes only its magnitude, not its direction. The chapter emphasizes using **unit eigenvectors** to remove scaling ambiguity, and it also adopts a sign convention to reduce the remaining ambiguity. fileciteturn8file0

## 3.2 Interpretation

Eigenvectors represent special directions associated with a matrix. The corresponding eigenvalue tells us how strongly the matrix stretches or scales that direction. In dimensionality reduction, the most important directions are those associated with the **largest eigenvalues**, because those directions capture the strongest structure in the data. fileciteturn8file0

## 3.3 Example in the chapter

The chapter introduces a simple 2 × 2 symmetric matrix and shows one eigenpair explicitly. This example illustrates that:

- the eigenvector must point in the right direction,
- the eigenvalue is recovered by applying the matrix,
- and the eigenvector can be normalized into a unit vector. fileciteturn8file0

---

## 4. Computing Eigenvalues and Eigenvectors

## 4.1 Determinant-based exact method

The chapter first describes the classical exact method:

1. Rewrite the eigen-equation as `(M − λI)e = 0`
2. Require the determinant of `(M − λI)` to be zero
3. Solve the resulting polynomial in `λ`
4. For each eigenvalue, solve for the corresponding eigenvector
5. Normalize the eigenvector to unit length fileciteturn8file0

This method is exact for symmetric matrices and gives all eigenpairs, though it is computationally expensive for large matrices.

## 4.2 Power iteration

The chapter then explains a more iterative and scalable method called **power iteration**. Starting with a nonzero vector, we repeatedly multiply by the matrix and normalize. Over time, the vector converges to the **principal eigenvector**, meaning the eigenvector associated with the largest eigenvalue. The chapter also shows how to estimate the corresponding eigenvalue from the converged vector. fileciteturn8file0

### Why it matters
Power iteration is important because it is practical for large data settings where exact symbolic computation is not.

## 4.3 Finding additional eigenpairs

After the principal eigenpair is found, the chapter explains how to remove its effect by modifying the matrix. Then power iteration can be run again on the modified matrix to obtain the next eigenpair. Repeating this process yields the eigenpairs in decreasing order of eigenvalue size. fileciteturn8file0

---

## 5. The Matrix of Eigenvectors

If the eigenvectors of a symmetric matrix are placed as columns in a matrix `E`, then `E` is an **orthonormal matrix**. That means:

- its columns are unit vectors,
- its columns are mutually orthogonal,
- and `EᵀE = EEᵀ = I`. fileciteturn8file0

This matters because orthonormal matrices can be interpreted geometrically as **rotations** (or rigid transformations) of coordinate systems. That geometric interpretation becomes central in PCA.

---

## 6. Principal-Component Analysis (PCA)

## 6.1 Core idea

PCA takes a dataset consisting of points in a high-dimensional space and finds the directions along which the data varies most. The chapter describes PCA as a way to identify the axes along which the points “line up best.” These axes are determined by the eigenvectors of either `MᵀM` or `MMᵀ`. fileciteturn8file0

## 6.2 Interpretation of principal components

- The **first principal component** is the direction along which the variance of the data is greatest.
- The **second principal component** captures the greatest remaining variance subject to being orthogonal to the first.
- This continues for later components. fileciteturn8file0

## 6.3 Why PCA is useful

PCA is a dimensionality reduction technique because the original high-dimensional data can be replaced by its projection onto the most important axes. Using only the components with the largest eigenvalues gives a lower-dimensional approximation that still captures the most important variation in the data. fileciteturn8file0

---

## 7. PCA Example in the Chapter

The chapter gives a deliberately simple 2D example with four points arranged roughly along the 45-degree line. The purpose of this example is to make the calculations transparent.

The steps are:

1. Represent the points as a matrix `M`
2. Compute `MᵀM`
3. Find the eigenvalues and eigenvectors
4. Form the matrix `E` of eigenvectors
5. Multiply `M` by `E` to rotate the data into a new coordinate system fileciteturn8file0

### Key insight from the example
After rotation, the first axis corresponds to the direction along which the points are most spread out, and the second axis captures the much smaller deviations away from that line. This makes the geometry of PCA very concrete.

The chapter includes several helpful visuals here:

- **Figure 11.1**: the original four points in 2D
- **Figure 11.2**: the same points with axes rotated 45 degrees counterclockwise
- **Figure 11.3**: the points in the new rotated coordinate system fileciteturn8file0

These figures are important because they show PCA not just as algebra, but as a geometric coordinate transformation.

---

## 8. Using Eigenvectors for Dimensionality Reduction

Once the eigenvectors are computed and ordered by decreasing eigenvalue, the best `k`-dimensional approximation is obtained by keeping only the first `k` eigenvectors. If `E_k` is the matrix of the top `k` eigenvectors, then `ME_k` is the reduced representation of the original data. fileciteturn8file0

### Meaning
This reduced representation preserves as much significance as possible for the chosen number of dimensions.

### Example
In the chapter’s 2D example, keeping only the top eigenvector reduces the data to one dimension by projecting all the points onto the principal axis. The chapter explains that this is the **best possible one-dimensional representation** in the variance-preserving sense. fileciteturn8file0

---

## 9. Relationship Between `MᵀM` and `MMᵀ`

The chapter explains that `MᵀM` and `MMᵀ` are closely related:

- they share the same nonzero eigenvalues,
- and their eigenvectors are related through multiplication by `M` or `Mᵀ`. fileciteturn8file0

This relationship matters because sometimes one matrix is smaller and therefore easier to work with computationally. The chapter also includes an example that computes `MMᵀ` and discusses its eigenstructure.

---

## 10. Singular-Value Decomposition (SVD)

## 10.1 Motivation

The chapter presents SVD as a more general and more powerful form of matrix factorization than the simple `UV` factorization seen earlier in the book. SVD gives an exact decomposition of a matrix and also makes it easy to build lower-dimensional approximations by discarding the least important parts. fileciteturn8file0

## 10.2 Definition

If `M` is an `m × n` matrix of rank `r`, then its singular-value decomposition is:

`M = UΣVᵀ`

where:

- `U` is an `m × r` column-orthonormal matrix
- `Σ` is an `r × r` diagonal matrix whose entries are the **singular values**
- `V` is an `n × r` column-orthonormal matrix, so `Vᵀ` has orthonormal rows fileciteturn8file0

## 10.3 Role of each matrix

The chapter gives a strong conceptual interpretation:

- `U` connects **rows** of the original matrix to hidden concepts
- `V` connects **columns** of the original matrix to those concepts
- `Σ` gives the **strengths** of the concepts fileciteturn8file0

This is one of the most important ideas in the chapter.

---

## 11. SVD Example: Movie Ratings and Hidden Concepts

The chapter develops a movie-rating example where:

- rows are users,
- columns are movies,
- and the hidden concepts are movie genres such as **science fiction** and **romance**. fileciteturn8file0

### What the example shows
A low-rank matrix can capture structure very efficiently when the interactions between rows and columns are driven by a small number of latent concepts.

The chapter includes:

- **Figure 11.6**: a rank-2 movie-rating matrix
- **Figure 11.7**: its SVD decomposition into `U`, `Σ`, and `Vᵀ` fileciteturn8file0

### Interpretation
In this example:

- some users align strongly with science fiction,
- others align strongly with romance,
- the singular values show which concept is stronger overall.

This example is central because it explains SVD as a concept-discovery tool, not just a matrix identity.

---

## 12. Approximation by Dropping Small Singular Values

One of the most powerful parts of SVD is that the least important singular values can be set to zero. When this is done:

- the corresponding columns of `U` and `V`
- and the corresponding entries in `Σ`

can be eliminated, producing a lower-dimensional approximation. fileciteturn8file0

## 12.1 Why this works

The chapter shows that this choice minimizes the **root-mean-square error** between the original matrix and the approximation. Equivalently, it minimizes the Frobenius norm of the difference. This is a major theoretical result: the best low-rank approximation is obtained by keeping the largest singular values. fileciteturn8file0

## 12.2 Energy rule of thumb

The chapter gives a practical heuristic: retain enough singular values to preserve about **90% of the energy** in `Σ`, where energy is the sum of squares of the singular values. fileciteturn8file0

### Why this matters
This gives a practical stopping criterion for deciding how many dimensions to keep.

## 12.3 Approximation example

The chapter modifies the movie-rating example slightly to make the rank 3 instead of 2, then drops the smallest singular value to produce a two-dimensional approximation. The relevant figures are:

- **Figure 11.8**: modified movie-rating matrix
- **Figure 11.9**: its SVD
- **Figure 11.10**: the approximation obtained by dropping the smallest singular value fileciteturn8file0

These visuals are useful because they show the approximation process explicitly.

---

## 13. Querying Using Concepts

A particularly interesting section shows how SVD can be used for querying.

### Basic idea
A new person or row vector that was not in the original matrix can be mapped into **concept space** by multiplying by `V`. Once represented in concept space, that new vector can be compared with existing rows or mapped back to estimate missing entries. fileciteturn8file0

### Example in the chapter
The chapter introduces a new user, Quincy, who has rated only *The Matrix*. By mapping Quincy into concept space, the system infers that he is strongly aligned with science fiction and not romance. Mapping back into movie space then predicts which other movies he would likely enjoy. fileciteturn8file0

### Why this is important
This section connects SVD directly to recommendation systems and latent-factor reasoning.

---

## 14. Computing the SVD from Eigenpairs

The chapter explains the tight mathematical connection between SVD and eigenvalues:

- the columns of `V` are eigenvectors of `MᵀM`
- the columns of `U` are eigenvectors of `MMᵀ`
- the singular values are square roots of the nonzero eigenvalues. fileciteturn8file0

This makes SVD computationally tractable once the relevant eigenpairs are known.

---

## 15. CUR Decomposition

## 15.1 Motivation

The chapter points out an important drawback of SVD in large sparse-data applications:

- even if the original matrix `M` is sparse,
- the matrices `U` and `V` are usually dense. fileciteturn8file0

That creates storage and efficiency problems.

CUR decomposition addresses this by using:

- actual **columns** from `M` to form `C`
- actual **rows** from `M` to form `R`
- and a small middle matrix `U` to connect them. fileciteturn8file0

This is especially attractive for sparse matrices because `C` and `R` remain sparse if `M` is sparse.

## 15.2 Main tradeoff

Unlike SVD, CUR is not exact even when the number of selected rows and columns grows, although it converges in theory as more are used. Its advantage is interpretability and sparsity preservation. fileciteturn8file0

---

## 16. Definition of CUR

To build a CUR decomposition:

1. choose `r` columns of `M` to form matrix `C`
2. choose `r` rows of `M` to form matrix `R`
3. define `W` as the intersection of those selected rows and columns
4. compute the SVD of `W`
5. compute the Moore–Penrose pseudoinverse of the singular-value matrix
6. construct the middle matrix `U` from that pseudoinverse and the SVD factors. fileciteturn8file0

The resulting approximation is:

`M ≈ CUR`

---

## 17. How CUR Chooses Rows and Columns

The chapter emphasizes that row and column selection should not be uniform. Instead, rows and columns are sampled with probability proportional to the **squared Frobenius norm** of the row or column. fileciteturn8file0

### Why this matters
Rows and columns with larger norm carry more information, so they should be more likely to be selected.

The chapter develops a detailed example using the movie-rating matrix and computes these probabilities explicitly.

---

## 18. CUR Example

The chapter continues the movie-rating example through the full CUR pipeline:

- selecting representative rows and columns,
- scaling them appropriately,
- forming `W`,
- computing the pseudoinverse-based middle matrix,
- and multiplying everything together to approximate the original matrix. fileciteturn8file0

Important figures in this section include:

- **Figure 11.12**: the original matrix repeated for CUR
- **Figure 11.13**: the CUR decomposition and approximation result fileciteturn8file0

### Main lesson from the example
The approximation is not exact, especially in such a small and contrived example, but it preserves broad structure while keeping the factors sparse.

---

## 19. Handling Duplicate Rows and Columns in CUR

Because CUR samples rows and columns randomly, the same row or column may be selected more than once. The chapter explains:

- duplicates can be merged,
- the remaining row or column is rescaled by `√k` if selected `k` times,
- and the middle-matrix construction can still proceed even if `W` is not square. fileciteturn8file0

This section is important because it handles a practical edge case in randomized matrix sampling.

---

## 20. Summary of the Chapter’s Most Important Ideas

The chapter’s summary section highlights these major points:

- **Dimensionality reduction** replaces a large matrix with smaller matrices that approximately reconstruct it.
- **Eigenvalues and eigenvectors** identify important directions associated with a symmetric matrix.
- **Power iteration** provides a practical way to compute eigenpairs.
- **PCA** finds directions of maximum variance and uses them for lower-dimensional approximation.
- **SVD** decomposes a matrix into interpretable latent concepts and enables optimal low-rank approximation.
- **Queries in concept space** allow new rows or hypothetical examples to be analyzed efficiently.
- **CUR decomposition** preserves sparsity by building the approximation from sampled rows and columns of the original matrix. fileciteturn8file0

---

## 21. Key Differences Among PCA, SVD, and CUR

## PCA
- best for understanding directions of variance in point data
- built from eigenvectors of `MᵀM` or `MMᵀ`
- most naturally geometric

## SVD
- most general and powerful decomposition in the chapter
- exact when full rank is retained
- optimal for low-rank approximation in Frobenius/RMSE terms
- reveals hidden concepts linking rows and columns

## CUR
- designed for sparse matrices
- keeps sampled rows and columns of the original matrix
- more interpretable and sparse-preserving
- approximate rather than exact fileciteturn8file0

---

## 22. Visuals and Diagrams in the Chapter

The chapter includes several diagrams and worked matrix visuals that are especially helpful:

- **Figure 11.1**: four points in 2D space
- **Figure 11.2**: the same points with axes rotated 45 degrees
- **Figure 11.3**: the points in the new coordinate system
- **Figure 11.4**: eigenvector matrix for `MMᵀ`
- **Figure 11.5**: the form of an SVD
- **Figure 11.6**: movie-rating matrix
- **Figure 11.7**: SVD of the rank-2 movie-rating matrix
- **Figure 11.8**: modified movie-rating matrix
- **Figure 11.9**: SVD of the modified matrix
- **Figure 11.10**: reduced SVD approximation
- **Figure 11.11**: exercise matrix
- **Figure 11.12**: matrix reused for CUR
- **Figure 11.13**: CUR decomposition result fileciteturn8file0

These visuals are especially valuable because the chapter is highly algebraic, and the figures make the geometric and structural ideas easier to follow.

---

## 23. Practical Takeaways

If you are studying this chapter for applications in data mining, the most useful practical takeaways are:

- large matrices often contain structure that can be captured with far fewer dimensions
- the most important dimensions correspond to the strongest eigenvalues or singular values
- PCA gives a principled low-dimensional view of data points
- SVD is the best low-rank approximation tool in the chapter
- CUR is preferable when preserving sparsity matters
- latent-concept representations can support better prediction, recommendation, and querying. fileciteturn8file0

---

## 24. One-Paragraph Chapter Summary

Chapter 11 shows how to reduce the complexity of large matrices without losing most of their useful structure. It begins with eigenvalues and eigenvectors, then uses them to motivate PCA as a way to rotate data into directions of greatest variance. It then introduces SVD, which factorizes a matrix into latent concepts and provides the best low-rank approximation when small singular values are dropped. Finally, it presents CUR decomposition, which sacrifices exactness in exchange for preserving sparsity by sampling actual rows and columns from the original matrix. Together, these methods form a core toolkit for scalable matrix analysis in data mining. fileciteturn8file0
