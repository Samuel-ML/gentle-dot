# Wake word audio: find the root cause before any more patches

Six fixes went in one symptom at a time (deadline, stall, resume_skipped, device gate, capture guard,
empty transcript). Each was real and tested, and the thing still fails: with a Bluetooth headset the
microphone ends up off, toggling the switch does nothing, and captures appear live while delivering
nothing. This task is an audit, not another patch: the design is under suspicion, not just its bugs.

## Specs

- S1 — No code changes until the audit names the root cause with `path:line` evidence and the user
  approves a plan.
- S2 — The audit answers, for this machine: why a capture can be open (orange dot on) and deliver no
  frames; whether the app can observe that today; and which of the six patches are symptoms of the
  same defect.
- S3 — The audit says whether a single long-lived audio engine with a swappable sink is the right
  architecture, and what it costs.
- S4 — The audit lists every actor that can open or close the microphone (guard, deadline, stall,
  retry ladder, panel, machine) and whether their orders can contradict each other.
- S5 — The audit lists every state the panel can be left in that the backend cannot correct.

## Tasks

| ID | Specs | Route | Commit |
|----|-------|-------|--------|
| T1 | S2, S3 | `gentle-ai-explore`: audio backend and device lifecycle (macos.rs, core_audio.rs, machine.rs open/close/arm) | — |
| T2 | S2, S4 | `gentle-ai-explore`: state machine and actors that touch the microphone (machine.rs, app.rs woken path, guard/deadline/stall/retry) | — |
| T3 | S5 | `gentle-ai-explore`: panel contract (voice-native.ts, useVoice.ts, SettingsPanel) | — |
| T4 | S1, S2 | Synthesize the three reports into one root cause, one plan, and ask before writing code | — |

## Log

- L1 — "espera, estás implementando por implementar, solo buscando salidas al primer error que te
  diga, tienes que hacer una exploración de la solución completa usando agentes y recién buscar cuál
  puede ser el problema de fondo, si te ayuda usar odd más tasks úsalo"
