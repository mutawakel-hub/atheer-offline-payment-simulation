# Parameter Mapping (Paper <-> Config)

Update `configs/paper.yml` whenever you change scenario parameters.

The parameters in `configs/paper.yml` **match exactly** the values declared in **Table V** of the
IDEASET-2026 camera-ready paper (v3.0.0 of this artifact).

| Parameter Name              | Paper (Table V)      | `paper.yml` Value   | Status |
|-----------------------------|----------------------|---------------------|--------|
| Network Mean Latency S1     | 400 ms               | 400 ms              | Match |
| Network Mean Latency S2/S3  | 60 ms                | 60 ms               | Match |
| Baseline Packet Loss S1     | 10%                  | 10%                 | Match |
| Baseline Packet Loss S2/S3  | 0.1%                 | 0.1%                | Match |
| Max Packet Loss (cap)       | 35%                  | 35%                 | Match |
| S1/S3 Retries               | 2                    | 2                   | Match |
| S2 Retries                  | 0                    | 0                   | Match |
| Bank servers (c)            | 50 servers           | 50                  | Match |
| Bank service time           | 30 ms (fixed)        | 0.030 s             | Match |
| Aggregate bank capacity     | ~1,667 TPS           | c x (1/0.030)       | Derived |
| S1 E2E timeout              | 15 s                 | 15.0                | Match |
| S2/S3 E2E timeout           | 5 s                  | 5.0                 | Match |
| Queue timeout S1 / S2,S3    | 12 s / 4 s           | 12.0 / 4.0          | Match |
| Degradation threshold       | lambda_th = 25 TPS   | 25.0                | Match |
| Latency alpha (S1 & S3)     | alpha_L = 0.75       | 0.75                | Match |
| Loss beta (S1 & S3)         | beta = 1.5           | 1.5                 | Match |
| Load degradation S1/S3      | Enabled              | enabled: true       | Match |
| Load degradation S2         | Disabled             | enabled: false      | Match |

## Bank queueing model (clarified in v3.0.0)

The core-banking subsystem is an **M/D/c queue**: Poisson arrivals, `c = 50`
parallel servers (a `simpy.Resource` with capacity 50), and a **deterministic
30 ms service time** per transaction (ISO 20022-style fixed processing).
The aggregate service capacity is therefore `c x (1/0.030 s) ~ 1,667 TPS`,
and the queue is FCFS with unbounded length bounded in practice by a
queue-timeout (12 s in S1, 4 s in S2/S3) and the end-to-end timeout.
At the maximum offered load of 500 TPS the utilization is rho ~ 0.30, so the
bank subsystem never saturates; high-load failures originate from the network
path, not the bank.

## Packet-loss degradation (clarified in v3.0.0)

Loss under load follows the paper's Eq. (3): additive form
`p_loss(lambda) = min(p0 + beta * max(0, lambda - lambda_th)/lambda_th, 0.35)`.
(v1.2.0 implemented a multiplicative variant `p0 * (1 + beta*overload)`;
v3.0.0 aligns the implementation with the published equation.)

## Determinism

Seeds are `BASE_SEED + (zlib.crc32(scenario_key) % 100000) * 1000 + tps * 10 + run_idx`.
v1.2.0 used Python's built-in `hash()`, which is randomized across processes;
v3.0.0 replaces it with `zlib.crc32` so results are bit-for-bit reproducible
on any machine.

> **Note:** The simulation results (Figs. 6, 7 and Tables VI-VIII) are generated
> using the `paper.yml` values above. Run `python atheer_sim.py` to reproduce.
