# U.S.O. WAV Doctor

A single-page, browser-based tool by [Unidentified Sound Object](https://unidentifiedsoundobject.com) that repairs `.wav` files left unplayable by a crash, a power cut or a full disk — typically when a recording app dies mid-write and never finalises the RIFF header. Runs entirely in the browser: nothing is uploaded, no cookies, no analytics, no external dependencies. Free to use, source available.

## Usage

Open `index.html` in Google Chrome (or host it as a static page, e.g. GitHub Pages), then drop in one or more `.wav` files. Each file gets a diagnostic report, a chunk map, an in-page audio preview and a download button for the repaired copy (`<name>_repaired.wav`). The original is never modified. Full user instructions, a manual-mode guide and an FAQ are on the page itself.

## What it does

- Verifies the `RIFF`/`WAVE` signature (also accepts `RF64`/`BW64` and rewrites them as standard RIFF) and recalculates the declared sizes from the real file length
- Walks the chunk list; if a chunk size is implausible, falls back to a direct search for `fmt ` and `data` in the first 4 MB
- Trusts a declared `data` size only when a valid chunk (or end of file) follows it, so stale sizes from periodic header updates don't truncate the audio, while trailing metadata in healthy files isn't read as audio
- Keeps intact metadata chunks (`bext`, `iXML`, `cue `, `LIST`, …) in their original order and rewrites `fact` with the correct frame count; drops damaged chunks and padding (`JUNK`, `FLLR`, `ds64`)
- Trims the audio to a whole number of frames and adds the RIFF pad byte when needed
- Manual mode builds a new header from sample rate, channels, bit depth, format (PCM / IEEE float) and data offset, with preview to test combinations by ear

## Limits

**File size:** the whole file is loaded into memory, so the practical limit is around 1–2 GB depending on the browser and available RAM.

**Browsers:** tested on desktop Google Chrome only. Other modern browsers should work but are untested.

**Other:** output larger than 4 GB (RF64) is not supported. The tool repairs the container, not the audio: dropouts or missing data stay as they are.

## Files

- `index.html` — the tool
- `corrupted_header.wav` — 2 s test file (48 kHz / 16-bit / stereo, 440 Hz) with RIFF size = 36 and data size = 0

## License

[PolyForm Shield License 1.0.0](LICENSE.md). The tool is free for anyone to use, including professionals in paid work, and may be shared and modified. It may not be sold, repackaged or used to provide a product or service that competes with it. Any copy must keep the license and the `Required Notice` line.

This is a source-available license, not an OSI-approved open-source license.

## Credits

Built by [Unidentified Sound Object](https://unidentifiedsoundobject.com).
