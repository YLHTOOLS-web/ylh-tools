# YLH Semantic Layer

**A transparent research layer for the Hebrew Bible.**

Most biblical software can tell you what the Hebrew text says
grammatically. This project makes something else visible: how a
scholar moves *from* that grammar *to* a historical, anthropological,
ecological, political, or theological interpretive claim — and lets
that whole chain be inspected, not just asserted.

This repository extends the ETCBC/BHSA Hebrew Bible corpus with a new,
custom Text-Fabric node type — `ylh_textual_grouping` — built using the
**Yahwist Liberation Hermeneutic (YLH)**, a methodology that reads
biblical texts as sites of competing cosmologies, and distinguishes
traditions that resist domination from those that stabilize hierarchy
and imperial order. Rather than embedding that interpretation inside
the text itself, YLH keeps every layer separate and honestly labeled:

```text
Hebrew Text
      ↓
ETCBC Linguistic Annotation      (grammatical fact)
      ↓
YLH Textual Classification
      ↓
YLH Analytical Objects
      ↓
Scholarly Claims                 (YLH argument)
      ↓
Evidence / Sources / Counterarguments / Confidence
```

**YLH does not modify the Hebrew Bible. It extends the research
environment around it.** Every interpretive claim remains explicitly
separate from the linguistic data that anchors it, so a reader can
inspect, challenge, replace, or extend the reasoning without altering
the underlying corpus.

## Why this architecture?

Traditional biblical software excels at linguistic analysis.
Traditional commentary excels at interpretation. Very little lets you
inspect both at once, at the same location in the text, without
collapsing the grammatical fact and the scholarly claim into one
undifferentiated blob. This project keeps grammatical fact (from
ETCBC) and scholarly argument (from YLH) in clearly separated,
honestly-labeled layers, so a reader can see:

- which textual evidence a claim rests on
- which historical or comparative assumptions it makes
- which sources it cites
- what counterarguments exist against it
- how confident the annotator actually is, stated categorically
  rather than with false-precision decimals

**Status: proof of concept.** This pilot covers three word occurrences
across two verses of Psalm 29 (the water/flood/chaos imagery in vv.
3 and 10). It demonstrates the methodology and a real, working
pipeline — it is not yet a populated annotation of the Psalter or the
wider Hebrew Bible. Treat this as working infrastructure, not a
finished dataset. The underlying architecture (separating grammatical
fact, textual classification, analytical objects, and scholarly claim
into distinct layers) doesn't inherently require a liberation-
hermeneutic lens — other interpretive traditions could in principle
plug into the same structure. That's a possible future direction, not
a current claim this repository makes.

## Philosophy

YLH is built on a simple principle:

> **The text is not the interpretation.**

The Hebrew text, the linguistic annotation, the analytical
classifications, and the scholarly claims occupy different layers of
the system because they represent different kinds of knowledge. By
keeping those layers distinct, YLH allows interpretation to remain
transparent, inspectable, revisable, and reproducible — without
confusing scholarly argument with the textual data itself.

## What this is NOT

This repository does **not** include a copy of the Hebrew Bible text,
the ETCBC/BHSA corpus, or any of ETCBC's grammatical/linguistic
annotation data. It contains only:

- This project's own new node type and features (`ylh_unit_id`,
  `ylh_claim_id`)
- The Python build script that constructs that node type
- The external YLH claims registry (the actual scholarly argument
  content, in `claims_registry` — sources, claims, counterarguments,
  confidence ratings)

Anyone using this repo must separately download the real BHSA data —
the build script does this automatically via Text-Fabric, and Text-
Fabric's tooling handles the licensing terms below on its own.

## Attribution and licensing — what you must do

The underlying BHSA corpus (Biblia Hebraica Stuttgartensia
Amstelodamensis) is published by the Eep Talstra Centre for Bible and
Computer (ETCBC) under **Creative Commons Attribution-NonCommercial
4.0 International (CC BY-NC 4.0)**. Confirmed directly from ETCBC's
own repository and documentation (github.com/ETCBC/bhsa,
etcbc.github.io/bhsa). Per their stated terms:

- You may download, process, copy, and modify the data.
- You may build new software applications on top of it (this is
  exactly that).
- You may use it for research and publish results.
- **You must give proper attribution** when you publish the data or
  results derived from it, citing ETCBC's persistent identifier:
  `10.17026/dans-z6y-skyh`
- **No commercial use without consent** — for any commercial
  application, ETCBC directs you to contact the German Bible Society
  directly.

### Practical checklist before you push to GitHub

1. **Add a LICENSE file for your own code** (this repo's Python script
   and any original YLH content) — MIT or Apache 2.0 are standard,
   permissive choices for code, and don't conflict with BHSA's terms
   since you're not redistributing their data.
2. **Add the attribution line above** (with the DOI) somewhere visible
   — this README, or a `CITATION.md` file. This is the one legally
   load-bearing requirement.
3. **State clearly that BHSA is downloaded separately, not bundled** —
   already true of this repo's structure, just say so explicitly so
   nobody assumes otherwise.
4. **Don't monetize this repository or any product built directly on
   it** without contacting the German Bible Society first, per the
   NonCommercial clause. A free, public, scholarly GitHub repo is
   squarely within the license's intent — this note is only relevant
   if you ever considered a paid product, app, or service built on it.
5. **Keep the interpretive claims clearly separated and labeled as
   your own scholarship**, not ETCBC's. This is already true of the
   architecture (claims live in `claims_registry`, entirely separate
   from BHSA's grammatical features) — worth saying so explicitly in
   the README so a reader never mistakes a YLH interpretive claim for
   an ETCBC grammatical fact.

None of this is a reason not to publish — CC BY-NC 4.0 is one of the
more permissive research-data licenses in the field, and what you've
built (a documented, cited, clearly-separated extension) is exactly
the kind of use it's designed to support.

## Repository contents

- `ylh_v5_build.py` — the build script. Downloads BHSA via Text-Fabric,
  verifies target word nodes live against the corpus, constructs the
  `ylh_textual_grouping` node type via `tf.dataset.modify()`, and
  writes the result to a local Text-Fabric-loadable module.
- `claims_registry.py` — the external scholarly claims database:
  sources, evidence types, counterarguments, and categorical
  confidence/inference-distance ratings for each claim, kept separate
  from the corpus-level features per the YLH architecture.
- `ARCHITECTURE.md` *(recommended addition)* — a short doc explaining
  the three-tier ID system (corpus node IDs / YLH analytical-object
  IDs / YLH claim IDs) and why claims are stored externally rather
  than as long-text Text-Fabric features.

## Requirements

- Python 3.10 or later (text-fabric's dependency chain requires
  callable `staticmethod` behavior only available from 3.10+)
- `pip install text-fabric`
- ~200MB free disk space for the cached BHSA corpus (downloads
  automatically on first run)

## Citation

If you use or extend this work, please cite both:

- **ETCBC/BHSA** (the underlying corpus): DOI `10.17026/dans-z6y-skyh`
- **This YLH extension** (once published): [your GitHub URL + a
  citation format of your choosing — a `CITATION.cff` file is the
  GitHub-native way to make this machine-readable]
