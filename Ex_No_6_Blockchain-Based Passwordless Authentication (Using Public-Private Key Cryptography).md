# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:
Step 1: User Registration
A user registers with their Ethereum public key (instead of a password).


Step 2: Login Process
When logging in, the user signs a random challenge message using their private key.


The smart contract verifies the signature using the user’s public key.



# Program:
#### Developed by: JAGADEESH P
#### Register number: 212223230083
#### Date: 28/04/2025
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordlessAuth {
    mapping(address => bool) public registeredUsers;

    event UserRegistered(address user);
    event UserAuthenticated(address user);

    function registerUser() public {
        require(!registeredUsers[msg.sender], "Already registered");
        registeredUsers[msg.sender] = true;
        emit UserRegistered(msg.sender);
    }

    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(registeredUsers[msg.sender], "User not registered");
        address signer = ecrecover(hash, v, r, s);
        return signer == msg.sender;
    }
}
```

# Expected Output:
![WhatsApp Image 2025-04-28 at 14 59 55_611321f6](https://github.com/user-attachments/assets/59ac4648-5263-482c-860c-28a029746043)
![WhatsApp Image 2025-04-28 at 15 00 00_6d08918e](https://github.com/user-attachments/assets/f725d1bc-6dfa-4fe0-ba62-53375b762257)
![WhatsApp Image 2025-04-28 at 15 00 10_b451feb1](https://github.com/user-attachments/assets/a36c9e9a-904a-4390-abdc-d22547ddaa4a)
![WhatsApp Image 2025-04-28 at 15 00 10_4d515622](https://github.com/user-attachments/assets/ce86464f-a93e-453a-a4ba-3044efbed303)




# High-Level Overview:
Eliminates password hacks & phishing attacks.


Uses Ethereum's built-in cryptographic functions.


Inspired by Web3 login solutions like MetaMask authentication.

# RESULT: 
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks is executed successfully
