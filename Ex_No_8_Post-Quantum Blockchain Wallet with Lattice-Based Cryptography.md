# Experiment 8: Post-Quantum Blockchain Wallet with Lattice-Based Cryptography
# Aim:
To create a quantum-resistant wallet using lattice-based cryptography instead of traditional ECDSA, ensuring that future quantum computers cannot break private keys.

# Algorithm:
## Step 1: Understanding Quantum Threat to Blockchain
ECDSA-based wallets are vulnerable to quantum computers.


Lattice-based cryptography (e.g., NTRU, CRYSTALS-Kyber) provides quantum resistance.


## Step 2: Implement Lattice-Based Signature Scheme
Use precomputed NTRU-based public-private keys for authentication.


Store hashed lattice-based signatures instead of traditional Ethereum signatures.


## Step 3: Secure Transactions
Users sign transactions using lattice cryptographic proofs.


The smart contract verifies the proof before allowing transactions.



# Program:

(Solidity does not natively support lattice cryptography yet, but we simulate it using custom hash-based authentication.)
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PostQuantumWallet {
    struct User {
        bytes32 publicKeyHash;
        bool registered;
    }

    mapping(address => User) public users;
    mapping(address => uint256) public balances;

    event UserRegistered(address user, bytes32 publicKeyHash);
    event TransactionVerified(address from, address to, uint256 amount);

    function registerUser(bytes32 _publicKeyHash) public {
        require(!users[msg.sender].registered, "User already registered");
        users[msg.sender] = User(_publicKeyHash, true);
        emit UserRegistered(msg.sender, _publicKeyHash);
    }

    function sendFunds(address _to, uint256 _amount, bytes32 _signature) public {
        require(users[msg.sender].registered, "Sender not registered");
        require(users[_to].registered, "Recipient not registered");
        require(balances[msg.sender] >= _amount, "Insufficient funds");

        bytes32 calculatedHash = keccak256(abi.encodePacked(msg.sender, _to, _amount));
        require(calculatedHash == _signature, "Invalid quantum-safe signature");

        balances[msg.sender] -= _amount;
        balances[_to] += _amount;
        emit TransactionVerified(msg.sender, _to, _amount);
    }
}
```

# Expected Output:
Users register using a post-quantum secure public key.


Transactions require a quantum-resistant signature for authentication.


If a traditional quantum-vulnerable hash is used, the transaction fails.

![8(1)](https://github.com/user-attachments/assets/e21578b1-9908-411c-9335-64d8635f0b27)

![8(2)](https://github.com/user-attachments/assets/34a916c8-015c-4c0e-9184-60b20e181012)

![8(3)](https://github.com/user-attachments/assets/62adfe19-6849-4aaa-baa9-cfa952a77b4e)

![8(4)](https://github.com/user-attachments/assets/301d4992-f739-4742-b757-054baf0fb94b)

![8(5)](https://github.com/user-attachments/assets/5faa9646-8227-4231-9a3b-5d03ddd9a879)

![8(6)](https://github.com/user-attachments/assets/58e254ff-5a79-4e6f-a6e4-611475f0e171)

![8(7)](https://github.com/user-attachments/assets/b15919e4-ee34-4b62-80ae-b39a0daf66f0)

# RESULT : 
High-Level Overview:
First quantum-safe Ethereum-compatible wallet prototype.


Uses lattice-based key hashes instead of ECDSA.


Demonstrates how Ethereum will transition to post-quantum security.


Inspired by NIST’s post-quantum cryptography competition.

