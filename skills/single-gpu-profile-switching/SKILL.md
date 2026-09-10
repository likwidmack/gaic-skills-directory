---
name: single-gpu-profile-switching
description: Use when a Docker Compose project runs multiple GPU-hungry services (LLM inference, image generation, etc.) on a host with exactly one GPU, and you need to start/stop them without VRAM contention or silent OOM crashes.
---

# Single-GPU profile switching

## The problem

Docker's NVIDIA runtime happily lets N containers all request the same GPU at
once. Nothing in Docker itself serializes access. On a workstation with one
GPU and several optional services — a chat model, an image generator, a
speech model — starting a second GPU-heavy container while the first is still
warm doesn't fail loudly. It either OOMs mid-inference, or both processes
silently share VRAM and both get slower and less reliable. The fix is not a
runtime flag; it's making "only one GPU consumer at a time" an explicit,
checked policy in your own tooling.

## The pattern

1. **Declare which services are GPU-exclusive, and which of those are
   simultaneously required by which "profile" (mode) of your stack.**
   Represent this as data, not as scattered `if` statements — a
   `{ services: [...], profilesByService: { service: [profiles] } }` map
   works well and is easy to unit test.

2. **Before starting a profile, compute a stop/keep plan, don't just start.**
   Given "GPU services currently running" and "profile about to start",
   derive two sets: services to stop (currently running, not needed by the
   target profile) and services to keep/start (needed by the target
   profile). Stop-then-start, in that order, never the reverse — starting
   before stopping is exactly the race that causes contention.

3. **Preflight-check on every start, not just on explicit "switch".**
   A user (or an agent) will eventually run the "start this one service"
   command directly instead of the "switch to this profile" command. Both
   paths need to consult the same conflict check before starting a
   container — one that throws on "you're about to start service B while
   GPU-exclusive service A is already running", with an explicit
   `--allow-gpu-share`-style opt-out for when contention is actually
   intended (a developer debugging both at once, a GPU with genuinely
   spare VRAM for two small models).

4. **Give an escape hatch for CPU-only hosts and for hosts where the whole
   policy doesn't apply.** Auto-detect compute mode where possible (probe
   for the GPU vendor's CLI tool — `nvidia-smi`, for example — and fall back
   to CPU on failure or absence), and let it be pinned explicitly. On CPU
   hosts, GPU-exclusivity checks are moot; skip them rather than special-
   casing every call site.

5. **Make the check pure and unit-testable.** The "which services must stop,
   which must stay" computation is pure logic — no I/O — and should live in
   its own function taking `(runningServices, targetProfile, exclusivityMap)`
   and returning `{ toStop, toKeep }`. Keep the actual `docker compose stop`
   /`docker compose up` calls in a thin wrapper around it. This is the
   difference between a policy you can unit-test in milliseconds and one you
   can only verify by actually starting containers.

## Signals this skill applies

- The user mentions running multiple AI/ML services (LLM server, image
  generation, TTS/STT, etc.) via Docker Compose on one machine
- Symptoms described: CUDA out-of-memory errors that only happen when
  "switching" between two services, or GPU services silently getting slower
  when more than one is up
- A request to add a new GPU-heavy service to an existing Compose stack that
  already has one

## What this skill does NOT cover

- Multi-GPU scheduling or spreading models across GPUs — that's a different
  problem (device selection, not exclusivity)
- NVIDIA MPS or time-slicing setups on shared/multi-tenant Linux hosts
- Kubernetes GPU scheduling (its device-plugin model already solves this at
  the orchestrator level)
