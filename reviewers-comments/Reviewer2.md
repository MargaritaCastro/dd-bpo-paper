Reviewer #2: Summary
The paper uses exact decision diagrams (DDs), built on local sub-hypergraphs, as separation oracles for the multilinear polytope $MP_G$ of binary polynomial optimization (BPO). The contributions are:
* a compact DD encoding that branches only on vertex variables and recovers hyperedge variables from the states, which also yields an extended formulation of $MP_G$;
* an adaptation of the target-cut CGLP of Tjandraatmadja & van Hoeve [32] to this encoding;
* a characterization of DD width through "valid antichains", with bounded-width results for flowers and cycles and a claimed new polynomially solvable class;
* a DD-based algorithm computing the dimension of the face induced by a valid inequality, used to audit the facetness of the cuts;
* three support-selection strategies (fixed, expanded, partition), evaluated in a cut-and-branch scheme on the LABS and VISION benchmarks.

Strengths
* The compact encoding is natural and well suited to BPO.
* Corollary 1 ($\mathrm{VAW}(G)=2^{\mathrm{pw}(G)}$ for graphs) is a clean link between the DD width and a classical parameter.
* The dimension certificate (Algorithm 3) is simple, correct, and applicable beyond BPO.
* Viewing a DD as a "black-box separator over a chosen support" is a good idea. The fixed-support experiment is a sensible sanity check.
* The LABS results look promising (see major comment 3), on instances known to be hard for existing families of inequalities.

Major comments

1. Novelty and positioning.
* Target cuts over DDs are known (Tjandraatmadja & van Hoeve, 2019). Separating over the exact convex hull of a small substructure and lifting the result is the classical local-cuts paradigm (Applegate et al.; Buchheim, Liers & Oswald, 2008).
* Adapting the CGLP to implicit hyperedge variables only changes the arc weights in the longest-path constraints.
* The authors should state clearly which parts are new and which are adaptations. In particular:     *  Is the DP model of Section 3.1, with only vertex variables in the decisions, original, or does it follow existing work? If the latter, please cite it.     *  Section 5 should explain explicitly, for readers not familiar with [32], how the CGLP differs from [32].
* The extended formulation of Section 3.2 should be compared with existing extended formulations for BPO, in particular the knowledge-compilation approach of Capelli, Del Pia & Di Gregorio [9].     *  That work already obtains polynomial-size extended formulations for BPO from d-DNNF representations, of which OBDDs are a special case.     *  Although [9] appears in the bibliography, it is cited only in Sections 4.2-4.3. It is not cited where the extended formulation is introduced, nor in the abstract and introduction, where the formulation and the facet certificate are listed as contributions.     *  The authors should discuss what the compact DD encoding adds with respect to [9].

2. Theoretical claims.
* Theorem 1 and the "new polynomially solvable class".
    * Theorem 1 follows directly from its assumptions: if an order of polynomial width exists, the DD has polynomial size. It reads more like a technical lemma than a theorem, and it does not by itself exhibit a new class.
    * Polynomial solvability also requires that such an order can be found in polynomial time, which is not discussed.
    * The hypergraph used in Proposition 3 to show that bounded VAW does not imply bounded incidence treewidth, $E=\{A\cup\{b\}: b\in B\}$, lies in a well-understood class, since it is β-acyclic (it is even gamma-acyclic in fact).
    * Similarly, the first family in Proposition 3 (complete binary trees) is Berge-acyclic, and for such hypergraphs the standard linearization is already integral [18].
    * The authors should therefore exhibit a natural family with bounded VAW that is not covered by known tractable classes, such as β-acyclic hypergraphs or hypergraphs of bounded incidence treewidth [9]. Otherwise the claim of a "new polynomially solvable class" should be removed from the abstract, the introduction and the conclusions.
* Polynomial-time separation (p. 15, l. 42-45). The statement that bounded VAW yields polynomial-time separation is, I believe, the actual complexity result. If the authors consider it a contribution, it should be stated as a proposition with its precise scope. Note, however, that it follows immediately from the polynomial-size extended formulation (3).
* Proposition 1.
    * It assumes that petals are pairwise disjoint outside the center. The flower definition of Section 2 does not require this, so the proposition covers only a subclass of flowers.
    * Moreover, the pairwise-disjointness of the petal-center intersections can itself be relaxed [16].
    * The proof should adapt to the more general setting, and I encourage the authors to cover it.
