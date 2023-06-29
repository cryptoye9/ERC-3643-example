# 🦖 ERC-3643 (T-REX) Token Standard Overview 🦖

## 📖 Table of Contents
1. [Introduction](#-introduction)
2. [🎯 Purpose of ERC-3643](#-purpose-of-erc-3643)
3. [🔐 Main Contracts](#-main-contracts)
    1. [💸 Token Contract](#-token-contract)
    2. [🆔 ONCHAINID Contract (Identity Registry)](#-onchainid-contract-identity-registry)
    3. [🔒 Compliance Contract](#-compliance-contract)
4. [Code Samples and Explanations](#-code-samples-and-explanations)
5. [Conclusion](#-conclusion)

## Introduction
**ERC-3643**, commonly known as **T-REX** (Token for Regulated EXchanges), is an Ethereum-based token standard specifically designed for the creation, issuance, and management of security tokens. Security tokens are blockchain-based assets that represent ownership in real-world assets such as stocks, real estate, or other investment contracts. T-REX standard was published on July 9, 2021.

## 🎯 Purpose of ERC-3643
The primary purpose of ERC-3643 is to provide a standardized and regulatory-compliant framework for security tokens on the Ethereum blockchain. It aims to facilitate the integration of blockchain technology in traditional financial systems. Key highlights of the standard include:

- **Standard Interfaces**: Ensuring interoperability amongst various systems and applications by providing standard interfaces for security tokens.
- **Regulatory Compliance**: Compliance with legal regulations is integral to the standard. It can enforce any compliance rule as required by the regulators or token issuers.
- **Identification System**: T-REX tokens must be used in conjunction with an on-chain identification system known as ONCHAINID.
- **Token Management**: Provides support for token recovery, freezing of tokens (fully or partially), minting, burning, and more.
- **Role Definitions**: Defines an Agent role and an Owner role (token issuer) and allows for forced transfers from an Agent wallet.
- **Upgradeability**: Allows for smart contract code upgrades without changing the token smart contract address.

## 🔐 Main Contracts
ERC-3643 T-REX standard comprises multiple smart contracts that work together to manage security tokens.

### 💸 Token Contract
The Token Contract is responsible for the basic functionalities of the security token. This includes:

- **Token Transfers**: Handling transfers of tokens between addresses.
- **Minting and Burning**: Creating new tokens or removing tokens from circulation.
- **Compliance Checks**: Ensuring that token transfers comply with the rules defined in the Compliance Contract.
- **Interacting with ONCHAINID**: Verifying the eligibility of participants by querying the Identity Registry.

### 🆔 ONCHAINID Contract (Identity Registry)
The ONCHAINID Contract is essential for identity verification. It includes:

- **Identity Records**: Maintaining records of participants' wallet addresses and associated identity claims.
- **Identity Verification**: Checking if a participant is eligible for a transaction based on their identity claims.
  
### 🔒 Compliance Contract
The Compliance Contract is either a separate contract or a part of the Token Contract that ensures regulatory compliance. It includes:

- **Compliance Rules**: Defining and enforcing compliance rules for token transfers.
- **Pre-checks**: Providing a standard interface to pre-check if a transfer will pass or fail before sending it to the blockchain.
- **Freezing Tokens**: Allowing to freeze tokens on the wallet of investors if needed, partially or totally.

## Code Samples and Explanations
This section provides code snippets and explanations for key functions in the ERC-3643 T-REX standard. Here is an example of how the `transfer` function can be implemented in the Token Contract:

```
function transfer(address _to, uint256 _amount) public override whenNotPaused returns (bool) {
    require(!frozen[_to] && !frozen[msg.sender], 'wallet is frozen');
    require(_amount <= balanceOf(msg.sender).sub(frozenTokens[msg.sender]), 'Insufficient Balance');
    if (tokenIdentityRegistry.isVerified(_to) && tokenCompliance.canTransfer(msg.sender, _to, _amount)) {
        tokenCompliance.transferred(msg.sender, _to, _amount);
        _transfer(msg.sender, _to, _amount);
        return true;
    }
    revert('Transfer not possible');
}
```

**Explanation**:
- The function checks if the sender and receiver wallets are not frozen.
- It ensures the sender has enough balance.
- The `isVerified` function checks if the receiver is whitelisted and verified on the Identity Registry.
- The `canTransfer` function checks if the transfer complies with global compliance rules.
- If all checks pass, the transfer is executed.

## Conclusion

ERC-3643 (T-REX) is an advanced token standard for security tokens on the Ethereum blockchain. It is built with a focus on regulatory compliance and interoperability. The standard comprises mainly of a Token Contract, an ONCHAINID Contract for identity verification, and a Compliance Contract for enforcing rules. This makes ERC-3643 a robust standard for integrating blockchain technology with traditional financial systems.

*For further details, please refer to the [official ERC-3643 documentation](https://eips.ethereum.org/EIPS/eip-3643).*
