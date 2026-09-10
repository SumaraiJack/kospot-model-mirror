# NOTICE — third-party models redistributed here

Nothing in this repository was written by KosPot. Every file is an unmodified,
byte-for-byte copy of a model published by someone else, renamed so its purpose
in the app is obvious. This file exists because three of the four licences
**require** attribution when the file is redistributed, which is exactly what
this repository does.

Licences were read from the archives themselves on 2026-09-10, not assumed.

---

## 1. Aura's natural voice — Kokoro

`kospot-aura-voice-natural-kokoro-int8-en-v0_19.tar.bz2`

- **Model:** Kokoro-82M, int8, English v0.19
- **Author:** hexgrad — https://huggingface.co/hexgrad/Kokoro-82M
- **Licence:** Apache License 2.0 (the full text ships inside the archive as
  `kokoro-int8-en-v0_19/LICENSE`)
- **Packaged by:** k2-fsa/sherpa-onnx

Apache 2.0 permits redistribution, with or without modification, provided the
licence and this attribution travel with it. Both do.

---

## 2. Aura's light voice — Piper (southern_english_female, medium)

`kospot-aura-voice-light-vits-piper-en_GB-southern_english_female-medium-int8.tar.bz2`

- **Model:** Piper VITS, en_GB southern_english_female, medium, int8
- **Project:** Piper — https://github.com/rhasspy/piper
- **Training data:** OpenSLR 83 — "Crowdsourced high-quality UK and Ireland
  English Dialect speech data set" — http://www.openslr.org/83/
- **Licence:** **CC BY-SA 4.0** —
  https://creativecommons.org/licenses/by-sa/4.0/
- **Packaged by:** k2-fsa/sherpa-onnx

### Read this one properly

This is the only file here that is not a permissive software licence, and it is
the only one with a condition that can bite later.

- **BY (attribution).** Redistributing it requires crediting the source. That is
  what this section is.
- **SA (share-alike).** Anything ADAPTED from it must be released under CC BY-SA
  4.0 as well. This repository does not adapt it — the file is an exact copy —
  so ShareAlike is not triggered by the mirroring.

The archive itself ships **no licence file**, which is why the terms had to be
traced back through the Piper voice index to the dataset. The `medium` build is
not in the official `piper-voices` index either; only `low` is. The model's own
metadata names its dataset as `ubuntu`, which is OpenSLR 83, so the same
CC BY-SA 4.0 terms apply.

If KosPot ever ships generated audio from this voice as a product in its own
right — not speech played to the user who asked for it, but recorded clips
distributed on their own — get that looked at properly. Whether synthesised
output counts as an adaptation of a CC BY-SA dataset is unsettled, and this
note is a flag, not an answer.

---

## 3. Aura's ears — Moonshine tiny

`kospot-aura-ears-asr-sherpa-onnx-moonshine-tiny-en-int8.tar.bz2`

- **Model:** Moonshine tiny, English, int8
- **Author:** Useful Sensors — https://github.com/usefulsensors/moonshine
- **Licence:** MIT (Copyright (c) 2024 Useful Sensors — full text ships inside
  the archive as `sherpa-onnx-moonshine-tiny-en-int8/LICENSE`)
- **Packaged by:** k2-fsa/sherpa-onnx

MIT permits redistribution provided the copyright notice and permission notice
travel with it. Both are in the archive.

---

## 4. Voice activity detection — Silero VAD

`kospot-aura-ears-vad-silero_vad.onnx`

- **Model:** Silero VAD
- **Author:** Silero Team — https://github.com/snakers4/silero-vad
- **Licence:** MIT (Copyright (c) 2020-present Silero Team)
- **Packaged by:** k2-fsa/sherpa-onnx

This one is a bare `.onnx` with no licence file alongside it, so the notice is
reproduced here instead. The terms were read from the project's own `LICENSE`
on 2026-09-10.

---

## Packaging

All four were converted to ONNX and published by the
[k2-fsa/sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) project, which is
Apache 2.0. KosPot uses the sherpa-onnx runtime as well.

## If a model is ever swapped

Re-read the new archive's `LICENSE`, and if it has none, trace it to the
upstream project before publishing it here. A model with a non-commercial or
no-redistribution clause cannot go in a public mirror at all.
