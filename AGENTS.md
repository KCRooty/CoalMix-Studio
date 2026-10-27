# AGENTS.md — CoalMix Studio

Reglas obligatorias para cualquier agente (Claude Code, etc.) o colaborador humano.
CoalMix Studio es un DAW open source de Static Forge que replica el flujo de trabajo
de FL Studio 21 con código propio, y lo mejora.

## 1. Clean-room (no negociable)

- Prohibido descompilar, desensamblar o depurar binarios de FL Studio o de sus plugins nativos.
- Prohibido copiar código, samples, presets, skins, iconos o sonidos de fábrica de Image-Line.
- Prohibido usar "FL Studio", "Image-Line", "Sytrus", "3xOSC", "Harmor", etc. como nombres del producto.
  En docs, solo descriptivo ("abre proyectos .flp").
- Permitido como referencia:
  - Uso del programa, manual público, grabaciones de pantalla.
  - Observación entrada → salida (oráculo, ver §4).
  - Formato .flp: documentación pública y PyFLP como referencia; el parser propio se
    documenta en docs/formats/flp.ksy (Kaitai Struct).
- Si algo solo se puede resolver mirando el binario: parar y avisar.

## 2. Qué se replica 1:1 y qué se mejora

| Replicar 1:1 | Mejorar libremente |
|---|---|
| Channel Rack, Piano Roll, Playlist por patrones, Mixer | DSP multihilo real por canal |
| Atajos de teclado y gestos del Piano Roll | UI en GPU (ImGui) sin robar CPU al audio |
| Enlace automático canal → pista del mixer | Plugins aislados en procesos separados |
| Abrir .flp con patrones, notas y estructura del Playlist | Instrumentos y efectos propios |

## 3. Instrumentos y contenido propio

CoalSynth 3X, CoalWave y CoalSampler se implementan con técnicas DSP públicas (PolyBLEP, SVF,
wavetables, FM). Samples y presets: creados por Static Forge o con licencia libre, listados en ASSETS.md.

## 4. Oráculo de comportamiento

Core tiene FL Studio legal. Se usa solo como caja negra: proyecto de prueba → export WAV/MIDI en
FL → tests/oracle/<caso>/expected.* → comparar con el export de CoalMix (MIDI exacto, audio por
tolerancia/espectro).

## 5. Stack

C++20/23 · CMake · audio: RtAudio (ASIO / WASAPI / PipeWire-JACK / ALSA) · plugins: VST3 + CLAP
(sin VST2) · UI: Dear ImGui sobre Vulkan · threading con primitivas estándar de C++20.
Si se usa JUCE: AGPLv3, y condiciona la licencia del proyecto (decidir antes — ver estado.md del
proyecto en el vault para la decisión tomada).
Windows y Linux con paridad; CI falla si uno de los dos no compila.

## 6. Calidad

- El hilo de audio nunca reserva memoria, bloquea ni hace I/O.
- Un plugin que crashea no tumba el DAW.
- Formato de proyecto versionado desde el primer commit.
- Cero telemetría. Sin anuncios. Sin paywalls.

## 7. Licencia

MIT OR Apache-2.0 si se evita JUCE; AGPLv3 (o comercial) si se usa JUCE — decidir antes del primer
commit, ver estado.md del proyecto en el vault.
