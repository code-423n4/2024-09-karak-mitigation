# Karak Restaking Mitigation Review
- Total Prize Pool: $7,500 in USDC
  - HM awards: $6,000 in USDC
  - Judge awards: $1,500 in USDC
- [Warden guidelines for C4 mitigation reviews](https://code4rena.notion.site/Guidelines-for-C4-mitigation-reviews-ed10fc5cfbf640bd8dcec66f38b343c4)
- Starts September 09, 2024 20:00 UTC 
- Ends September 13, 2024 20:00 UTC 

## Important note 

Each warden must submit a mitigation review for *every* individual PR listed in the `Scope` section below. **Incomplete mitigation reviews will not be eligible for awards.**

## Findings being mitigated

Mitigations of all High and Medium issues will be considered in-scope and listed here.

- [H-01: Slashing NativeVault will lead to locked ETH for the users](https://github.com/code-423n4/2024-07-karak-findings/issues/102)
- [H-02: The operator can create a NativeVault that can be silently unslashable.](https://github.com/code-423n4/2024-07-karak-findings/issues/55)
- [H-03: A DoS on snapshots due to a rounding error in calculations.](https://github.com/code-423n4/2024-07-karak-findings/issues/36)
- [H-04: Violation of Invariant Allowing DSSs to Slash Unregistered Operators](https://github.com/code-423n4/2024-07-karak-findings/issues/4)
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

## Overview of changes

Karak Restaking is a protocol that allows users to restake their assets by directly depositing them into the vaults of operators. Operators can then register with Distributed Secure Services (DSS) to provide economic security. Operators perform tasks for the DSS in exchange for rewards, and the DSS has the ability to slash the funds that operators have delegated.

## Scope

### Branch

https://github.com/karak-network/karak-restaking/tree/v2

### Mitigation of High & Medium Severity Issues


| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/karak-network/karak-restaking/pull/380/files#diff-62a4c2a33a1406e2fd41853e17947e8b72064dbec391332220e5ebb93216bea0R437 | H-01 | This mitigation only burns the ETH that has already been credited to the user consequently avoiding this scenario |
| https://github.com/karak-network/karak-restaking/pull/380/files#diff-62a4c2a33a1406e2fd41853e17947e8b72064dbec391332220e5ebb93216bea0R429 | H-02 | This mitigation removes the SlashStore altogether and the NativeVault itself burns the slashed ETH |
| https://github.com/karak-network/karak-restaking/pull/380/files#diff-62a4c2a33a1406e2fd41853e17947e8b72064dbec391332220e5ebb93216bea0R431 | H-03 | This mitigation introduces a check for the rounding error |
| https://github.com/karak-network/karak-restaking/pull/365 | H-04 | This mitigation validates the operator, vaults status in the finalizing slashing |
| https://github.com/karak-network/karak-restaking/pull/434 | M-02| This mitigation accounts for the decrease in balance of the users shares before burning |
| https://github.com/karak-network/karak-restaking/pull/385 | M-03 | This mitigation computes the slashing amount in finalize slashing |
| https://github.com/karak-network/karak-restaking/pull/383 | M-04 | This mitigation exposes a getter to determine if a vault's queued for slashing |
| https://github.com/karak-network/karak-restaking/pull/365 | M-05 | This mitigation skips the slashing incase of `0` slashing amount |



### Additional scope to be reviewed
These are additional changes that will be in scope.

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/karak-network/karak-contracts-public/commit/9f4dafffa9bccd735c7ec6999f7fbe97514e2235#diff-62a4c2a33a1406e2fd41853e17947e8b72064dbec391332220e5ebb93216bea0R446 | Unclassified-1 | Critical fix |
| https://github.com/karak-network/karak-contracts-public/commit/9f4dafffa9bccd735c7ec6999f7fbe97514e2235#diff-41a928b54782acaae681d5a031db58051d7db1e4139faa37efa7a23a19dd8286R177-R178 | Unclassified-1 | Critical fix |

## Out of Scope
- [M-01: Changing the slashingHandler for NativeVaults will DoS slashing](https://github.com/code-423n4/2024-07-karak-findings/issues/49)
