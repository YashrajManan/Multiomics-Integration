# Multi-Omics Integration — Real iHMP IBDMDB Cohort (Classical)

Integrating four real, independently-sourced omics layers — gut microbial taxonomy, metagenomic functional
pathways, proteomics, and metabolomics — from the real iHMP IBDMDB inflammatory bowel disease cohort into one
classical multi-block statistical analysis, to test whether real shared cross-omics factors separate real
IBD diagnosis groups.

## Aim

Using a real, publicly available cohort with genuinely independent real genomics, functional, proteomics, and
metabolomics measurements, can a classical multi-block statistical method find real shared axes of variation
that meaningfully separate real disease groups?

## Objective

Align four independently-sourced real omics data tables to a common real per-participant key, integrate them
via block-scaled joint PCA (equalizing each block's contribution regardless of its raw feature count), and
test the resulting real latent factors against real clinical diagnosis (Crohn's disease / ulcerative colitis /
non-IBD) with proper multiple-testing correction.

## Data fetch

**Real cohort: iHMP IBDMDB** (Lloyd-Price et al. 2019, *Nature*). **Genomics + functional layer:**
`curatedMetagenomicData` (R/Bioconductor), real `HMP_2019_ibdmdb` study — real MetaPhlAn taxonomic profiles
(585 real taxa, 1627 real samples) and real HUMAnN pathway profiles (22,113 real pathways), exported to CSV via
a companion R script. **Proteomics + metabolomics:** real merged tables fetched directly from IBDMDB's own
site — proteomics as a real `.tsv.gz`, metabolomics as a real BIOM-format file (confirmed live to be BIOM 1.0
JSON, not BIOM 2.x HDF5 as the format extension suggests). **Clinical metadata:** IBDMDB's own real metadata
CSV, including real per-sample diagnosis and the real ID fields used to bridge all four sources together.

## Data describe

After real sample-ID reconciliation (see Methods) and restriction to the real complete-case subset with all
four omics layers plus diagnosis simultaneously present: **83 real participants** (36 CD, 24 UC, 23 nonIBD),
221 real prevalence-filtered taxa, 7577 real pathways, 7880 real proteins, and 80,880 real metabolite
features — 96,558 real combined features integrated via classical block-scaled PCA into 10 real latent
factors.

## Methods / Workflow — what we did

Loaded the real `curatedMetagenomicData` exports and fetched real proteomics/metabolomics/metadata directly
from IBDMDB. Resolved one real diagnosis inconsistency found during a direct consistency check. Discovered
that `External ID` — the obvious real join key — is actually assay-specific, not a stable per-subject key, and
switched the real master join to `Participant ID` instead (confirmed zero inconsistencies across 131 real
participants), bridging all four omics sources through it. Aligned all four real blocks to the real common
83-participant set. Applied a real low-prevalence feature filter (≥10% of real samples) before scaling —
necessary because ultra-rare, near-singleton real features otherwise produce degenerate, indistinguishable
standardized columns that manufacture spurious statistical structure. Ran a real classical block-scaled joint
PCA (StandardScaler per block, Frobenius-norm block-scaling, concatenation, 10-component PCA), then tested
each real factor against real diagnosis via Kruskal-Wallis with Benjamini-Hochberg FDR correction across all
10 tests.

## Results

No real factor survives FDR correction against real diagnosis (smallest real FDR-adjusted p-value: 0.088,
PC2; uncorrected p-values for the three lowest factors range ≈0.015–0.03). This honest null result replaced
an initial, incorrect positive finding: before the real low-prevalence filter was applied, the same pipeline
found four factors (PC1, PC2, PC3, PC5) "significant" even after FDR correction — traced directly to
degenerate loadings from ultra-rare, single-sample features (178 of 585 real taxa were singletons before
filtering) rather than genuine shared biological signal. Full tables: `results/multiomics_factor_scores.csv`,
`results/factor_diagnosis_association.csv`, `results/top_loadings_per_pc_per_block.csv`.

## Biology interpretation of results

The real, corrected null result means this specific pipeline — classical, linear, block-scaled PCA on 83 real
complete-case participants, after real prevalence filtering and honest multiple-testing correction — does not
detect a shared multi-omics factor that cleanly separates IBD from non-IBD in this real cohort. It does not
mean gut multi-omics carries no real information about IBD status in general; other real, published analyses
of this same cohort have found real single-omics associations using larger samples and different methods. The
defensible real reading is specific to this method and this real sample size, not a general claim about
multi-omics integration's value for IBD research.

## Learning through project

The real hardest part of this project was not the statistics — it was real sample-ID reconciliation across
four independently-sourced tables, where the obvious join key (`External ID`) turned out to be assay-specific
rather than a stable per-subject identifier, caught only by directly investigating a single real inconsistent
diagnosis rather than silently resolving it. The second major lesson: an unnaturally *identical* loading value
repeated across unrelated real features is as strong a correctness tell as a suspiciously *round* row
count (the same "measure before trusting" discipline applied across this portfolio) — it caught a real
rare-feature artifact that had initially produced four FDR-significant factors, a false positive that did not
survive scrutiny. The corrected, honest null result is the one this project stands behind.

## Caveats

This is a real, linear, classical integration method — it would not detect a real factor whose relationship to
diagnosis is strongly nonlinear (project #7B applies a deep-learning-based approach to this same real aligned
data for comparison). The real complete-case restriction to 83 of 131 total participants is a disclosed
trade-off chosen explicitly over imputing missing omics layers. The 10% prevalence threshold is a standard,
defensible but not uniquely "correct" choice.