* Literature structures (p. 11). It is not clear which "cycles" are meant: β-cycles as defined in Section 2, or cycle hypergraphs as defined in Section 4.2. The authors should state explicitly which literature results their propositions generalize and which they do not. This is only briefly addressed after Proposition 2.
* Width parameters (Section 4.3). Pathwidth and incidence treewidth are different notions. Please state the known relation between them, and say which graph's pathwidth is meant for hypergraphs of higher rank (the intersection graph?). 


3. Exactness, facetness and the local separation.
* Exact separation (p. 15, l. 46-47). Since the DD is exact for the support, an optimal CGLP value of at most 1 should imply $\bar z_S\in MP_{G_S}$, that is, the separation over the local polytope is exact. Please state this explicitly. As written, a reader may wonder whether a violated cut could still exist.
* Facetness. The abstract states that the method "separates a facet-defining cut". This holds only if the CGLP returns an extreme point of the polar.
    * Does a simplex vertex of (4), which also contains the $\theta$ variables, always project to an extreme point of the polar?
    * Uniqueness of the optimum is sufficient (p. 16), but how can it be checked efficiently?
    * Please explain how extremality is ensured in practice, or soften the claim.
    * The introduction (p. 3, l. 2-5) should also say what can be said about cuts that are not facet-defining: they are still valid (I suppose).
* The interior point $\omega$. Target cuts depend on $\omega$. Please discuss whether this choice is recommended in [7, 32], and how sensitive the results are to it.
* Zero-lifting guarantees. The partition strategy is said to be "backed by zero-lifting guarantees". These require the blocks $V_h$ to be inducing, which the contained-edge rule does not ensure. Please report how often the blocks are inducing, and discuss the fact that hyperedges crossing blocks are ignored entirely.
* Cost of the certificate. Please state the complexity of Algorithms 3 and 4, in particular the rank computation and the construction of the DD over $I_K$. 


4. Baselines and the state of the art. This is my main concern.
VISION: The paper takes the VISION instances from [20] but does not report that paper's results.
* In [20], running-intersection inequalities are separated exactly, in $O(|E|^2)$ for fixed rank.
* Embedded in BARON, this solves all 44 solvable VISION instances at the root node.
* The headline result, full root-gap closure on VISION, is therefore not evidence of superiority over the state of the art and should be presented as a sanity check.
* The LT baseline closes 95.8% of the root gap and leaves five instances unsolved, so it is weaker than what is known for this benchmark.
Running-intersection inequalities: The authors exclude them because they generalize flowers (p. 5, l. 16-17). This reasoning is backwards: the more general family is precisely the right baseline, and it is known to be computationally stronger [20]. Moreover, separating flowers is NP-hard for general rank, whereas exact separation is polynomial for fixed rank, which covers all instances here.
Odd β-cycle inequalities: The baseline uses regular odd β-cycles of length 3 only, separated heuristically.
* On a fixed cycle, regular inequalities dominate the simple ones of [21]. However, the simple ones can be separated exactly, for any cycle length, in strongly polynomial time, provided flowers with at most two neighbours are satisfied.
* [21] reports that they generate no cuts on VISION but close substantial additional root gap on LABS, at a high separation cost.
* They are therefore the natural baseline on the instances where the paper claims its main advantage.
* [21] reports about 74-77% root-gap closure on LABS with flowers and simple odd β-cycles, while the paper reports up to about 92%.
* These numbers are not directly comparable, since the instance sets, base relaxation, solver and stopping rules all differ. But if the gain is confirmed in a controlled comparison, it would be the paper's strongest result.
How the baseline is separated. The paper says it uses "well-known separators" (p. 23, l. 9) and a heuristic search (Section 6) for two-links, flowers and β-cycles. Please specify the separation routines. Not all of these families can be separated efficiently, and the quality of the heuristic directly affects both LT and the seeds of FS and EX.
I ask the authors to:
* (i) add exact running intersection separation and exact simple odd β-cycle separation, on top of flowers with at most two neighbours, as baselines on all instance sets, in the same LP and solver setting;
* (ii) report both gap closed and separation time, possibly also under a common time budget;
* (iii) also compare against Gurobi's default settings, since Cuts=0 is not how a practitioner would run it;
* (iv) revise the abstract and conclusions so that the claims rest on instances where known families are insufficient. 


