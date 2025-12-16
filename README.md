# Uniswap V2 Core (Parallel Friendly)

This repository is a modified version of [Uniswap V2 Core](https://github.com/Uniswap/uniswap-v2-core), optimized for parallel execution environments and benchmarking within the Altius project.

## Modifications

The main changes are located in `contracts/UniswapV2Pair.sol`. The modifications aim to **remove control flow divergence** (branchless programming) to make the bytecode more friendly for parallel execution engines, ZK circuits, or specific VM optimizations.

### Key Changes

1.  **Branchless `_update` Function**:
    *   Removed `if` statements checking for `timeElapsed > 0` and non-zero reserves.
    *   Implemented logic using **inline assembly** and bitwise operations to calculate accumulators without branching.

2.  **Branchless `swap` Function**:
    *   Replaced short-circuiting logical OR (`||`) with bitwise OR (`|`) in `require` statements to ensure both conditions are evaluated (avoiding branch jumps).
    *   Removed `if` checks before `_safeTransfer` calls. Transfers are attempted even if amounts are zero (though implementation of `_safeTransfer` or the underlying call handles it, the control flow at this level is linear).
    *   Replaced ternary operators for calculating `amount0In` and `amount1In` with inline assembly logic.

## Original Documentation

> Below is the original documentation from Uniswap V2 Core.

[![Actions Status](https://github.com/Uniswap/uniswap-v2-core/workflows/CI/badge.svg)](https://github.com/Uniswap/uniswap-v2-core/actions)
[![Version](https://img.shields.io/npm/v/@uniswap/v2-core)](https://www.npmjs.com/package/@uniswap/v2-core)

In-depth documentation on Uniswap V2 is available at [uniswap.org](https://uniswap.org/docs).

The built contract artifacts can be browsed via [unpkg.com](https://unpkg.com/browse/@uniswap/v2-core@latest/).

# Local Development

The following assumes the use of `node@>=10`.

## Install Dependencies

`yarn`

## Compile Contracts

`yarn compile`

## Run Tests

`yarn test`
