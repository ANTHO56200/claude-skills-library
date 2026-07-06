---
name: rag-designer
description: Design a retrieval-augmented generation (RAG) system that actually returns grounded, correct answers — chunking, embeddings, retrieval, generation, and evaluation. Use when building RAG/search-over-docs, when a RAG system gives wrong or ungrounded answers, or asked "design a RAG pipeline", "why is my RAG bad", "chat over my documents". A comprehensive, end-to-end skill.
---

# RAG Designer

Most RAG systems fail not at the LLM but at retrieval: if the right chunk isn't retrieved, no
prompt saves the answer. Design each stage deliberately, and measure retrieval separately from generation.

## 1 — Ingestion & chunking (where most quality is won or lost)

- **Chunk by meaning, not by fixed character count.** Split on structure (headings, sections,
  paragraphs, code blocks) so a chunk is a coherent unit. A chunk cut mid-sentence retrieves poorly.
- **Right size**: big enough to hold a complete thought, small enough to be specific. Overlap
  chunks slightly so a fact spanning a boundary isn't lost.
- **Attach metadata**: source, section, title, date, permissions — you'll filter and cite with it.
- **Preserve structure**: tables, lists, code often need special handling; flattening them to prose loses meaning.

## 2 — Embedding & indexing

- Pick an embedding model suited to the domain and language; be consistent (query and docs
  embedded by the SAME model).
- Store vectors + the metadata + the original text. You return the text, not the vector.
- Consider hybrid: dense (semantic) + sparse/keyword (BM25) — pure vector search misses exact
  terms (names, codes, error strings); keyword catches those. Hybrid beats either alone on most corpora.

## 3 — Retrieval (the make-or-break stage)

- Retrieve top-k, then **re-rank** (a cross-encoder or the LLM) — first-pass vector recall is
  broad; re-ranking gets the truly relevant to the top.
- **Filter by metadata** (recency, source, the user's permissions — never retrieve a doc the
  user can't see; RAG is an access-control surface).
- Tune k: too few misses context, too many buries the answer in noise and burns tokens.
- If retrieval quality is bad, the whole system is bad — fix here before touching the prompt.

## 4 — Generation (grounded, honest)

- Instruct the model to answer ONLY from the retrieved context, and to say "I don't know / not
  in the provided documents" when the answer isn't there. This is the anti-hallucination core.
- **Cite sources**: have it point to which chunk/document each claim came from — grounding you can verify.
- Pass enough context, in a clear structure, with the question. Don't dump 20 chunks; pass the re-ranked best.

## 5 — Evaluate (separately, or you're flying blind)

- **Retrieval metrics**: for a set of test questions with known relevant docs, measure hit
  rate / recall@k. If the right doc isn't retrieved, generation can't recover — this is where to debug first.
- **Generation metrics**: faithfulness (is the answer grounded in the retrieved context, no
  invention?), answer relevance, and does it correctly say "don't know" when it should?
- Build a small eval set of real questions early; iterate against it, not against one demo query.

## Rules

- Diagnose retrieval before generation — a wrong answer is usually a retrieval miss, not a prompt problem.
- Ground and cite; the model must refuse to answer beyond the retrieved context.
- Respect permissions in retrieval — RAG can leak documents a user shouldn't see.
- Evaluate retrieval and generation separately, on a real question set, or you can't improve anything.

## Output

The pipeline design stage by stage (chunking strategy, embedding + index choice, retrieval +
re-rank + filtering, generation prompt, eval plan), the key decisions with reasons, and — if
debugging an existing RAG — which stage is failing (with how you'd confirm) and the fix.
