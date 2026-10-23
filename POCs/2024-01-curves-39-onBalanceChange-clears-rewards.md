# 2024-01-curves-39: onBalanceChange clears unclaimed rewards

## Summary
The `onBalanceChange` path resets the fee offset for a user, which can cause previously earned but unclaimed rewards to be lost.

## Root cause
The function clears or overwrites the fee tracking state before the prior accrued rewards are properly credited.

## Impact
A user who buys or transfers the same token twice can lose earlier reward credits, leading to underpayment and incorrect reward accounting.

## Proof of concept
The report includes a Foundry test in which a user makes multiple purchases. The logs show that the first reward is cleared after later balance changes, and the final claimable value is lower than expected.

## Recommended fix
Update the fee credit before resetting the offset, or call the appropriate `updateFeeCredit` logic so previously owed rewards are carried forward instead of discarded.

## Contest link
https://github.com/code-423n4/2024-01-curves-findings/issues/39
