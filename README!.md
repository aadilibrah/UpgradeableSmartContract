# Upgradeable Smart Contract using EIP-1967 Proxy Pattern

## 📌 Overview

This project demonstrates a simplified implementation of the **EIP-1967 proxy pattern**, enabling smart contract **upgradability** by separating logic from storage.

## 🛠️ Proxy Pattern Used: EIP-1967

### 🔄 What is a Proxy Contract?

A proxy contract delegates all function calls to an implementation (logic) contract. This allows for the logic to be upgraded without changing the contract's address or state.

### 📖 About EIP-1967

EIP-1967 standardizes the storage slot used to store the implementation address. It uses the slot:


## - bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1)

This avoids storage conflicts between the proxy and logic contracts.

## 🧩 Components

### 🔹 Logic.sol
Contains the actual business logic (e.g., `setNumber`, `getNumber`). It is stateless and called via `delegatecall`.

### 🔹 Proxy.sol
Acts as the entry point. Stores the logic address in a fixed slot and forwards all calls using low-level `delegatecall`.

## ⚙️ How It Works

1. Deploy `Logic.sol`
2. Deploy `Proxy.sol` with Logic's address in the constructor
3. Interact with Proxy using Logic's ABI
4. Data is stored in Proxy; logic is executed from Logic contract

## 🔐 Security Considerations

- Uses the EIP-1967 reserved storage slot
- Constructor ensures non-zero logic address
- No admin upgradeability logic in this simplified version

## 🧪 Testing in Remix

1. Deploy `Logic.sol`
2. Deploy `Proxy.sol` with Logic's address
3. Go to Remix → "At Address" → paste Proxy address
4. Use Logic ABI to interact (e.g., call `setNumber(42)`, then `getNumber()`)


## 📚 References

- [EIP-1967](https://eips.ethereum.org/EIPS/eip-1967)
- [OpenZeppelin Upgrades](https://docs.openzeppelin.com/upgrades/2.3/)
- [Solidity Docs](https://docs.soliditylang.org/)

---

## 👨‍💻 Author

Aadil Ibrahim  
GitHub: [aadilibrah](https://github.com/aadilibrah)


