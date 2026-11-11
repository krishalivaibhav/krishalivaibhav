# From 11 FPS to 60: finding 64 ms of hidden GPU work in a PS5 port

**Project:** [AnyPS5](https://github.com/boykopovar/AnyPS5), an open-source tool that ports PS5 games to PC.
**Target:** Balatro at a constant 60 FPS on a fanless MacBook Air M2, 1920x1080, through MoltenVK.
**Result:** the worst scene went from about 11 FPS (77.5 ms of GPU time per frame) to 60 FPS (10 to 11 ms), held for a five-minute play test.

## The problem

Balatro is a 2D card game. On the M2 it ran at 60 FPS for about two minutes, then fell to around 40. Opening a booster pack dropped it to 11.

Three different problems were hiding behind one symptom, and the first two guesses were wrong.

## 1. The slowdown after two minutes was heat, not code

`powermetrics` showed the GPU clock falling from 1365 MHz to 838 MHz while the work per frame stayed flat. The machine has no fan. No setting could fix that; the only fix was doing roughly 40% less GPU work per frame so the chip stayed cool.

## 2. A wrong theory, and the measurement that killed it

Every full-screen pass cost about 3.7 ms, even when nearly empty. That looked like a fixed per-pass cost in the Vulkan-to-Metal layer.

I added a profiling switch that times each shader in its own pass. The "empty" pass turned out to cost 0.6 ms. The other 7 ms belonged to one procedural background shader.

The shader was slow because the recompiler reproduces the PS5 GPU's exact floating-point behaviour on every operation: NaN propagation, denormal flushing, and sine and cosine built from polynomial series. An opt-in fast-math mode cut the menu frame by 22%, with no visible difference.

## 3. The 11 FPS scene: 730 draws, each restarting the renderer

On the pack screen the cost was per draw, not per pixel. Shrinking every sprite to a single pixel changed nothing. Skipping the draw calls tripled the frame rate.

Every sprite was being drawn through a tessellation pipeline. The port used it to emulate a PS5 interpolation feature, and MoltenVK runs tessellation as a separate compute step, which stops and restarts the Metal render pass for each draw. At 730 draws a frame, that was 50 ms.

The emulation was only triggered by a leftover value that fed pixels the GPU then discards. I wrote a check that proves this, shader by shader, before switching to the fast path. Four of six shaders passed the proof; the other two keep the slow path.

## Results

| Pack screen | FPS | GPU time per frame |
|---|---|---|
| Before | ~11 | 77.5 ms |
| Interpolation fix | ~24 | 35.1 ms |
| Plus fast float and trig | 60 | 10 to 11 ms |

A five-minute run with full tracing held 55.6 to 60.2 FPS in all 59 five-second windows, with no upward drift in GPU time through the period where it used to throttle.

## What happened to the fix

Another contributor had an open pull request that solved the same interpolation fallback more generally and measured slightly better (p99 frame time 18.4 ms against my 23.9 ms). I did not open a competing PR. I tested theirs on the M2 and posted the comparison data on it.

## How I worked

I ran this investigation with AI coding agents under my direction: I set the goal, chose what to measure, and checked each claim against a fresh run before believing it. Every number above comes from a recorded trace.
