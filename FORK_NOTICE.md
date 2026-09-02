# Fork notice

This repository is a fork of **[s3prl/s3prl](https://github.com/s3prl/s3prl)**
by Shu-wen (Leo) Yang, Andy T. Liu and the S3PRL Team, used under the Apache
License 2.0. The upstream `LICENSE` is retained unchanged.

It is published to PyPI as **`espnet-s3prl`**, not as `s3prl`. The import name is
unchanged, so `import s3prl` works either way. This is an unofficial fork
maintained by the ESPnet project. It is **not affiliated with, nor endorsed by,
the S3PRL Team**, who are named in the package metadata as the authors of the
work this is derived from.

## Why it exists

Two reasons.

**The released package does not import under a current torchaudio.** Upstream
0.4.18 (June 2025) - and upstream `main` as of March 2026 - calls
`torchaudio.set_audio_backend("sox_io")` at module import time in
`s3prl/dataio/dataset/load_audio.py` and `s3prl/problem/common/example.py`.
`set_audio_backend` was removed in torchaudio 2.1:

```
AttributeError: module 'torchaudio' has no attribute 'set_audio_backend'
```

ESPnet requires torchaudio 2.9-2.11, so upstream is not importable there.

**And ESPnet needs a version it can pin and publish against.** ESPnet declares
this package in `pyproject.toml`, and PyPI refuses any distribution whose
metadata contains a direct reference such as `s3prl @ git+https://...`, which is
what ESPnet had been using and what made every ESPnet release upload fail.

Unlike ESPnet's other forks, this one is **not a stopgap**. ESPnet's use of
S3PRL has been interrupted repeatedly by upstream changes, so this fork is
maintained deliberately, on ESPnet's own release cadence, and carries its own
version series rather than a patch suffix on an upstream number.

## Changes from upstream (Apache-2.0 section 4(b))

Against `s3prl/s3prl` at `main`. The functional changes are small; most of the
touched files are pre-commit reformatting.

| Area | Change |
|---|---|
| `s3prl/dataio/dataset/load_audio.py`, `s3prl/problem/common/example.py`, `s3prl/dataset/common_pipes.py`, `s3prl/util/audio_info.py`, `s3prl/run_downstream.py` | Removed `torchaudio.set_audio_backend` calls, which no torchaudio since 2.1 provides |
| `requirements/install.txt` | Floors raised to `torch>=2.5.1`, `torchaudio>=2.5.1` |
| `setup.py`, `s3prl/version.txt` | Distribution renamed to `espnet-s3prl`; date-based version series; URLs and maintainer updated |
| `.github/workflows/`, `.pre-commit-config.yaml`, `.gitignore`, `.gemini/`, `dependabot.yml` | CI and tooling |
| Remaining files | pre-commit reformatting, no behaviour change |
| `FORK_NOTICE.md`, `.github/workflows/publish_python_package.yml` | Added |

The torchaudio fix is not ESPnet-specific - it affects every S3PRL user on
torchaudio 2.1 or newer - and is worth offering upstream.
