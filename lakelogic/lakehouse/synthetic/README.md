# Synthetic data generator bundle (LakeLogic Synthetic v2)

Source: lakehouse design bp:snap-aff8243e11c14b4a r1
Generated at: revision:see .lakelogic/project.yaml seal:see .lakelogic/project.yaml
Generator: lakelogic DataGenerator v1.89.0

## Fidelity profile: contract-derived

This bundle ships a **deterministic seeded generator**, not synthetic rows. It was built
from the contract **schemas only** — **no production rows were read** — so it can be shared
freely: no synthetic data crosses a boundary until *you* run `generate.py` locally.

Values are contract-derived: the generator uses semantic hints from column names (email,
name, phone, address, ...) and type-correct fakes otherwise. This is NOT profile- or
sample-informed fidelity (those are later, separately-governed modes); no distribution or
value from your production data informs these fakes.

## Permanent synthetic classification

Every artifact produced by this bundle is **synthetic, permanently** (ADR-0011 decision 6:
synthetic is not the same as anonymised). Do not re-classify generated data as production.

## PII / masked columns

Columns flagged `pii` / `sensitive` in the contracts carry a `masking` strategy. Run the
generated frames through `lakelogic.DataProcessor(<contract>).run(df)` to apply masking by
construction (`redact` -> `***REDACTED***`, `hash` -> pseudonym). The generator emits
semantically-shaped fakes for these columns; it never emits a real identifier.

## Staged rollout (use synthetic BEFORE real data)

Run any exported pipeline against THIS synthetic data first, staged:
`dry_run` -> `ddl_only` -> `live` — validate the design on synthetic before pointing it at
production. Injected-invalid rows (`invalid_ratio=0.0`) let you prove your
quality gates quarantine bad rows.

## Contents

  - `generate.py` — the standalone generator (needs `pip install lakelogic`)
  - `config.json` — the pinned config (seed, rows, window, relationships)
  - `contracts/*.yaml` — the contracts (deterministically serialized, once; the canonical source of truth)

## Config

  - seed: 7
  - rows_per_table: 100
  - invalid_ratio: 0.0
  - window: 2020-01-01T00:00:00 .. 2025-01-01T00:00:00 (module default)

## Tables

  - bronze_checkr_driver_licences
  - bronze_rideflow_driver_profiles
  - bronze_rideflow_driver_telemetry
  - bronze_rideflow_rider_app_events
  - bronze_rideflow_rider_profiles
  - gold_dim_city_code
  - gold_dim_driver_licences
  - gold_dim_driver_profiles
  - gold_dim_rider_profiles
  - gold_dim_vehicle_type
  - gold_fact_driver_telemetry
  - gold_fact_rider_app_events
  - silver_checkr_driver_licences
  - silver_rideflow_driver_profiles
  - silver_rideflow_driver_telemetry
  - silver_rideflow_rider_app_events
  - silver_rideflow_rider_profiles
