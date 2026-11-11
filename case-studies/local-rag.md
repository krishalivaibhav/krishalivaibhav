# Question answering over technical PDFs, with nothing leaving the machine

**Project:** [local-multimodal-rag](https://github.com/krishalivaibhav/local-multimodal-rag), a retrieval system for technical documents where the answers sit in tables and figures.
**Constraint:** fully on-premise. Open-weights models only, running locally.
**Result:** reranking raised multi-document Hit@5 from 0.73 to 0.91 and citation accuracy from 0.55 to 0.82 on my multi-document evaluation set.

## The problem

Technical standards keep their answers in tables, figures and formulas. Standard retrieval treats a PDF as a stream of prose, so a question like "what is the limit for condition X?" finds the paragraph around the table and misses the row that holds the number.

The target use is documents that cannot be sent to a third party, so no cloud API is allowed.

## What I built

**Each content type gets its own path.**
- Prose is extracted directly.
- Tables are split one chunk per row, with the column headers attached to every row, so a single row can be retrieved and still makes sense alone.
- Figures are described by a small local vision model.

**Retrieval in layers.** Dense embeddings and keyword search (BM25) are fused, then a cross-encoder reranks the candidates. A deterministic router picks the right document, and duplicate passages from different revisions of the same standard are collapsed.

**Answers that can be checked.** Every answer cites the page, table row or figure it came from. Formulas are evaluated by a symbolic maths library, not by the language model. When the documents do not contain the answer, the system says so.

## How I measured it

I built the evaluation before tuning anything.

- Questions were written from the documents, not from the retrieved chunks. That one change dropped the measured score from 0.92 to 0.80, which is the honest number.
- Finding the right value and citing the right source are scored separately.
- Ideas that did not help are kept in the results as a table of rejected approaches.

## Limits

- The evaluation documents are not in the repository, so a visitor cannot reproduce the numbers.
- The test sets are small.
- It runs on small local models (7B and 4B). Citation accuracy on the hardest set is 0.65, and deeply nested tables are still unsolved.
- The reranker adds about 3 seconds per query on an M2 CPU.
