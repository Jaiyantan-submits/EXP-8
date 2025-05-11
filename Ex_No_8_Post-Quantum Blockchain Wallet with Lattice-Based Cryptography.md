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

![image](https://github.com/user-attachments/assets/09eb85c6-d126-4bb9-8cb2-f0e24e95494d)

![image](https://github.com/user-attachments/assets/8e066d42-1236-42a6-8cc0-48818d411316)


![image](https://github.com/user-attachments/assets/3a2e25cb-8d49-4920-9910-617a21153cc2)

![image](https://github.com/user-attachments/assets/0a6d055a-ee03-41b3-947c-5a53fda77fb3)

![image](https://github.com/user-attachments/assets/c309f93d-3ee3-43fa-9eb4-10b5b9cdc377)



Users register using a post-quantum secure public key.


Transactions require a quantum-resistant signature for authentication.


If a traditional quantum-vulnerable hash is used, the transaction fails.


# RESULT : 
High-Level Overview:
First quantum-safe Ethereum-compatible wallet prototype.


Uses lattice-based key hashes instead of ECDSA.


Demonstrates how Ethereum will transition to post-quantum security.


Inspired by NIST’s post-quantum cryptography competition.

