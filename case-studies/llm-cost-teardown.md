# What 22 days of coding-agent calls would cost, and two bugs the real data found in my audit tool

**Data:** 7,407 Claude API calls from my own Claude Code sessions, 21 October to 11 November 2026.
**Tool:** [llm-cost-audit](https://github.com/krishalivaibhav/llm-cost-audit), an open-source CLI I wrote that reads LLM call logs and ranks possible savings.
**Result:** $566 at list API prices. Without prompt caching the same calls would have cost $6,301. The run also exposed two bugs in the tool that synthetic test data never hit.

I use a subscription, so $566 is what these calls would cost at API prices, not what I paid. Only token counts, model names, timestamps and project folder names were read; no prompt text.

## 1. Caching is doing almost all the work

A coding agent re-sends its whole conversation on every turn. The median call read 191,278 input tokens to write 358 output tokens.

98.3% of those input tokens (1.69 billion) came from the prompt cache, billed at 5% of the normal input price on the models I mostly used. Priced at the full input rate, the same 22 days come to $6,301. Caching cut the bill by 91%.

## 2. Where the remaining money goes

| Token type | Cost | Share of spend |
|---|---:|---:|
| Cache reads | $303.82 | 54% |
| Cache writes | $190.53 | 34% |
| Output | $71.43 | 13% |
| Full-rate input | $0.06 | 0% |

Cache writes are 1.7% of input tokens but 34% of spend. 92% of them are 1-hour writes, which cost 2x the input price, against 1.25x for the default 5-minute writes. Whether 5-minute writes would be cheaper here depends on how often calls are more than five minutes apart; I have not measured that yet.

## 3. Bug one: the tool undercounted spend by 12%

The tool priced every cache write at the 5-minute rate. On this log it reported $497.59 instead of $565.85. Any team using 1-hour caching would have got the same undercount.

The fix reads the 1-hour count Anthropic reports in each response (`usage.cache_creation.ephemeral_1h_input_tokens`) and prices it separately. If a model has no 1-hour price, the report now says so instead of quietly undercounting.

## 4. Bug two: a placeholder guess outranked a measured finding

Six of the seven findings were "extreme input:output ratio", each with a saving based on a fixed assumption that 25% of the input could be cut. They ranked above the one finding computed from the log.

For an agent, a huge input and a short answer is normal. The input is the conversation history, and 95 to 98% of it was already cached. The tool now reports this pattern as "long reused context", where the lever is compacting history, not trimming a prompt. Any finding whose saving is a placeholder is marked as illustrative and ranks after every measured one.

## What I would check first on a team's agent

1. Is caching on, and is it hitting? Here it was worth 91%.
2. Which cache lifetime are you paying for? The write price differs by 60% between the two.
3. Once caching works, cost grows with context length times the number of turns. Compaction and shorter sessions are the next lever.
4. Run any cost tool, including mine, on your real logs before trusting its numbers.

## Limits

- One user, one product, 22 days. Agent cost patterns differ from chat or batch workloads.
- Dollar figures use list prices checked on 11 November 2026, standard tier, no batch or regional pricing.
- Calls were deduplicated by message ID from local transcript files; calls missing from those files are not counted.
- Shares are rounded, so they do not sum to exactly 100%.
