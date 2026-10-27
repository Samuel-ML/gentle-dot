# Voice input: devices appear while the app is open

The panel lists microphones once, when it mounts, and the app enumerates them when it is asked.
Plugging a headset in, or pairing one, does nothing until Gentle Dot is quit and reopened — the
"Restart assistant" item does not help either. The list has to follow the system, not a snapshot.

## Specs

- S1 — "yo creo que la detección de dispositivos para el input voice debe hacerse en tiempo real,
  porque tengo que cerrar gentle-dot y volver a abrir para ver selecciona mi audífino, ni siquiera
  sirve la opción de 'Restart assistant' para que pueda detectar mi dispositivo": connecting,
  disconnecting or switching a microphone updates the panel's list without restarting anything.
- S2 — "la solución debería ser tener un canal de escucha sobre los input voices": the app listens to
  CoreAudio's device notifications instead of polling, and pushes the new list to the panel.
- S3 — A device that disappears while it is the chosen input is reported: the list keeps the choice
  visible as missing and the app does not silently record from something else.
- S4 — The listener is closed when the app quits, and a failure to install it is logged with its
  reason instead of leaving a silent stale list.

## Tasks

| ID | Specs | Route | Commit |
|----|-------|-------|--------|
| T1 | S1, S2 | Explore: how devices are enumerated today, which files and commands are involved, and what a CoreAudio property listener would cost | — |
| T2 | S1, S2 | The listener: `core_audio` watches `kAudioHardwarePropertyDevices` and the default input, the app pushes a `voice://devices` event, the panel re-renders | — |
| T3 | S3, S4 | A vanished chosen device reads as missing; the listener is removed on shutdown and its failure is logged | — |

## Log

- L1 — "otra cosa más al plan, yo creo que la detección de dispositivos para el input voice debe
  hacerse en tiempo real, porque tengo que cerrar gentle-dot y volver a abrir para ver selecciona mi
  audífino, ni siquiera sirve la opción de 'Restart assistant' para que pueda detectar mi dispositivo,
  por eso creo que la solución debería ser tener un canal de escucha sobre los input voices, has una
  revisión con un agente para que valide cuál es la implementación actual de eso, qué archivos están
  implicados, puedes usar odd para toda la implementación más el todo y sus tasks"
