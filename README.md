# Explainable Quran and Hadith Retrieval

A bilingual Arabic and English retrieval system for finding relevant Quran
verses and Hadith records with source references and short match explanations.

The project was developed by a university team. Mohammed Yousef Rasheed focused
on Arabic normalization, the hybrid retrieval layer, and the evaluation harness.

## The Problem

Arabic religious text retrieval cannot rely on literal word overlap alone.
Diacritics, letter variants, morphology, translation differences, and short
conceptual queries can separate a useful result from the exact wording entered
by a user.

The system combines lexical and semantic signals, then returns references and
match explanations so relevance can be inspected rather than accepted blindly.

## Verified Corpus

The bundled loader returns **40,098 records**:

| Source | Records |
| --- | ---: |
| Quran | 6,236 |
| Hadith | 33,862 |

The Hadith corpus covers Bukhari, Muslim, an Nasai, Abu Dawud, at Tirmidhi, and
Ibn Majah.

## Retrieval Pipeline

```text
Arabic or English query
        ↓
Normalization and language aware preprocessing
        ↓
Dense embeddings and TF IDF character matching
        ↓
FAISS candidates and metadata filters
        ↓
Hybrid ranking
        ↓
Source reference and match explanation
```

The design combines multilingual Sentence Transformer embeddings with character
level TF IDF so Arabic spelling and morphology do not depend on one retrieval
signal.

## Engineering Decisions

**Hybrid retrieval**

Dense embeddings capture conceptual similarity while character level TF IDF
retains sensitivity to names, phrases, and Arabic orthographic variation.

**Source aware records**

Every result carries a stable identifier, source type, collection metadata,
language fields, and a reference. Retrieval relevance is kept separate from
religious authenticity metadata.

**Reproducible indexing**

Dataset fingerprints and index metadata determine whether cached FAISS artifacts
can be reused or must be rebuilt.

**Explainability at the result level**

The interface describes why a result matched and preserves the source reference
needed for human review.

## Evaluation

The repository includes an evaluation harness for:

* Precision at 5
* Recall at 5
* F1 at 5
* Mean Reciprocal Rank
* nDCG at 5
* average query latency

The included gold file is a starter benchmark. It should be expanded with
qualified human judgments before the project makes strong retrieval quality
claims.

## Verified Baseline

The private source baseline was checked without modification on 29 July 2026:

* 10 dependency independent tests passed
* Python compilation passed
* the loader returned 40,098 records

The complete test suite requires FAISS. FAISS was not installed in the verification
environment, so full retrieval execution remains to be confirmed there.

## Source Availability

The complete team source and bundled corpus are maintained in a private
development repository while dataset attribution, local path cleanup, and the
full FAISS evaluation path are reviewed. This public repository presents the
system design and verified evidence without exposing the working repository.

## Current Limitations

* the starter relevance benchmark is too small for strong quality claims
* scholarly review is needed for a production grade relevance set
* first time embedding setup may require model downloads
* retrieval relevance must never be presented as religious authority
* latency and memory behavior still need measurement on the full FAISS index

## Next Technical Milestones

* expand the benchmark with Arabic and English query families
* add hard negatives and morphology focused failure cases
* report per source and per language retrieval quality
* compare lexical, dense, and hybrid methods under one frozen protocol
* add citation level feedback without modifying the source texts

## Scope

This is an information retrieval project, not a source of religious rulings.
Results should always preserve their references, text, grading metadata, and
human review context.
