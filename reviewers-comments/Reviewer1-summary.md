# Reviewer 1: summary of the annotations on the PDF



Source: `Reviewer 1 - annotated0.pdf` (32 PDF pages). The first PDF page is the Editorial Manager cover sheet, so **manuscript page = PDF page − 1**. Reviewer 1 did not send a separate written report. All comments are typed margin notes (blue and red), highlights, and one vertical bracket. Page and line numbers below refer to the manuscript and its line numbering. Reviewer quotes are verbatim.

Legend: 🔴 = substantive (technical) comment · 🔵 = wording / clarity.

## Overview

| # | Location | Type | Topic |
|---|---|---|---|
| 1 | Abstract, l. 21 | 🔵 | "two-links" → "2-links" |
| 2 | Sec. 1 (Introduction), p. 3, l. 36–37 | 🔵 | Sentence hard to read |
| 3 | Sec. 4.2, proof of Prop. 1, p. 11, l. 27 | 🔵 | "remaining" is redundant |
| 4 | Sec. 4.3, proof of Prop. 3, p. 14, l. 26 | 🔵 | Missing "that" |
| 5 | Sec. 5, after model (4), p. 15, l. 44 | 🔵 | Mention polynomial-time solvability of LPs |
| 6 | Sec. 5, model (4), constraints (4b)–(4c), p. 15, l. 52–58 | 🔴 | Obtaining an extreme point of the projection onto *u*; tight arcs ↔ tight (4b)/(4c) |
| 7 | Sec. 5, p. 16, l. 1–6 | 🔴 | An extreme point of (4) need not be an extreme point of the translated polar |
| 8 | Sec. 6, partition-support strategy, p. 17, l. 49 | 🔴 | "Maximal" used without definition |
| 9 | Sec. 7.1, Alg. 3 / Lemma 2, p. 20, l. 1–5 | 🔵 | "cand" reads like a word; refer to line 5 |
| 10 | Sec. 7.1, proof of Lemma 2, p. 20, l. 15 | 🔵 | 𝓜 is a set of vectors, not a matrix |
| 11 | Sec. 7.2, Alg. 4, line 11, p. 22, l. 10–12 | 🔴 | How can a cut be not a local facet but a global one? |
| 12 | Sec. 7.2, maximality check, p. 22, l. 17–35 | 🔴 | If the cut is non-maximal, why not lift it to the maximal inequality? |
| 13 | Sec. 8.1, PTq, p. 23, l. 11–17 | 🔴 | How exactly is *V* partitioned into V₁,…,V_H? |
| 14 | Sec. 9 (Conclusions), p. 29, l. 4–38 | 🔵 | Remove the first two paragraphs of the conclusions |
| 15 | Sec. 9, p. 29, l. 47 | 🔴 | "Randomized" first appears in the conclusions |
| 16 | Sec. 9, p. 30, l. 4–5 | 🔴 | If it is a "research tool", is the implementation shared? |

---

## E-Mail comment

The paper develops mathematical concepts and notation for using established decision diagrams techniques for cutting-plane generation in binary polynomial optimization. The theory part establishes necessary math for the approach (cut generation LP for facets based on an extended formulation; face-dimension computation algorithm) and exploits the natural theoretical questions that relate sizes of decision diagrams to other complexity measures like treewidth. The computational part is certainly of prototype flavor but already yields interesting insights and inspiration for a more professional implementation (e.g., inclusion in a solver). The paper is well readable and uses notation suitably. I think the overall material is publishable in Math Programming, but have several comments in the annotated manuscript.

---

## Substantive comments (🔴)

### 6. Extremality of the CGLP solution (Sec. 5, model (4))
*p. 15, l. 52–58: note at the bottom of the page, below constraints (4a)–(4e) and the paragraph on facet correspondence (l. 48–49).*

> "Later you examine your generated cuts for being facet-defining. Is there any way to construct a solution that yields an extreme-point of the projection onto the u-space?
> Isn't there a correspondence between tight arcs (for your certificate) and tight constraints (4b), (4c)? So if you would construct a basic solution then there should [be] dimension-many binding (and thus tight) constraints and should get linear/affine independence for free since it is a basis."

