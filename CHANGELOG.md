# Changelog

All notable changes to this simulation artifact are documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [3.0.0] — Camera-ready release (IDEASET-2026, Submission 43)

### Fixed
- **Deterministic seeding**: replaced Python's process-randomized `hash()` with
  `zlib.crc32` in the seed formula. Results are now bit-for-bit reproducible
  across machines and runs.
- **Packet-loss degradation model**: aligned the implementation with the
  paper's Eq. (3) (additive form `p_loss = min(p0 + beta*overload, 0.35)`).
  v1.2.0 implemented a multiplicative variant, which under-stated loss for
  the stress scenario.

### Added
- **Scenario S3 (Private APN with degradation)** to `configs/paper.yml`,
  fully documented: latency 60 ms, p0 = 0.1%, retries = 2, E2E timeout 5 s,
  queue timeout 4 s, degradation enabled (alpha_L = 0.75, lambda_th = 25 TPS,
  beta = 1.5, loss cap 35%). S3 was reported in the paper but absent from the
  published configuration in v1.2.0.
- **Bank queueing clarification** in `docs/PARAMETERS.md`: M/D/c, c = 50
  parallel servers, deterministic 30 ms service, aggregate capacity ~1,667 TPS.
- `CHANGELOG.md` (this file).

### Changed
- Documentation now references **Table V** of the camera-ready paper
  (previously "Table III") and the **IDEASET-2026** venue (previously "DTISD").
- Regenerated all outputs (Tables VI-VIII, Figs. 6-7) for the three scenarios
  with the deterministic seeds.

---

## [1.2.0]
- Initial Zenodo release accompanying the paper submission (S1, S2).
