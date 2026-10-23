# 2024-01-curves-4: Unauthorized access to setCurves

## Summary
The `FeeSplitter` contract exposes an unprotected `setCurves` function, allowing any user to replace the active `Curves` reference with a malicious contract.

## Root cause
The setter function is callable without access control, so an attacker can re-point the fee logic to a fake `Curves` contract.

## Impact
The attacker can manipulate the values used for fee calculations and inflate the amounts they can claim.

## Proof of concept
The report demonstrates the attack flow:
1. deploy a fake `Curves` contract,
2. call `setCurves` from an attacker account,
3. manipulate token balance and token supply values in the fake contract,
4. call `getClaimableFees`,
5. observe that the attacker receives inflated claimable rewards.

## Recommended fix
Restrict `setCurves` to `onlyOwner` or an approved manager role.

## Contest link
https://github.com/code-423n4/2024-01-curves-findings/issues/4
