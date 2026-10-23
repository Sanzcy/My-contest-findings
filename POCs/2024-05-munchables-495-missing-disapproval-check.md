# 2024-05-munchables-495: Missing disapproval check allows simultaneous approval and disapproval of a price proposal

## Summary
A price feed can disapprove a proposal and then immediately approve the same price. The system is supposed to allow one vote action per user per proposal, not both.

## Root cause
`approveUSDPrice()` does not check whether the caller already disapproved the same proposal. This leaves a gap that enables conflicting votes from the same feed.

## Impact
A single price feed can effectively vote both ways on the same proposal, undermining the intended governance/validation logic and weakening price-update integrity.

## Proof of concept
The report provides a Foundry test where the price feed calls `disapproveUSDPrice()` followed by `approveUSDPrice()` against the same value and the same proposal. The test shows the duplicate behavior is possible.

## Recommended fix
Reject approval if the same user already recorded a disapproval for the same proposal, and enforce one-vote-per-proposal semantics.

## Contest link
https://github.com/code-423n4/2024-05-munchables-findings/issues/495
