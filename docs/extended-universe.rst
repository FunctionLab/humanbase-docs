==================================
Extended universe (legacy mode)
==================================

The ``extended_universe`` URL parameter reproduces the cross-organism
term-enrichment behavior used in
:doc:`Functional Module Detection <modules>` and
:doc:`Tissue-specific Networks <functional-networks>` on GIANT
networks between February 2024 and April 2026. New analyses default
to human-only; MAGE networks have always used a human-only annotation
universe and the flag does not apply to them.

When to use it
==============

Use ``extended_universe=true`` only when you need to reproduce a result
from a publication, figure, or saved link generated between February
2024 and April 2026.

What it changes
===============

Term enrichment in :doc:`Functional Module Detection <modules>` uses
a one-sided Fisher's exact test followed by Benjamini–Hochberg
correction. Two inputs to that test depend on which annotation
universe is in effect:

* **Term size (K).** The number of genes annotated to the term.
* **Background universe (N).** The total number of genes considered
  available for annotation.

In the current default mode, both K and N are computed from
human annotations only. In extended-universe mode, K and N include
annotations from non-human organisms that were carried over from
the source databases: mouse (*Mus musculus*), zebrafish (*Danio
rerio*), fruit fly (*Drosophila melanogaster*), nematode worm
(*Caenorhabditis elegans*), and budding yeast (*Saccharomyces
cerevisiae*).

A note on Q values
------------------

Functional Module Detection now computes term enrichment Q values using a one-sided
Fisher's exact test (the upper-tail probability ``hypergeom.sf(k - 1)``),
as described in Krishnan et al. (2016) Genome-wide prediction and
functional characterization of the genetic basis of autism spectrum
disorder. *Nature Neuroscience*. From November 2017 through
April 2026, the calculation instead used the point probability of
observing exactly the seen overlap (``hypergeom.pmf(k)``).

``extended_universe=true`` restores the point-probability calculation
along with the cross-organism universe, so any result produced between
February 2024 and April 2026 can be reproduced exactly. Results from
before February 2024 will have matching test statistics, but term-size
and universe definitions will differ from what this flag restores. If
you need to reproduce a result from before February 2024, please get
in touch and we will help you recover matching values.

A note on data versioning
-------------------------

The ``extended_universe`` flag restores the legacy *code path*, but
it cannot, on its own, restore the legacy *data state*. HumanBase
imports gene records and term annotations from external sources
(NCBI, Gene Ontology, MSigDB, MeSH, and others) on its own
schedule. Two quantities used by the hypergeometric test drift
between releases independently of any term-release version label:

* the **gene universe (M)** — the set of distinct genes with at
  least one annotation, summed across the loaded organisms; and
* each **term size (K)** — the number of distinct genes annotated
  to a given term.

In practice this means a community page rerun today with
``extended_universe=true`` will reproduce the exact statistical
calculation used in the legacy code path, but the inputs M and K
reflect today's annotation tables, not the tables present at the
time of the original run. Q values shift in proportion to how much
the underlying data has drifted since.

This is a known limitation. Long-term reproducibility is on the
roadmap, achieved by pinning gene and term snapshots per HumanBase
release so the data state itself is versioned.

Where it applies
================

The flag is supported only for **GIANT** networks (the original
human tissue and biological-process networks). MAGE network analyses
have used a human-only annotation pipeline from the start, so there
is no legacy cross-organism behavior to reproduce. Requests
that combine ``extended_universe=true`` with a MAGE network are
rejected by the API.

The flag applies to the GIANT version of:

* :doc:`Functional module detection <modules>` — term enrichment for
  each detected community.
* :doc:`Tissue-specific networks <functional-networks>` — annotated
  term tables on gene pages (Process and Tissue tabs).

It does not affect network edge weights, gene-prediction scores, or
any non-enrichment output.

How to use it
=============

Append ``extended_universe=true`` to the URL of a community page or
gene page. For example:

* Functional module detection result::

    https://humanbase.io/module/overview/?body_tag=<job_id>&extended_universe=true

* Gene page::

    https://humanbase.io/gene/3553/blood?extended_universe=true

The interface displays a banner whenever the flag is active, so it
is always visible whether a page is in extended-universe or
default mode. On a non-GIANT page the flag is ignored, removed from
the address bar, and a warning banner explains that the request
fell back to human-only data.

Programmatic access
===================

The same parameter is forwarded to the underlying API endpoints
(``/community/`` and ``/terms/annotated/``). Scripts replaying
historical analyses through the API should append
``&extended_universe=true`` to the query string.
