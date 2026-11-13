# clone-lin-velo-getin

Experimental **LIN** clone of Velodrome Finance V2 (Solidly-style)
exact-output `getAmountIn` — the inverse of published `Pool.getAmountOut`
(volatile closed-form +1 ceiling; stable `x³y+y³x` Newton then fee gross-up).
Same kernel applies to Aerodrome on Base. Pool.sol does **not** publish `getAmountIn`.

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [velodrome-finance/contracts](https://github.com/velodrome-finance/contracts) `b3065d8b6702b14b094f9f6046b752cc9f78c43b`
  - `contracts/Pool.sol` sha256 `32d040ff419fa9b1f9113f7e10d587b332a2ab9c6b6099851025cc16a4e986bb` blob `91412c378bba4d8076271cee52e1397b1520b709`
  - License GPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_velo_getin.lin`](src/lin_velo_getin.lin) (sha256 `d979a4812941f73d72a1ece58dbc39295ce69614d86da9c8f52b118de2c44777`, identical to lin-open)
- **C11 oracle copy:** [`test/oracles/velo_getin_c11.c`](test/oracles/velo_getin_c11.c)
- **Proofs (lin-open only):** `examples/velo_getin/` and `make velo-getin-proof`
- **Class:** EXPERIMENTAL uint64-scale, `SCALE = 1e9`. Not Optimism/Base uint256. Not 1e18 decimals. Not a production pool replacement.

Named goldens (equal `1e9` reserves, `amountOut=1e8`): volatile fee 0 → **111111112**; fee 5 → **111166695**; fee 30 (Uniswap-0.3% match) → **111445448**; naive floor **111445447** (ceil gap **1**); round-trip `getAmountOut` **100000000**; stable fee 0 → **100050050**; fee 1 → **100060056**; `dx=0` / `aout>=reserve` / `fee=10000` fail-closed **0**. Naive i64 `4e9*4e9` wraps to **-2446744073709551616**; 128-bit muldiv fail-closes to **0**.

## Reproduce proofs (in lin-open)

```bash
make velo-getin-proof
# or: python3 test/prove_velo_getin_external.py
```

## Not claimed

Uint256 / ONE=1e18 / token-decimals mainnet parity.
LIN replacing Velodrome, Aerodrome, Uniswap, or the EVM. TVL or dollar savings. zk soundness.
Pool `getAmountOut` itself (see clone-lin-velo-getout). Solidly cubic `k` (see clone-lin-solidly-k).