**What is asked:** Discuss whether the optimal basis of LP (4) can guarantee, or directly certify, that the returned cut is facet-defining. The reviewer argues that the tight arcs used by the certificate (Sec. 7.1, Alg. 3) should correspond to binding constraints (4b)/(4c). A basic solution would then give dimension-many binding constraints with affine independence for free. In other words, the facet certificate might be read off the LP basis instead of being computed separately.

### 7. Extreme point of (4) ≠ extreme point of the polar (Sec. 5)
*p. 16, l. 1–6: next to "If the solution returned by (4) is an extreme point of (MP_G − ω)°, then the associated inequality corresponds to a facet… a unique optimum is sufficient…".*

> "Worth to mention that an extreme point solution of (4) does not necessarily yield an extreme point of the translated polar (because (4) is only an extended formulation)."

**What is asked:** State explicitly that (4) is an extended formulation because of the θ variables. A vertex of (4) may therefore project to a non-extreme point in *u*-space, so "the simplex returns a vertex" is not enough to claim facetness. This is closely related to item 6 and to Reviewer 2's major comment 3 ("Facetness").

### 8. "Maximal" used without definition (Sec. 6, partition-support strategy)
*p. 17, l. 49 (highlighted): "…then its zero-lifting is facet-defining for MP_G whenever it is **maximal** [17]."*

> "You don't explain maximality here. It is difficult to define that in one sentence, but you should rephrase it such that the reader does not think that they should have seen the definition before in the paper."

**What is asked:** Rephrase so that readers do not think the term was already defined. For example, give an informal description or a forward pointer to Sec. 7.2, where the maximality check is described (Del Pia & Khajavirad [17], Prop. 7 and Lemma 2).

### 11. Algorithm 4, line 11: not a local facet but a global facet? (Sec. 7.2)
*p. 22, l. 10–12: next to Algorithm 4 (GlobalFacetAuditViaIK), line 11 "return certified global facet by direct global audit".*

> "How can the cut be locally not facet-defining but then globally a facet? I somehow don't understand how line 11 can occur at all. Probably I misunderstand something here!"

**What is asked:** Explain when line 11 can be reached, that is, when the *I_K* certificate fails at line 5 but the direct global audit (lines 7–11) certifies a facet. The text should clarify that failing line 5 does not mean "not a local facet". It only means the conservative *I_K* certificate is inconclusive, since it is a sufficient condition (Sec. 7.2, p. 21, l. 22–25). If line 11 can actually never occur, it should be removed. An example or a short explanation of why the *I_K* certificate is conservative would help.

### 12. Non-maximal cuts: why not lift them? (Sec. 7.2, maximality check)
*p. 22, l. 17–35: margin note next to the highlighted text "[the candidate inequality is] rejected as a facet of the full multilinear [polytope]" (l. 19). This continues the maximality paragraph on p. 21, l. 44–49, and refers to Alg. 4, lines 1–2.*

> "I understand that it is not a facet in this case. However, can't you then just lift the inequality to the maximal one? The non-maximal inequality is the sum of two other inequalities, which arise by multiplication with a product (or 1 minus a product) and then linearizing. But this should mean that whenever the non-maximal inequality is tight that then both summand inequalities are tight as well, i.e., the non-maximal inequality's face is the intersection of these facets. So you should be able to do a (non-zero) lifting in this case. Or am I missing something?"

**What is asked:** Instead of discarding a non-maximal cut as a "global nonfacet" (Alg. 4, lines 1–2), consider lifting it with a non-zero lifting to the maximal inequality or inequalities whose sum it is. Those summands are tight wherever the cut is tight, so they would give stronger cuts. The response should say whether this is feasible (and possibly implement it) or explain why not.

### 13. How is V partitioned? (Sec. 8.1, PTq strategy)
*p. 23, l. 11–17: next to the description of PTq ("…each PTq strategy considers a vertex partition generated with target size r + q, inducing a section hypergraph as described in Algorithm 2").*

> "Maybe I missed it, but how do you exactly partition V into V_1, V_2, …, V_H?"

**What is asked:** Describe precisely how the partition is built. Sec. 6 (p. 17, l. 41) only says "we arbitrarily partition the set of vertices V", and Sec. 8.1 only gives the target block size r + q. This is related to item 15 (randomization) and to Reviewer 2's comment on whether the blocks V_h are inducing ("Zero-lifting guarantees").

