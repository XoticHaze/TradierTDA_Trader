# Public Market Data Transport Plane

This directory is a **public data transport/catalog surface** for research compute. It is not trading, StrategySpec, Foundry, corpus-selection, model-selection, or promotion authority.

## Purpose

Use this plane to move redistributable market-data bytes from approved source owners into GitHub-hosted compute (including Codespaces and public Actions) without exposing private application or research source code.

Recommended flow:

`approved source -> immutable export -> public manifest + SHA256 -> public release/shards -> Codespaces/compute -> private Foundry consumes by exact identity -> research artifact`

## Fail-closed publication rule

A dataset may be published here only when its manifest declares:

- `redistribution.status = "allowed"`;
- a concrete rights/provenance basis;
- immutable SHA-256 identity for every payload/shard;
- explicit symbol, asset type, timeframe, time range, format, and row count when applicable.

`unknown` or `prohibited` redistribution status must never publish payload bytes. Broker credentials, account identifiers, private source code, proprietary secrets, and live-trading state are never valid contents of this plane.

## Layout

```text
market_data/
  manifest.schema.json
  <asset_type>/
    <symbol>/
      <dataset_id>/
        manifest.json
        checksums.sha256
```

Large immutable payloads should be kept out of ordinary Git history where practical. The manifest may reference immutable public release assets or bounded compressed shards. Consumers must verify SHA-256 before use.

## Authority boundaries

- This repository says **what public bytes exist and how to verify them**.
- Foundry remains research execution/evidence authority.
- MM-IBKR remains its own application/runtime/StrategySpec authority.
- Publication here does not make a dataset suitable for training, promotion, or trading.
- Derived corpora must retain upstream provenance and redistribution basis.

## Initial lane

The first intended dataset family is deep-history MNQ research data already proven through the public-data transfer path. Expansion to additional futures/commodities should reuse the same manifest contract rather than inventing per-symbol publication rules.
