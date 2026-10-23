# 2024-04-dyad-338: Missing exogenous collateral check in liquidation

## Summary
The `VaultManagerV2::liquidate` flow does not verify that the vault has enough non-kerosene collateral before allowing liquidation. This creates a scenario where a vault can remain undercollateralized in terms of exogenous backing while still being treated as valid.

## Root cause
The liquidation check only looks at the collateral ratio and not whether the vault has enough non-kerosene collateral. As a result, a vault can have enough total collateral due to kerosene value but still be unsafe because it lacks sufficient exogenous collateral.

## Impact
A vault can avoid liquidation even when the DYAD minted exceeds the usable non-kerosene value. That can leave the protocol exposed, and the value of DYAD can be pressured downward by undercollateralized positions.

## Proof of concept
The report demonstrates a scenario where:
1. a user deposits ETH/WETH and wstETH collateral,
2. mints DYAD up to the non-kerosene value,
3. the WETH price drops,
4. the vault is no longer sufficiently backed by exogenous collateral,
5. liquidation reverts because the missing exogenous-collateral check is not enforced.

## Recommended fix
Require the liquidation condition to also ensure that the current non-kerosene collateral stays above the minted DYAD value before allowing the operation to proceed.

## Contest link
https://github.com/code-423n4/2024-04-dyad-findings/issues/338