### 15. "Randomized" first appears in the conclusions (Sec. 9)
*p. 29, l. 47 (highlighted): "Finally, the **randomized** nature of the PT family suggests studying multi-seed or adaptive partitioning schemes…".*

> "This is the first occurence of "random" in the paper. Please explain this already in the core sections!"

**What is asked:** Explain in Sec. 6 (Alg. 2) and/or Sec. 8.1 that the partition is random. Report the seed(s), the number of repetitions, and the variability of the results. This goes together with item 13.

### 16. Is the implementation shared? (Sec. 9)
*p. 30, l. 4–5 (highlighted): "…our procedure can be seen as a **research tool** in its own right."*

> "If it is a tool - do you also share its implementation?"

**What is asked:** State whether the code is available (repository or supplementary material), or soften the claim.

---

## Wording and clarity (🔵)

1. **Abstract, l. 21** (highlighted "two-links"): *"in the literature, they are called 2-links"*. Apply the change throughout the paper (Sec. 1, 2.1, 6, 8.1).
2. **Sec. 1 (Introduction), p. 3, l. 36–37:** *"This phrase is quite hard to read."* The highlighted sentence is "Specifically, expanded and, especially, partition supports close substantially more of the root gap with far fewer cuts—up to 92.4% of the gap on LABS and the full gap on VISION—and these stronger relaxations carry over to branch-and-bound (B&B), where the partition setting gives the best practical performance." Consider splitting it.
3. **Sec. 4.2, proof of Prop. 1, p. 11, l. 27** (highlighted "remaining" in "order any remaining vertices of e₀ ∖ ⋃ⱼ eⱼ arbitrarily"): *"redundant? The set difference yields precisely the remaining vertices. Maybe '(remaining) vertices' or so is clearer."*
4. **Sec. 4.3, proof of Prop. 3, p. 14, l. 26** ("We now show the converse also fails"): *"'that' missing?"* Change to "We now show **that** the converse also fails".
5. **Sec. 5, p. 15, l. 44** (after model (4): "together with the structural results of Section 4, a bounded valid-antichain width yields polynomial-time separation"): *"You should mention poly-time solvability of LPs."* The polynomial-time claim should be justified by (4) being a polynomial-size LP and LPs being solvable in polynomial time. Reviewer 2 also asks to state this claim as a proposition (p. 15, l. 42–45).
9. **Sec. 7.1, p. 20, l. 1–5** (highlighted "cand" in "the discrepancy cand − rep[u′] = …"): *"'cand' reads like a word. Please clarify that you mean the variable in the algorithm (maybe refer to line 5)."* Typeset it as `\mathit{cand}`/`\texttt{cand}`, or rename the variable.
10. **Sec. 7.1, proof of Lemma 2, p. 20, l. 15** (highlighted "matrix 𝓜" in "0 ∈ span(𝓜) for any matrix 𝓜"): *"\mathcal{M} is a set of vectors, not a matrix, right?"* Use consistent terminology here and also in "added to the matrix 𝓜" (p. 20, l. 2) and "rank(𝓜)".
14. **Sec. 9 (Conclusions), p. 29, l. 4–38** (vertical bracket over the first two paragraphs): *"These first two paragraphs simply summarize and are essentially a reproduction of intro and results sections. They are also not needed to explain the interesting parts that come below. Without any need I recommend to remove them unless you add new research questions or new insights that require them to be understandable."*

---

## Overlap with Reviewer 2

Several points match `Reviewer2.md`:
- Items 6–7 ↔ major comment 3, "Facetness": extremality of the solution of (4) and the projection of the θ variables.
- Item 5 ↔ major comment 2, "Polynomial-time separation (p. 15, l. 42–45)".
- Items 13 and 15 ↔ major comment 3, "Zero-lifting guarantees": whether the partition blocks V_h are inducing.

---

## Completeness check

- All 32 PDF pages were checked for added text (annotation fonts) and for highlight/ink colours. Every PDF page with an annotation was inspected visually: PDF pp. 2, 4, 12, 15, 16, 17, 18, 21, 23, 24, 30, 31. That is manuscript pp. 1, 3, 11, 14, 15, 16, 17, 20, 22, 23, 29, 30.
- The 16 items above cover every margin note and every highlight. Highlights with no note attached: none.
- The remaining coloured marks in the PDF are the original hyperlink colours (citations and cross-references) and the colours of the result figures. They are not reviewer annotations.