5. Experimental methodology.
* Strength of the claims. On LABS, every method solves 10 or 11 of 22 instances. The claim that the cuts "markedly accelerate" B&B is supported only on VISION, where it is not informative (see comment 4).
* Reporting. Please state whether B&B times include root separation. Please also report DD sizes (nodes and width per support), CGLP times, DD construction times and the vertex orders used.
* Partitions. The partition strategy is randomized, but results come from a single run. Please report results over several seeds with variance, explain how partitions are generated, and discuss whether a structure-aware choice of partition could do better.
* Scope. The algorithm is cut-and-branch with root cuts only, while running intersection cuts were evaluated at every node of BARON. This should be stated clearly and taken into account in comparisons. 
Presentation and accessibility


6. How the DDs fit into the cutting-plane and B&B framework. It would be nice to explain this a bit more, for readers coming from polyhedral B&B rather than from the DD literature. In particular, the paper should make clear:
* that an exact DD represents the integer hull of its support exactly, and that the fractional points come from the LP relaxation of the full problem, not from the DDs;
* that the cuts are added to that LP, not to the DDs;
* whether several DDs coexist, whether they are rebuilt at each round, and whether any single DD accumulates information.
A short paragraph or a small diagram of the overall loop at the start of Section 6 would help considerably, together with the clarification requested above on how the CGLP differs from [32]. 


7. Density of definitions. The paper introduces many notions: active, compatible and reduced hyperedges, valid antichains, ideals, $\Gamma$-sets, $\mathcal L_i$, $\mathcal E_0(a)$, $\mathcal E_1(a)$, and others. They are not unnecessary, but they are not easy to digest. I suggest:
* one running example, richer than Example 1 (which has at most two nodes per layer), that illustrates all of these notions, not only chains;
* showing, in Example 1, a few root-terminal paths with the corresponding points $(z_v, z_e)$;
* an example where a layer has more nodes, which would also motivate the sets $\Gamma^1_j,\Gamma^0_j$. 


