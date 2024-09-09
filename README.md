# Karak Restaking Mitigation Review
- Total Prize Pool: $7,750 in USDC
  - HM awards: $6,000 in USDC
  - Judge awards: $1,500 in USDC
- [Warden guidelines for C4 mitigation reviews](https://code4rena.notion.site/Guidelines-for-C4-mitigation-reviews-ed10fc5cfbf640bd8dcec66f38b343c4)
- Starts September 09, 2024 20:00 UTC 
- Ends September 13, 2024 20:00 UTC 

## Important note 

Each warden must submit a mitigation review for *every* individual PR listed in the `Scope` section below. **Incomplete mitigation reviews will not be eligible for awards.**

## Findings being mitigated

Mitigations of all High and Medium issues will be considered in-scope and listed here.

- [H-04: Slashing NativeVault will lead to locked ETH for the users](https://github.com/code-423n4/2024-07-karak-findings/issues/102)
- [H-02: The operator can create a NativeVault that can be silently unslashable.](https://github.com/code-423n4/2024-07-karak-findings/issues/55)
- [H-03: A DoS on snapshots due to a rounding error in calculations.](https://github.com/code-423n4/2024-07-karak-findings/issues/36)
- [H-04: Violation of Invariant Allowing DSSs to Slash Unregistered Operators](https://github.com/code-423n4/2024-07-karak-findings/issues/4)
- [M-01: Changing the slashingHandler for NativeVaults will DoS slashing](https://github.com/code-423n4/2024-07-karak-findings/issues/49)
- [M-02: A snapshot may face a permanent DoS if both a slashing event occurs in the NativeVault and the staker's validator is penalized.](https://github.com/code-423n4/2024-07-karak-findings/issues/31)
- [M-03: When malicious behavior occurs and DSS requests slashing against vault during 2 day period after SLASHING_WINDOW of 7 days is passed after staker initiates a withdrawal, token amount to be slashed is calculated to be higher than what it should be](https://github.com/code-423n4/2024-07-karak-findings/issues/17)
- [M-04: Delayed Slashing Window and Lack of Transparency for Pending Slashes Could Lead to Loss of Funds](https://github.com/code-423n4/2024-07-karak-findings/issues/15)
- [M-05: Slashing’s will Always Fail In Some Cases](https://github.com/code-423n4/2024-07-karak-findings/issues/7)

New found issues post audit:
- [Unclassified-1]: packages/contracts/src/NativeVault.sol L446 `+ node.withdrawableCreditedNodeETH -= slashedWithdrawable;`
- [Unclassified-2]: packages/contracts/src/entities/NativeVaultLib.sol L177-L178
```
  validatorDetails.lastBalanceUpdateTimestamp =
            node.currentSnapshotTimestamp == 0 ? node.lastSnapshotTimestamp : node.currentSnapshotTimestamp;
```


[ ⭐️ SPONSORS ADD INFO HERE ]

## Overview of changes

Please provide context about the mitigations that were applied if applicable and identify any areas of specific concern.

## Scope

### Branch
[ ⭐️ SPONSORS ADD A LINK TO THE BRANCH IN YOUR REPO CONTAINING ALL PRS ]

### Mitigation of High & Medium Severity Issues
[ ⭐️ SPONSORS ADD ALL RELEVANT PRs TO THE TABLE BELOW:]

Wherever possible, mitigations should be provided in separate pull requests, one per issue. If that is not possible (e.g. because several audit findings stem from the same core problem), then please link the PR to all relevant issues in your findings repo. 

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/karak-network/karak-restaking/pull/365 | H-04 | This mitigation validates the operator, vaults status in the finalizing slashing |
| https://github.com/karak-network/karak-restaking/pull/385 | M-03 | This mitigation computes the slashing amount in finalize slashing |
| https://github.com/karak-network/karak-restaking/pull/383 | M-04 | This mitigation exposes a getter to determine if a vault's queued for slashing |
| https://github.com/karak-network/karak-restaking/pull/365 | M-05 | This mitigation skips the slashing incase of `0` slashing amount |
### Additional scope to be reviewed
[ ⭐️ CAS PLEASE REMOVE THIS SECTION IF THE SPONSOR IS ONLY MITIGATING HMS]

[ ⭐️ SPONSORS ADD ALL RELEVANT PRs TO THE TABLE BELOW:]

These are additional changes that will be in scope.

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/karak-network/karak-contracts-public/commit/9f4dafffa9bccd735c7ec6999f7fbe97514e2235#diff-62a4c2a33a1406e2fd41853e17947e8b72064dbec391332220e5ebb93216bea0R446 | Unclassified-1 | Critical fix |
| https://github.com/karak-network/karak-contracts-public/commit/9f4dafffa9bccd735c7ec6999f7fbe97514e2235#diff-41a928b54782acaae681d5a031db58051d7db1e4139faa37efa7a23a19dd8286R177-R178 | Unclassified-1 | Critical fix |

## Out of Scope

Please list any High and Medium issues that were judged as valid but you have chosen not to fix.
