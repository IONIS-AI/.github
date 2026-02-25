# IONIS-AI — Ionospheric Neural Inference System

*The logs were speaking for decades, but nobody was listening. Now we're listening.*

IONIS is an open-source HF radio propagation prediction system trained on
14+ billion amateur radio observations (WSPR, RBN, Contest, PSK Reporter). It predicts
signal-to-noise ratio for any HF path on Earth using real-time solar conditions.

## What It Does

- Predicts HF propagation (SNR) between any two points, any band, any time
- Trained on the largest curated amateur radio propagation dataset that we know of
- Uses real-time solar flux (SFI) and geomagnetic (Kp) indices
- Validated against VOACAP: Pearson +0.49 vs +0.02, 84% recall on independent live data

## Packages

| Package | Language | Description |
|:--------|:---------|:------------|
| [ionis-apps](https://github.com/IONIS-AI/ionis-apps) | Go | Data ingesters (WSPR, RBN, Contest, PSK Reporter, Solar) with idempotent watermark tracking |
| [ionis-core](https://github.com/IONIS-AI/ionis-core) | SQL/Shell | 36 ClickHouse DDL schemas, 15 population scripts, base configuration |
| [ionis-cuda](https://github.com/IONIS-AI/ionis-cuda) | C++/CUDA | GPU-accelerated signature embedding engine |
| [ionis-training](https://github.com/IONIS-AI/ionis-training) | Python | PyTorch model training, sensitivity analysis, version lifecycle |
| [ionis-validate](https://github.com/IONIS-AI/ionis-validate) | Python | Model validation CLI ([PyPI](https://pypi.org/project/ionis-validate/)) |
| [ionis-hamstats](https://github.com/IONIS-AI/ionis-hamstats) | Python/MkDocs | [ham-stats.com](https://ham-stats.com) — live HF propagation reports |
| [ionis-docs](https://github.com/IONIS-AI/ionis-docs) | MkDocs | [Documentation site](https://ionis-ai.github.io/ionis-docs/) |

## Dataset

| Source | Volume | Years |
|:-------|:-------|:------|
| WSPR | 10.9B spots | 2008–2026 |
| Reverse Beacon Network | 2.26B spots | 2009–2026 |
| CQ Contest Logs | 234M QSOs | 2005–2025 |
| PSK Reporter | ~26M spots/day (live) | 2026+ |

## Model

**IONIS V22-gamma + PhysicsOverrideLayer** (Production) — 205K parameter PyTorch model
with physics-constrained solar and geomagnetic sidecars, plus a deterministic
post-inference clamp for high-band night closure.

| Metric | IONIS V22-gamma | VOACAP |
|:-------|:----------------|:-------|
| Pearson correlation | **+0.49** | +0.02 |
| RMSE | **0.821σ** | — |
| KI7MT operator tests | **17/17** | — |
| TST-900 band x time | **9/11** | — |

## Data Pipeline

Every data source follows a paired **download → ingest** pattern with idempotent
watermark tracking. Downloaders write to disk, ingesters load into ClickHouse.
All ingesters support `--full`, `--prime`, and `--dry-run` modes.

| Source | Downloader | Ingester | Schedule |
|:-------|:-----------|:---------|:---------|
| WSPR | `wspr-download` | `wspr-turbo` | Daily |
| RBN | `rbn-download` | `rbn-ingest` | Daily |
| Contest | `contest-download` | `contest-ingest` | Weekly |
| PSK Reporter | `pskr-collector` | `pskr-ingest` | Hourly |
| Solar | `solar-download` | `solar-ingest` | 15 min / 6 hr |
| DSCOVR L1 | `dscovr-ingest` | `dscovr-ingest` | 15 min |

## Install

```bash
# Rocky Linux 9 / RHEL 9 (via COPR)
dnf copr enable ki7mt/ionis-ai
dnf install ionis-apps ionis-core

# Model validation (any platform, via PyPI)
pip install ionis-validate
```

## License

GPLv3 — See individual repositories for details.