8. The DP model (Section 3.1).
* Terminology. The text calls $z_{v_i}$ the "state variable at stage $i$", whereas in DP terms it is the decision, and the subset $s_i$ is the state (isn't it?).
* "Compatible". The term suggests the existence of a feasible completion. Such a completion always exists here (set the remaining vertices to zero), but a more precise term might be preferable.
* The transition function. The definition of $\phi_i$ is correct but hard to parse, and its verbal description does not match the formula.
    * For $e\in\mathcal A_i$, the condition "$e\notin\mathcal A_{i-1}$" is equivalent to $e\cap U_{i-1}=\emptyset$, since $e\subseteq U_{i-1}$ would contradict $e\in\mathcal A_i$. It therefore identifies hyperedges that become active at stage $i$, and for these $v_i\in e$ necessarily holds.
    * The second condition then reduces to $z_{v_i}=1$ for newly active hyperedges, and is trivially satisfied by carried-over hyperedges not containing $v_i$.
    * I suggest writing the transition explicitly as
$$\phi_i(s_{i-1},z_{v_i})=\{e\in\mathcal A_i : e\cap U_{i-1}=\emptyset,\ z_{v_i}=1\}\ \cup\ \{e\in s_{i-1}\cap\mathcal A_i : v_i\notin e \text{ or } z_{v_i}=1\}.$$
* In the same place, the authors should state that hyperedges in $\mathcal L_i$ leave the state, with $z_e=1$ if and only if $e\in s_{i-1}$ and $z_{v_i}=1$. At present, a hyperedge whose last missing vertex is $v_i$ is not covered by the definition of $\phi_i$, and its treatment through $\mathcal L_i$ only appears later in the text.
* The phrases "the first condition" and "the second condition" in the following paragraph should be aligned with this decomposition.
* The sentence "ignores them if we have already assigned all vertices associated with a specific hyperedge" is redundant, since active hyperedges exclude fully assigned ones by definition. 
9. Valid antichains (Section 4.1).
* The ideal is taken within $(\mathcal R_i,\subseteq)$, which in general is not the full Boolean lattice used in the running example. Please make this explicit.
* The sentence "not every antichain encodes a reachable state" could be illustrated directly.
* In the proof of Lemma 1, "the corresponding ideal" should be defined formally.
* Lemma 1 identifies states with sets of reduced hyperedges. Please state explicitly that distinct hyperedges with the same reduced set are always simultaneously compatible, so that the bijection is well defined.

Minor comments
* p. 1 (abstract). The facet certificate is presented as a contribution. Please indicate its cost and whether it is easy to verify (see comment 3).
* p. 2, l. 39-41. Target cuts are facet-defining for the convex hull of the points the DD encodes. In the BPO setting, please clarify that this is $MP_{G_S}$ of the support, not $MP_G$.
* p. 2, l. 48-49. The contrast between a "named support pattern" and a "chosen support" is unclear. Both are supports; the difference is in how the inequality is obtained.
* p. 3, l. 33-40. The claim of "clear superiority over existing methods" should be revised in light of comment 4.
* p. 8, l. 46-47. "The size of a DD, which translates directly to DD width": the width is defined from the layer sizes, so the implication seems to go the other way.
* p. 9, l. 47-49 and p. 10. The layer indexing is inconsistent. Lemma 1 associates layer $N_i$ with the assignment of $U_i$, whereas the proof of Theorem 1 uses $|\mathrm{VA}_{i-1}(\sigma)|$ for $N_i$.
* p. 11, l. 41-42. "At most two reachable states": please name them ($\emptyset$ and $\{\rho_i(e_0)\}$).
* p. 12, l. 5-7. Could the bound of Proposition 2 be improved by ordering the vertices of $e_0\setminus e_L$ before those of $e_0\cap e_L$?
* p. 14, Proposition 3. The incidence treewidth of the second family is unbounded only if both $p$ and $q$ grow: the incidence graph contains $K_{p,q}$, whose treewidth is $\min(p,q)$. Please state this. See also comment 2 on the acyclicity of both families.
* p. 16, l. 40-42. The "edge-overlap graph" is the line graph (intersection graph) of the hypergraph. Please use the standard name.
* p. 19, Algorithm 3. "Exact DD": all DDs in the paper are exact, so either drop the qualifier or explain the contrast with relaxed DDs.
* p. 21, l. 4-15. It would be simpler to state Algorithm 3 directly for the compact encoding.
* "Facet audit." The term is unusual; "facetness certificate" or "facet check" may be clearer. On p. 4, l. 1, "dimension" should say of what: the face induced by the cut.

Typos and editorial corrections
* p. 1, abstract, l. 22: "hand-derived for fixed support patterns" → "hand-derived from fixed support patterns"?
* p. 2, l. 22: specify $c_v, c_e \in \mathbb R$.
* p. 2, l. 34: "such general cuts": "such" has no antecedent.
* p. 3, l. 11-12: "the target cut procedure" → cite [32] here.
* p. 4, l. 34: spacing between $M$ and $P$ in $MP_G^{LP}$.
* p. 5, l. 38: the subscript in $X_B$ is used before $B$ is defined.
* p. 8, l. 34-36: check the index range in the definition of width.
* p. 9, l. 23: "ignoring all vertices except…": ignoring for what purpose? Please rephrase.
* p. 9, l. 39-45: the explanation of valid antichains is awkwardly phrased; the example helps.
* p. 10, l. 26: "algorithmic primitive" is unclear.
* p. 10, l. 34-35: use $:=$ for the definitions of $W(\sigma)$ and $\mathrm{VAW}(G)$.
* p. 10, l. 45-48: $n$ and $|V|$ are used interchangeably; use one.
* p. 11, l. 14: flower inequalities are not "based on" flowers; they are defined on exactly these structures.
* p. 11, l. 15: "thus, also attractive": remove "also".
* p. 11, l. 18-19: Section 2 denotes the center and petals by $f$ and $T$; Section 4 uses $e_0, e_1,\dots,e_m$. Please unify.
* p. 11, l. 32-33: "While the order is processing" → "While the vertices of … are assigned".
* p. 11, l. 41: "have closed" → "are no longer active".
* p. 11, l. 46: "Let $G$ be a cycle" → "a cycle hypergraph".
* p. 11, l. 49: introducing $L$ with $|E|=L+1$ adds an unnecessary symbol.
* p. 13, l. 41: "prefixes" is used here for the first time; $U_i$ was introduced earlier as "the first $i$ vertex decisions".
* p. 15, l. 12: "so $B$ is exact": "so" suggests a consequence that does not follow from the preceding sentence.
* p. 15, l. 50: "the usual facet correspondence": please state it or cite precisely.
* p. 23, l. 9: "well-known separators": please cite them.