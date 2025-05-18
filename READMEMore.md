# Upgradeable Smart Contract Using EIP-1967 Proxy Standard

## Introduction

Smart contracts are immutable by default once deployed on the blockchain. However, in real-world applications, there is often a need to fix bugs, add new features, or upgrade logic. This is where **proxy patterns** come into play. They enable **upgradability** by decoupling contract logic from its data storage.

This project explores the **EIP-1967 Proxy Standard**, a widely-used approach to writing upgradeable contracts in a secure and standardized manner.

---

## What is EIP-1967?

**EIP-1967** is an Ethereum Improvement Proposal that defines a standard for storing proxy-related variables (like the address of the implementation contract) in specific storage slots to prevent collisions with the logic contract's state variables.

It builds on the concept of the **Transparent Proxy Pattern**, where:
- The **Proxy Contract** delegates function calls to a **Logic (Implementation) Contract** using `delegatecall`.
- State variables are stored in the Proxy.
- Logic is defined and upgraded in a separate contract.

This separation ensures contracts remain upgradable without changing the address users interact with.

---

## How It Works

### Components
1. **Proxy Contract**: 
   - Stores the implementation contract’s address.
   - Uses `delegatecall` to forward user calls to the logic contract.

2. **Logic (Implementation) Contract**:
   - Contains the actual business logic (e.g., setting a value).

3. **Storage Slot**:
   - A specific key is used to store the implementation address:
     ```solidity
     bytes32 private constant IMPLEMENTATION_SLOT = keccak256("eip1967.proxy.implementation") - 1;
     ```

### Delegatecall Behavior
- `delegatecall` runs the logic of the called contract **in the context of the caller**.
- This means all state changes happen in the Proxy’s storage, not the Logic’s.

---

## Example Scenario

1. Deploy `Logic.sol`, which contains the function `setNumber(uint256)`.
2. Deploy `Proxy.sol`, passing the logic contract’s address to the constructor.
3. Interact with the Proxy using the ABI of the Logic contract.
4. The function calls are delegated to Logic, but the state is saved in Proxy’s storage.

---

## Why Use EIP-1967?

- **Prevents storage collision** with a predefined slot.
- **Compatible with OpenZeppelin Upgrades** plugin.
- Makes proxy-based upgrades safer and more standardized.
- Transparent to users — the contract address remains unchanged.

---

## Security Considerations

- Only trusted parties (e.g., an admin contract or governance mechanism) should be allowed to perform upgrades.
- Avoid directly exposing upgrade functions to external users.
- Validate the logic contract before upgrading.
- Avoid introducing storage layout incompatibilities between versions.

---

## Project Structure

