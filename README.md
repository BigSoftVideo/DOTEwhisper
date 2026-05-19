# Dote Whisper

**Private, on-device audio transcription — built for researchers.**

Dote Whisper turns recorded audio (interviews, lectures, focus groups, field recordings) into accurate, time stamped transcripts. Everything runs locally on your own computer. No account, no API key, no internet round-trip, no data leaving your machine.

It is designed for academic work where confidentiality and privacy matter, and when you don't always have the time or the budget to have your hundreds of hours of recordings professionally transcribed.

---

## Why use it

- **Your audio never leaves your computer.** Suitable for IRB-/ethics-approved recordings, sensitive interviews, and other material you cannot upload to a cloud service.
- **No subscriptions, no per-minute fees.** One-time download. Transcribe as much as you like.
- **Verbatim transcription** A useful first step when planning analysis or trying to manage hundreds of hours of field recordings.
- **Word-level timestamps and confidence scores.** Every word in the transcript carries its own start time, end time, and a 'confidence' value between 0 and 1.
- **Automatic speaker labels.** Optional diarization process tags each segment with a speaker ID (`S_01`, `S_02`, …) so that you can see who said what. (Note: the computational science of speaker diarization is still in it's infancy, so for noisier recordings and those involving many speakers, automatic speaker ID's may be a rough guide at best.)
- **Open transcript format.** Transcripts export as plain JSON with a stable, documented schema. Perfect for importing into [DOTE](https://www.dote.aau.dk/what-is-dote). - Also exportable as SRT or VTT files.

---

## Features at a glance

| Feature | What it does |
|---|---|
| **High-quality speech recognition** | Powered by [whisper.cpp](https://github.com/ggml-org/whisper.cpp), a native build of OpenAI's Whisper. |
| **Word-level confidence** | Each word gets a 0–1 score so you can see which parts of the transcript are reliable and which need a human ear. A built-in colour "heat strip" highlights low-confidence stretches.  |
| **GPU acceleration** | Automatic. Uses Metal on Apple Silicon Macs and CUDA on Windows machines with an NVIDIA GPU. Falls back to CPU otherwise. |
| **Multiple model sizes** | From `tiny` (~75 MB, fast drafts) to `large-v3` (~3 GB, best accuracy). Models download on demand and are cached. |
| **Custom HuggingFace models** | Install community fine-tuned models (e.g. domain-specific or non-English models) straight from the app. |
| **Cancellable, progress-tracked jobs** | Long transcriptions show real progress (load, decode, transcribe, align, diarize, format) and can be stopped cleanly. |
| **Speaker diarization** | Detects who is speaking when, using [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx). Works without a HuggingFace token. |
| **Batch queue** | Drop a folder of recordings; Dote Whisper works through them one by one. |
| **JSON export** | Stable, documented schema with segments, words, speakers, timestamps, and confidences. |

---

## Supported platforms

| Platform | Notes |
|---|---|
| macOS, Apple Silicon (M1/M2/M3/M4) | GPU-accelerated via Metal. (Recommended) |
| macOS, Intel | CPU only — works, but transcribing long recordings takes noticeably longer. |
| macOS Universal | There is a single MacOS installer that runs natively on both Apple Silicon and Intel. |
| Windows 10/11, 64-bit | GPU accelerated if you have a recent NVIDIA GPU and driver. Otherwise it uses CPU.|

---

## Getting started

### 1. Install

Download the latest installer for your operating system from the
[Releases page](../../releases/latest).

| Your system | File to download |
|---|---|
| Mac (universal) | `Dote-Whisper-osx-<version>.dmg` |
| Windows | `Dote-Whisper-win-x64-<version>.exe` |
| Mac (for admin/IT installation) | `Dote-Whisper-osx-<version>.pkg` |
| Windows (for admin/IT installation) | `Dote-Whisper-win-x64-<version>.msi` |

**macOS:** open the `.dmg`, drag *Dote Whisper* into your Applications folder, then launch it. The first time you open it, macOS may ask you to confirm that you want to run an application downloaded from the internet — this is expected. - If you are using an institutionally-managed computer, and do not normally have administrator rights, you *may* need to use the `.pkg` installer with the help of your IT department.

**Windows:** double-click the `.exe`. The app installs into your user profile (no admin rights needed) and creates a Start-menu shortcut. - If you are using an institutionally-managed computer, and do not normally have administrator rights, you *may* need to use the `.msi` installer with the help of your IT department.

### 2. Pick a model

On first launch the app will offer to download a Whisper model. If you are unsure,
**start with `base.en` for English audio** or **`base` for other languages** —
they strike a good balance between speed and accuracy and only need ~500 MB of
disk space.

| If you care most about… | Pick |
|---|---|
| Speed (quick drafts, short clips) | `base` or `base (English only)`|
| Balanced quality (default) | `mediem` or `medium (English only)` |
| Best accuracy (long, noisy, or non-English audio) | `Large v3` |

Use the `(English Only)` variants whenever you know the audio is English — they're a little faster and slightly more accurate than the multilingual model of the same size.

### 3. Transcribe a recording

1. Click **Add files** (or drag-and-drop) to load one or more audio files. Common formats are supported: WAV, MP3, M4A, FLAC, OGG, plus most video files (the audio track is extracted automatically).
2. (Optional) Toggle **Speaker labels** on if you want diarization. The first time you enable this, the diarization models (~46 MB total) will download.
3. (Optional) Set the **Language** if you know it. Leave it blank to auto-detect. For noisy recordings, it is highly recommended that you manually set the language to avoid a incorrect assumption.
4. Click **Transcribe**.

Progress is shown for each file. When a job finishes, the transcript appears in the main pane with words colour-coded by confidence. You can:

- **Copy** plain text to the clipboard.
- **Export** to JSON for downstream analysis, or import into [DOTE](https://www.dote.aau.dk/what-is-dote).
- **Export** to SRT or VTT for use as subtitles or import to other software.

### 4. Reading the confidence heat strip

Below the **full text** the transcript is listed in segments or turns (if diarization is enabled). Below each word is a thin coloured bar representing the 'confidence' of that words transcription. Each word is shaded from green
(high confidence) through yellow to red (low confidence). Low-confidence stretches are the ones most worth listening back to. Each sequence also has a total confidence percentage, calculated as the average confidence of all the words in that segment.

---

## Adding custom models from HuggingFace

Out of the box, Dote Whisper ships a standard array of OpenAI Whisper sizes (base, medium, large). For many research projects you'll want something more specialised — such as a model fine-tuned on Swedish parliamentary speeches, on medical dictation, on a low-resource language, on noisy field recordings, etc. The community has converted hundreds of these and published them on [HuggingFace](https://huggingface.co/).

Dote Whisper can install most Whisper-architecture-based models from HuggingFace directly. (as long as the repository is public).

### How to add one

1. Open **Settings → Models → Add custom model**.
2. Paste the HuggingFace **repo identifier** — this is the `owner/name` part from the URL. For example, the URL `https://huggingface.co/KBLab/kb-whisper-large` has the repo id `KBLab/kb-whisper-large`. Full URLs are also accepted and will be converted automatically.
3. Click **Probe & install**.

Dote Whisper will check the repo, work out which files it needs, download them, and verify their integrity. Progress is shown live. When it finishes, the new model appears in the model dropdown alongside the built-ins.

### Three kinds of repositories are compatible

The app handles three common layouts you'll see on HuggingFace:

1. **Pre-converted ggml/GGUF repos.** Repos whose names end in `-ggml` or `-gguf`, or that already contain a `ggml-*.bin` file. These install directly — fastest path. Example: `ggerganov/whisper.cpp`.
2. **Single-file safetensors repos.** Standard HuggingFace Whisper repos with one `model.safetensors`. The app downloads the raw weights and converts them on your device. Typical for `tiny`/`base`/`small` fine-tunes. Example: `openai/whisper-tiny`.
3. **Sharded safetensors repos.** Larger models split across several `model-00001-of-00003.safetensors` files plus an index. Also converted on device. Typical for medium/large fine-tunes. Example: `CoRal-project/roest-v3-whisper-1.5b` (Danish).

For (2) and (3), conversion runs locally and typically takes a few minutes depending on model size and your CPU. You'll see a live progress bar, and the app will warn you in advance if there isn't enough free disk space. Extra disk space is needed during the conversion process, but it is released again once the conversion is complete.

### Tried-and-tested examples

| Repo id | Notes |
|---|---|
| `ggerganov/whisper.cpp` | The standard mainline Whisper models, pre-converted. |
| `KBLab/kb-whisper-large` | Swedish-optimised large model. |
| `distil-whisper/distil-large-v3` | Distilled large-v3 — faster, slightly less accurate. |
| `CoRal-project/roest-v3-whisper-1.5b` | Danish, sharded weights, converted on device. |
| `openai/whisper-tiny` | Tiny model, converted on device. Good for testing the flow. |

### Finding new models

Click **Browse on HuggingFace** in the *Add custom model* dialog. This opens the HuggingFace website already filtered to Whisper-architecture models. You can optionally type a 2-letter ISO language code (e.g. `da` for Danish, `ja` for Japanese, `fr` for French) to narrow the list further. On the repo page that you like, copy the `owner/name` from the title and paste it back into the dialog.

### A note on quantisation (advanced)

When converting a model on device, the app defaults to a sensible balance between size and quality, Q5_1 – roughly 50% smaller at a small cost in accuracy compared to F16 (representing full precision). If you're tight on disk space or want to squeeze more speed out of a small computer, you can choose an even more *quantised* variant (Q5, Q4). If space and time are of no consequence, feel free to switch to F16 for 'the full experience'. If you're not sure, leave the default. The "Show advanced variants" link reveals the full picker.

### What if the model isn't compatible?

If you paste in a reference to a repo that uses a different architecture (Conformer, wav2vec2, NeMo Canary, etc.) the app will tell you clearly and list everything it tried. The underlying engine (whisper.cpp) only understands the Whisper architecture itself.

---

## Where files are stored

Models, settings, and logs live in your user data folder:

| OS | Location |
|---|---|
| macOS | `~/Library/Application Support/dote-whisper/` |
| Windows | `%APPDATA%\dote-whisper\` |

Models specifically go in a `whisper-models/` subfolder. You can move this folder to an external drive or shared location and point the app at it from **Settings** — handy if you want to share large models across machines or keep them off your system drive.

---

## Troubleshooting

**The app is slow on my Intel Mac / older laptop.** Try a smaller model (`base`, or even `medium`). The `large` models really need a GPU to be comfortable.

**Custom model download succeeded but the transcript looks coarser than usual.**
Some custom models don't have a matching word-alignment preset, so the app falls back to slightly less precise (~20 ms instead of ~10 ms) word timestamps. The transcript itself is unaffected.

**"HuggingFace repo not found" when adding a custom model.** Check spelling and that the repo is public. Private HuggingFace repos aren't supported.

**Diarization splits one speaker into two (or merges two into one).** Speaker clustering is imperfect, especially on short recordings, overlapping speech, or low-quality audio. If you have a longer recording of the same speakers, results usually improve.

**A model download is slow.** Models are pulled from HuggingFace's CDN — speed
depends on your region. The app saves to a partial file and resumes cleanly on retry if interrupted.

For anything else, please open an issue on the
[Issues page](../../issues) with:
- your operating system and version,
- the model you were using,

---

## Citing Dote Whisper

If you use Dote Whisper in published academic work, please also cite the underlying projects it builds on:

- OpenAI Whisper — Radford et al., 2022.
- whisper.cpp — Georgi Gerganov.
- sherpa-onnx — k2-fsa.

A `CITATION.cff` is included in this repo for convenience.

---

## License

Dote Whisper is released under the **MIT License**. The bundled models retain their original upstream licenses (mostly MIT or Apache-2.0). See `LICENSE` in the release archive for full text.

## Credits

Built on the work of many open-source projects, especially:

- [whisper.cpp](https://github.com/ggml-org/whisper.cpp) — speech recognition runtime
- [OpenAI Whisper](https://github.com/openai/whisper) — the underlying model family
- [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) — speaker diarization
- [Electron](https://www.electronjs.org/) — cross-platform desktop framework
- [shadcn/ui](https://ui.shadcn.com/) — interface components
