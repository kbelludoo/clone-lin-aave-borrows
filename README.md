# clone-lin-aave-borrows

Experimental **LIN** clone of Aave V3 `GenericLogic.calculateAvailableBorrows`
(remaining borrow capacity) plus `ValidationLogic` isolation-mode debt ceiling.

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [aave/aave-v3-core](https://github.com/aave/aave-v3-core) `782f51917056a53a2c228701058a6c3fb233684a`
  - `GenericLogic.sol` sha256 `6a5dbf307801b001ae5cd450c7bc6a7c77947b50f4bc29bdb93661ed22ea795b` blob `3342316e772a7e733723ef2963ab79566904fb30`
  - `PercentageMath.sol` sha256 `6f165ad32ddcc3383cbb611cc3045c1a4bb28f16f83a85015fe202803de0161f` blob `c000be3a6c38a959aa3d28737445eaa5762c53b0`
  - `ValidationLogic.sol` sha256 `5d5f7a76a4e03b258679792885c068e2974293679d7baea09c0b7d6de0c2d329` blob `75218f0549067bf1a1a037d401668abe3fcba648`
- **License of the algorithm:** MIT after BUSL Change Date 2023-01-27. This clone is original LIN/C11.
- **Class:** EXPERIMENTAL uint64-scale, `PERCENTAGE_FACTOR = 1e4` (same bps as Aave). Not mainnet `WAD = 1e18` / oracle / aToken.

## Files

- `src/lin_aave_borrows.lin` — LIN kernel (`aab_available(coll, debt, ltv)`, `aab_iso_ok(active, current, amount, decimals, ceiling)`)
- `test/oracles/aave_borrows_c11.c` — independent C11 `__int128` XOR limb oracle (not TCB)

## Reproduce proofs (in lin-open)

```bash
make aave-borrows-proof
# or: python3 test/prove_aave_borrows_external.py
```

Named goldens: 80% LTV on `1e9` collateral with `1e8` debt **700000000**;
underwater `1000/900` **0**; USDC 6-dec `1e6` **100** isolation units;
`percentMul(1, 0.5e4)` half-up **1** vs floor **0**.

## Not claimed

Uint256 / WAD=1e18 / oracle / aToken mainnet parity. LIN replacing Aave or the EVM.
TVL or dollar savings. zk soundness. Full `calculateUserAccountData` (oracle loop).
