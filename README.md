# CoalMix Studio

DAW open source (Static Forge) que replica el flujo de trabajo de FL Studio 21 —
código 100% propio, reimplementación clean-room — con DSP multihilo real, UI en
GPU (ImGui/Vulkan) y compatibilidad con proyectos `.flp` y plugins VST3/CLAP.

**Antes de tocar nada:** lee `AGENTS.md` (reglas clean-room, no negociables) y el
Documento Maestro del proyecto en `V:\ObsidianVault\14-Iron3D-CoalMix\Iron3D_CoalMix_Documento_Maestro_2026-10-27.pdf`
(resumen en `estado.md` de esa misma carpeta).

Estado: fase de diseño (2026-10-27). Sin código todavía. Stack de audio/plugins/MIDI:
**JUCE** (decisión de Core, 2026-10-27) → licencia del proyecto **AGPLv3**. UI en
ImGui/Vulkan de todos modos, JUCE solo para audio — ver AGENTS.md §5 y §7.

## Estructura

```
src/
  core/             Motor de audio, DSP multihilo, gestor de buffers
  audio_drivers/    ASIO, WASAPI, PipeWire/JACK, ALSA
  plugins_host/     VST3 + CLAP en procesos aislados
  instruments/      CoalSynth3X, CoalWave, CoalSampler
  fx/               EQ paramétrico, compresor, reverb, delay
  ui/               Piano Roll, Channel Rack, Playlist, Mixer (ImGui + GPU)
  flp_importer/     Parser de .flp (referencia: PyFLP)
docs/formats/       flp.ksy (Kaitai Struct)
tests/oracle/        Exports MIDI/WAV de FL Studio 21 como referencia
assets/              Presets, skins e iconos propios
```

## MVP

Motor de audio con JUCE/ASIO, Channel Rack, Piano Roll básico, CoalSynth 3X y
exportación a WAV.

## Licencia

**AGPLv3** (JUCE la impone). Ver AGENTS.md §7.
