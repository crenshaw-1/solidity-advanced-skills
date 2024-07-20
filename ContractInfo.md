# solidity-advanced-skills

## Introduction

- Protocol Name: BSC (BEP20)
- Category: RWA Tokenization
- Smart Contract Name: BEP20UpgradeableProxy (MANTRA-OM)

## Function Analysis

- Function Name: sendValue
- Block Explorer Link: [click here](https://bscscan.com/address/0xf78d2e7936f5fe18308a3b2951a93b6c4a41f5e2#code#L483)
- Function Code:

```solidity
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
```

Used Encoding/Decoding or Call Method: The `call` method is used here.

## Explanation

- Purpose: The `sendValue` function is used flexibly in transfering ETH from an address/contract to another address/contract (recipient).

- Detailed Usage: The `sendValue` function is utilized as a better and safer option for function calls involving transferring ETH. Since the original transfer function has a hard gas limit of 2300 gas units, which was initially implemented as a security measure, but due to the EIP-1884 (Ethereum Improvement Proposal 1884) the gas cost of certain opcodes increased, hence some contracts that previously worked fine with transfer might now fail because they exceed the 2300 gas limit. The `sendValue` function is designed, utilizing the `recipient.call` method send the amount of ETH prefilled by the sender and consequently forward all available gas to the recipient, rather than imposing a fixed limit, which allows for more complex operations to be performed by the receiving contract if necessary, and still maintaining safety by reverting transactions if any errors occur during the transfer.

- Impact: The `sendValue` function provides a more robust way of transferring tokens or value within smart contracts. For a platform dealing with real-world asset (RWA) tokenization, having secure and reliable methods for value transfer is crucial. This could contribute to the overall security and dependability of Mantra's blockchain infrastructure.

As Mantra aims to provide developers with tools to build RWA-centric protocols, having flexible and gas-efficient methods like sendValue could be beneficial. It allows for more complex operations in receiving contracts, which might be necessary when dealing with sophisticated RWA tokenization schemes.
