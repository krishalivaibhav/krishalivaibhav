## Vaibhav Krishali

I build systems that sit underneath other software — audio pipelines, retrieval
engines, proxies, memory layers. Mostly local-first, mostly the parts that are
hard to get right rather than the parts that demo well.

Currently interested in: on-device inference, personal memory for AI tools, and
real-time signal processing.

---

### Selected work

**[autoduck](https://github.com/krishalivaibhav/autoduck)** · Python, DSP
Automatically ducks music when someone starts talking nearby. The naive version
fails because the microphone mostly hears the music, not the person — so this
taps the playback signal through a loopback device as a reference, runs adaptive
acoustic echo cancellation to subtract the room's response, and feeds the clean
residual to a Silero VAD. **32.8 dB measured ERLE** on real hardware.

**[local-multimodal-rag](https://github.com/krishalivaibhav/local-multimodal-rag)** · Python, RAG
Retrieval-augmented QA over technical PDFs where the answers live in tables and
figures, not prose. Each element type is routed to a dedicated extractor,
tables are chunked **per row** so individual values stay queryable, and every
chunk carries page and bounding-box provenance so answers can cite back to the
document. Runs entirely on open weights, on-premise.

**[ckyc-auditor](https://github.com/krishalivaibhav/ckyc-auditor)** · Python, Flutter, multi-agent
Continuous KYC auditing for banks — five cooperating agents watching a customer
book against sanctions lists and adverse media. The thesis: *the product isn't
the alerts you raise, it's the ones you refuse to raise.* Name collisions get
resolved on hard identifiers instead of drowning compliance teams in false
positives. Built for Code by Tech Mahindra, Challenge 3.

**[Track+](https://tracker-v2-nine.vercel.app)** · React, Vercel, Groq · [source](https://github.com/krishalivaibhav/tracker-v2)
DSA and placement tracker that's actually deployed and used — Striver A2Z
progress, an AI resume scanner, and a job application pipeline.

**Memoir** *(in progress, private)*
A local-first memory layer that sits under any AI tool. Intercepts
conversations, extracts durable facts, stores them as plain Markdown the user
owns and can edit, and injects the relevant ones into every request regardless
of model or client. The engine depends on nothing external; the proxy and hooks
are disposable adapters over it.

---

### Also here

[Gesture](https://github.com/krishalivaibhav/Gesture) — real-time two-hand
tracking with OpenCV + MediaPipe, with game and sign-language practice modes ·
[Resume-Analyzer](https://github.com/krishalivaibhav/Resume-Analyzer) — FastAPI
resume/JD matcher with Overleaf-ready LaTeX output

---

### Tools

`Python` `JavaScript` `React` `FastAPI` `Flutter` `SQLite` `Docker`
`ChromaDB` `MediaPipe` `ONNX` `local LLMs`
