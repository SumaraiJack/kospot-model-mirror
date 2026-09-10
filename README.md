# kospot-model-mirror

A private copy of the four on-device AI models the KosPot app downloads for Aura.

This repo exists for one reason: **insurance.** The app fetches these files from
someone else's GitHub releases. If that project ever deletes a release, renames a
tag, or disappears, every KosPot user loses the ability to install Aura's voice
and ears. These copies mean the app can be repointed in an afternoon instead of
losing the feature.

Nothing here is modified. Every file is a byte-for-byte copy of the upstream
release, renamed so it is obvious what it is for.

---

## The files

All four live on the **`models-v1` release**, not in git. GitHub refuses any
git-tracked file over 100 MiB and one of these is 102.6 MiB; release assets allow
up to 2 GB, which is also exactly how the originals are published.

| File | What it is for | Size |
| --- | --- | --- |
| `kospot-aura-voice-natural-kokoro-int8-en-v0_19.tar.bz2` | Aura's best voice. Neural TTS, runs on the phone. | 98.5 MiB |
| `kospot-aura-voice-light-vits-piper-en_GB-southern_english_female-medium-int8.tar.bz2` | Aura's small voice. Nearly as good, a quarter of the data. | 22.6 MiB |
| `kospot-aura-ears-asr-sherpa-onnx-moonshine-tiny-en-int8.tar.bz2` | Aura's ears. Turns speech into text with no signal. | 102.6 MiB |
| `kospot-aura-ears-vad-silero_vad.onnx` | Hears when you start and stop talking. Bare `.onnx`, no archive. | 629 KiB |

### Why the names are long

Each name carries the app-side purpose **and** the original folder name. The
folder name is load-bearing: it is the directory inside the tarball, and the app
checks for it after unpacking (`AuraVoicePack.folder` /
`AuraListenPack.asrFolder`). Keeping it in the filename means the app's own
manifest test — which asserts the download URL contains the folder name — keeps
passing if the URLs are ever repointed here.

---

## Where each file came from

Downloaded 2026-09-10 from the [k2-fsa/sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx)
project's public releases.

| Mirrored as | Upstream URL |
| --- | --- |
| `...voice-natural-kokoro-int8-en-v0_19.tar.bz2` | `https://github.com/k2-fsa/sherpa-onnx/releases/download/tts-models/kokoro-int8-en-v0_19.tar.bz2` |
| `...voice-light-vits-piper-...-int8.tar.bz2` | `https://github.com/k2-fsa/sherpa-onnx/releases/download/tts-models/vits-piper-en_GB-southern_english_female-medium-int8.tar.bz2` |
| `...ears-asr-sherpa-onnx-moonshine-tiny-en-int8.tar.bz2` | `https://github.com/k2-fsa/sherpa-onnx/releases/download/asr-models/sherpa-onnx-moonshine-tiny-en-int8.tar.bz2` |
| `...ears-vad-silero_vad.onnx` | `https://github.com/k2-fsa/sherpa-onnx/releases/download/asr-models/silero_vad.onnx` |

Checksums are in `SHA256SUMS`. To prove a copy is untouched:

```bash
sha256sum -c SHA256SUMS
```

---

## THE CATCH — read before repointing the app at this repo

**A private repo's release assets cannot be downloaded without a GitHub token.**

The app downloads with a plain unauthenticated `GET` (see `auraDownloadFile` in
`lib/aura/aura_voice_pack.dart`). Pointed at a private release, every user would
get a `404`, and the app would correctly report "the server refused the download
(404)".

Putting a token in the app to fix that is not an option. Anyone can unzip an APK
and read it, and that token would have `repo` scope on the whole account.

So this repo is a **vault, not a CDN.** To actually serve from it you must first
do one of:

1. **Make the repo public.** Simplest. These are permissively licensed models
   (each archive carries its own `LICENSE`) — check each one still allows
   redistribution before doing this.
2. **Put the files somewhere public you own** — Supabase Storage, R2,
   Cloudflare Pages — and point the app there. Costs money in egress; roughly
   100 MB per user who installs a voice.

Either way, the change in the app is small: the URLs in
`lib/aura/aura_voice_pack.dart` and `lib/aura/aura_listen_pack.dart`.

---

## What must stay true if these are ever swapped

The app verifies what it unpacked. Two things will break an install silently if
they drift, and both are pinned by
`test/aura_voice_pack_manifest_test.dart` in the KosPot repo:

- **The folder inside the tarball** must match `AuraVoicePack.folder`.
- **The model filename inside that folder** must match `AuraVoicePack.modelFile`.

This is not hypothetical. The Kokoro pack shipped declaring `model.onnx` while
the archive contains `model.int8.onnx`. The download worked, the unpack worked,
and the install then failed with "the file downloaded but would not unpack" —
leaving 140 MB stranded on the phone with no way to remove it. It took three
attempts to find. Re-run `tar -tf` and update the test whenever a model changes.

---

## Contents of each archive

Verified with `tar -tf` on 2026-09-10.

**kokoro-int8-en-v0_19** — `model.int8.onnx`, `tokens.txt`, `voices.bin`,
`espeak-ng-data/`, `README.md`, `LICENSE`

**vits-piper-en_GB-southern_english_female-medium-int8** —
`en_GB-southern_english_female-medium.onnx`,
`en_GB-southern_english_female-medium.onnx.json`, `tokens.txt`,
`espeak-ng-data/`

**sherpa-onnx-moonshine-tiny-en-int8** — `preprocess.onnx`, `encode.int8.onnx`,
`uncached_decode.int8.onnx`, `cached_decode.int8.onnx`, `tokens.txt`,
`test_wavs/`, `README.md`, `LICENSE`

**silero_vad.onnx** — a single ONNX file, no archive.
