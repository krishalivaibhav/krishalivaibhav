## Vaibhav Krishali

I find out why AI systems and graphics code are slow, wrong or expensive. I measure first, then fix, then prove the fix with numbers.

**Available for:** LLM cost and quality audits, retrieval (RAG) evaluation, and performance investigations.

`Python` · `C++` · `FastAPI` · `RAG and evals` · `Anthropic API` · `local LLMs` · `Vulkan / Metal` · `SPIR-V`

---

### Case studies

**[What 22 days of coding-agent calls would cost](case-studies/llm-cost-teardown.md)**: $566 at API prices, $6,301 without prompt caching.
Run through [llm-cost-audit](https://github.com/krishalivaibhav/llm-cost-audit), my open-source cost audit CLI, which the real data caught undercounting by 12%.

**[From 11 FPS to 60 on a fanless M2](case-studies/balatro-m2.md)**: a PS5 port was spending 64 ms a frame on work nobody could see.
Three root causes, one wrong theory killed by measurement, and a fix that proves it is safe before it runs.

**[RAG over technical PDFs, fully on-premise](https://github.com/krishalivaibhav/local-multimodal-rag)**: answers that live in tables and figures, cited back to page and bounding box.
Nothing leaves the network. [Read the case study](case-studies/local-rag.md).

**[A KYC auditor that raises fewer alerts](https://github.com/krishalivaibhav/ckyc-auditor)**: five agents screening a bank's customer book against sanctions lists and adverse media.
The product is the alerts it refuses to raise. Built for Code by Tech Mahindra (top 6 of about 30,000 registrants). [Read the case study](case-studies/ckyc-auditor.md).

---

### Open source

**[AnyPS5](https://github.com/boykopovar/AnyPS5)** (PS5-to-PC porting tool, 53k stars): [20 merged pull requests](https://github.com/boykopovar/AnyPS5/pulls?q=is%3Apr+author%3Akrishalivaibhav+is%3Amerged), mostly bringing the runtime up on macOS and Apple Silicon and fixing the graphics driver.

### Also

[Track+](https://tracker-v2-nine.vercel.app) (DSA tracker, live) · [Gesture](https://github.com/krishalivaibhav/Gesture) (hand tracking) · [Resume-Analyzer](https://github.com/krishalivaibhav/Resume-Analyzer) (resume and job matching)

---

[Email](mailto:krishalivaibhav@gmail.com) · [X](https://x.com/getaway_kr) · [LinkedIn](https://linkedin.com/in/vaibhav-krishali)
