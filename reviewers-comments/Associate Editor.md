## Associate Editor:

I have received two referee reports, both recommending major revision.

The first referee is positive and finds the material publishable in Mathematical Programming after revision. Both referees view the main idea, using exact decision diagrams over chosen local supports as separation oracles for binary polynomial optimization, as useful and general.

The second referee, however, raises substantial concerns. Several contributions are adaptations of known machinery, and some claims are overstated, in particular the new polynomially solvable class and the facet-defining guarantee. Most importantly, the computational comparison omits the strongest available separation methods. This referee also asks for a more accessible presentation.

I recommend major revision, and the authors should address all comments of both referees. I agree with the second referee that the theoretical contributions alone are not sufficient for Mathematical Programming, so the paper must stand on its computational results. The revised version must include a controlled comparison with the strongest known relaxations, including exact separation of running-intersection and simple odd beta-cycle inequalities and the clique relaxation of Khajavirad and Wang (INFORMS J. Comput., 2026). It can be considered for acceptance only if this comparison shows a clear and consistent advantage, in root gap closed relative to computing time, on LABS and on at least one other benchmark where known inequality families are insufficient, and if this advantage carries over to branch-and-bound under the solver's default settings.


## Overall assessment

The paper offers a useful and general tool, and its LABS results suggest a genuine advantage on dense instances where known inequality families struggle. However, it presents an adaptation of known machinery as more novel than it is, overstates a theoretical result, and builds its strongest computational evidence on a benchmark already solved at the root by exact RI separation, while omitting the exact separators available for RI and simple odd β-cycle inequalities, the presentation is dense and readers unfamiliar with DD-based cutting planes would benefit from more explanation.
I recommend major revision. If, after the requested comparisons, the DD cuts clearly outperform exact running intersection inequalities and simple odd β-cycle separation on LABS and other dense families, in gap closed per unit of time, the paper would merit publication. If they do not, the remaining contributions (the compact encoding, the width analysis and the facet audit) would in my view not suffice for a top-tier journal.
