# 2024-01-salty-614: First LP can claim all initial pool rewards

## Summary
The first liquidity provider in an empty staking pool can capture the entire reward balance because there is no virtual reward accounting when `totalShares` is zero.

## Root cause
When the pool is empty, `_increaseUserShare()` skips the `virtualRewards` accounting branch. That means the first staker gets a full share of rewards without the usual deduction model that prevents early capture.

## Impact
The first LP can claim a disproportionate amount of the protocol's bootstrap rewards, effectively capturing all current pool rewards in the staking contract.

## Proof of concept
The contest report shows a Foundry test in which:
1. the pool has no prior shares,
2. upkeep is triggered to send rewards into the staking contract,
3. the first user deposits liquidity,
4. the user reward calculation returns the total pool rewards,
5. the user claims all rewards without the expected virtual reward offset.

## Recommended fix
Ensure virtual rewards are initialized or accounted for even in the first-user case, and/or delay or rework initial reward distribution so early depositors cannot claim the full pool reward set.

## Contest link
https://github.com/code-423n4/2024-01-salty-findings/issues/614
