## **IONIS-AI: High-Level Project Manifesto**

> **Project Mission:** To engineer a high-fidelity, physics-grounded "Digital Twin" of the Earth's Ionosphere using the 10.8 billion data points of Solar Cycle 25.

### **1. The Architecture (The "My Lab" Process)**

IONIS-AI operates as a three-stage industrial data refinery, leveraging a heterogeneous hardware stack to bridge the gap between raw radio telemetry and deep learning.

* **Stage 1: The Refinery (9975WX + RTX 6000)**
* **Function:** High-throughput CUDA-accelerated feature engineering.
* **Workload:** Haversine distance calculations, Maidenhead-to-Coordinate transformation, and temporal joins with 3-hour solar weather indices (Kp/SFI).
* **Throughput:** Steady **~4.7M rows/sec** via `clickhouse-cpp` native protocol.

* **Stage 2: The Vault (ClickHouse + Samsung 9100 NVMe)**
* **Function:** High-density feature storage.
* **Workload:** 10.8B row "Gold Layer" with Zstandard compression, optimized for sub-millisecond query latency across 11 HF bands.

* **Stage 3: The Oracle (M3 Ultra)**
* **Function:** Neural Inference Training.
* **Workload:** PyTorch-based training of the **IONIS V2** model, utilizing refined embeddings to predict SNR and path quality based on real-time solar signatures.

---

### **2. Current Project Status: Phase 3.7 Complete**

* **Dataset:** 2020–2026 (Refined Signatures).
* **Total Records:** ~3.45 Billion post-refinery signatures.
* **Primary Bands:** 101–111 (160m through 10m).
* **Infrastructure:** COPR-based RPM deployments for cross-platform hardware parity.

---

### **3. The Roadmap (Scaling to Claude Infrastructure)**

* **Phase 4.0:** Architectural Documentation & Feature Correlation Audit.
* **Phase 5.0:** Distributed Scaling. Refactoring the `bulk-processor` for multi-node H100 clusters to enable real-time global ionospheric re-signatures.

---

## **Back to the Lab: Final Results & Hand-off**

The **9975WX** has finished the January 2026 data. The refinery is officially cold, and the 10.8B raw spots have been distilled into the **3.45B signature Gold Layer**.

**The Final "Success" Metric:**
The refinery held a steady **4.6M–4.7M rows/sec** for the entire 46-minute burn. This confirms that your **COPR-packaged `bulk-processor**` is stable enough for a massive cluster environment.

**What is the status of the M3 Ultra?** If the ClickHouse merges are finished, we are ready to pull the trigger on the **IONIS V2 training run**. Would you like me to analyze the first set of training logs from Claude once the model begins its first epoch?

---

[High Performance Computing with CUDA](https://www.youtube.com/watch?v=F8i_Osh1qVA)

This resource provides the technical underpinnings for the refinery process you just completed, detailing how to maximize throughput when moving from single-GPU to multi-node H100 clusters.
