# IONIS-AI — Ionospheric Neural Inference System

*The logs were speaking for decades, but nobody was listening. Now we're listening.* 

IONIS is an open-source HF radio propagation prediction system trained on
13+ billion amateur radio spots (WSPR, RBN, Contest, PSK Reporter). It predicts
signal-to-noise ratio for any HF path on Earth using real-time solar conditions.

## What It Does

- Predicts HF propagation (SNR) between any two points, any band, any time
- Trained on the largest curated amateur radio propagation dataset that we know of
- Uses real-time solar flux (SFI) and geomagnetic (Kp) indices
- Validated against VOACAP: Pearson +0.49 vs +0.02, 84% recall on independent live data

## Packages

| Package | Description |
|---------|-------------|
| [ionis-apps](https://github.com/IONIS-AI/ionis-apps) | Data ingesters (WSPR, RBN, Contest, PSK Reporter, Solar) and pipeline tools |
| [ionis-core](https://github.com/IONIS-AI/ionis-core) | ClickHouse DDL schemas, population scripts, base configuration |
| [ionis-cuda](https://github.com/IONIS-AI/ionis-cuda) | GPU-accelerated signature embedding engine (CUDA) |
| [ionis-training](https://github.com/IONIS-AI/ionis-training) | PyTorch model training and sensitivity analysis |
| [ionis-tools](https://github.com/IONIS-AI/ionis-tools) | Prediction CLI and REST API |
| [ionis-docs](https://github.com/IONIS-AI/ionis-docs) | Documentation site |

## Dataset

| Source | Volume | Years |
|--------|--------|-------|
| WSPR | 10.8B spots | 2008–2025 |
| Reverse Beacon Network | 2.18B spots | 2009–2025 |
| CQ Contest Logs | 195M QSOs | 2005–2025 |
| PSK Reporter | ~26M spots/day (live) | 2026+ |

## Model

**IONIS V20** (Production) — 203K parameter PyTorch model with physics-constrained
solar and geomagnetic sidecars.

| Metric | IONIS V20 | VOACAP |
|--------|-----------|--------|
| Pearson correlation | **+0.49** | +0.02 |
| Live recall (PSK Reporter) | **84%** | — |

## Install

```bash
# Rocky Linux 9 / RHEL 9 (via COPR)
dnf copr enable ki7mt/ionis-ai
dnf install ionis-apps ionis-core
```

## License

GPLv3 — See individual repositories for details.
