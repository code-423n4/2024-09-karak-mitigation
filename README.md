# Karak Restaking Mitigation Review
- Total Prize Pool: $7,500 in USDC
  - HM awards: $6,000 in USDC
  - Judge awards: $1,500 in USDC
- [Warden guidelines for C4 mitigation reviews](https://code4rena.notion.site/Guidelines-for-C4-mitigation-reviews-ed10fc5cfbf640bd8dcec66f38b343c4)
- Starts September 10, 2024 20:00 UTC 
- Ends September 16, 2024 20:00 UTC 

## Important note 

Each warden must submit a mitigation review for *every* individual PR listed in the `Scope` section below. **Incomplete mitigation reviews will not be eligible for awards.**

## Findings being mitigated

Mitigations of all High and Medium issues (+ Additional Issues to be mitigated) will be considered in-scope and listed here.

- [H-01: Slashing NativeVault will lead to locked ETH for the users](https://github.com/code-423n4/2024-07-karak-findings/issues/102)
- [H-02: The operator can create a NativeVault that can be silently unslashable.](https://github.com/code-423n4/2024-07-karak-findings/issues/55)
- [H-03: A DoS on snapshots due to a rounding error in calculations.](https://github.com/code-423n4/2024-07-karak-findings/issues/36)
- [H-04: Violation of Invariant Allowing DSSs to Slash Unregistered Operators](https://github.com/code-423n4/2024-07-karak-findings/issues/4)
- [M-02: A snapshot may face a permanent DoS if both a slashing event occurs in the NativeVault and the staker's validator is penalized.](https://github.com/code-423n4/2024-07-karak-findings/issues/31)
- [M-03: When malicious behavior occurs and DSS requests slashing against vault during 2 day period after SLASHING_WINDOW of 7 days is passed after staker initiates a withdrawal, token amount to be slashed is calculated to be higher than what it should be](https://github.com/code-423n4/2024-07-karak-findings/issues/17)
- [M-04: Delayed Slashing Window and Lack of Transparency for Pending Slashes Could Lead to Loss of Funds](https://github.com/code-423n4/2024-07-karak-findings/issues/15)
- [M-05: Slashing’s will Always Fail In Some Cases](https://github.com/code-423n4/2024-07-karak-findings/issues/7)

Additional issues to be mitigated:
- [ADD-01]: packages/contracts/src/NativeVault.sol L446 `+ node.withdrawableCreditedNodeETH -= slashedWithdrawable;`
- [ADD-02]: packages/contracts/src/entities/NativeVaultLib.sol L177-L178
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
| https://github.com/karak-network/karak-arena-mitigations/commit/fdef9d25e2b7c0a528d5a6dfcce64a3a518165af#diff-940446432243a929cd0f5ea691c4e90d60ee655723e2d5d8fcafc7b7504cfe98R437 | H-01 | This mitigation only burns the ETH that has already been credited to the user consequently avoiding this scenario |
| https://github.com/karak-network/karak-arena-mitigations/commit/fdef9d25e2b7c0a528d5a6dfcce64a3a518165af#diff-940446432243a929cd0f5ea691c4e90d60ee655723e2d5d8fcafc7b7504cfe98R429 | H-02 | This mitigation removes the SlashStore altogether and the NativeVault itself burns the slashed ETH |
| https://github.com/karak-network/karak-arena-mitigations/commit/fdef9d25e2b7c0a528d5a6dfcce64a3a518165af#diff-940446432243a929cd0f5ea691c4e90d60ee655723e2d5d8fcafc7b7504cfe98R216 | H-03 | This mitigation introduces a check for the rounding error |
| https://github.com/karak-network/karak-arena-mitigations/commit/69644a7b1c3607aea5f876d9ee6be24035c1d9d2 | H-04 | This mitigation validates the operator, vaults status in the finalizing slashing |
| https://github.com/karak-network/karak-arena-mitigations/commit/af49375f2f7682b6372477d68d367f2dee4256ca | M-02| This mitigation accounts for the decrease in balance of the users shares before burning |
| https://github.com/karak-network/karak-arena-mitigations/commit/71b4d9609b441072d2e0da62d67d6ad6cec0e550 | M-03 | This mitigation computes the slashing amount in finalize slashing |
| https://github.com/karak-network/karak-arena-mitigations/commit/a153880bfe7077b9f15c999297f8dbb582df1e09 | M-04 | This mitigation exposes a getter to determine if a vault's queued for slashing |
| https://github.com/karak-network/karak-arena-mitigations/commit/69644a7b1c3607aea5f876d9ee6be24035c1d9d2| M-05 | This mitigation skips the slashing incase of `0` slashing amount |



### Additional scope to be reviewed
These are additional changes that will be in scope.

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/karak-network/karak-arena-mitigations/commit/4b6a05e9b82e7e56901f5bb70f4095c48656083e#diff-940446432243a929cd0f5ea691c4e90d60ee655723e2d5d8fcafc7b7504cfe98R446 | ADD-01 | Critical fix |
| https://github.com/karak-network/karak-arena-mitigations/commit/4b6a05e9b82e7e56901f5bb70f4095c48656083e#diff-43f1883e39f28737075e02ba241ac1800e383631a5c788709a0e108da40e9159R177-R178 | ADD-02 | Critical fix |

## Out of Scope
- [M-01: Changing the slashingHandler for NativeVaults will DoS slashing](https://github.com/code-423n4/2024-07-karak-findings/issues/49)
