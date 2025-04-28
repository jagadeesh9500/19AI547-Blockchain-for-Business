# Experiment 4: DeFi Lending and Borrowing Protocol
# Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

# Algorithm:

Step 1: Users deposit ETH into the smart contract to earn interest over time.

Step 2: Users can borrow ETH by providing enough collateral (at least 150% of the borrowed amount).

Step 3: The contract checks that collateral is sufficient before allowing the loan.

Step 4: Interest is calculated dynamically based on how much ETH is borrowed compared to total deposits.

Step 5: If a borrower’s collateral value drops below the safe level (liquidation threshold), they can be liquidated.

Step 6: Liquidators can repay a borrower's debt and claim their collateral to maintain system stability.



# Program:
#### Developed by: jagadeesh P
#### Register number: 212223230083
#### Date: 28/04/2025

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {
    address public owner;
    uint256 public interestRate = 5; // 5% interest per cycle
    uint256 public liquidationThreshold = 150; // 150% collateralization
    mapping(address => uint256) public deposits;
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Deposited(address indexed user, uint256 amount);
    event Borrowed(address indexed user, uint256 amount, uint256 collateral);
    event Liquidated(address indexed user, uint256 debtRepaid, uint256 collateralSeized);

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        require(msg.value > 0, "Deposit must be greater than zero");
        deposits[msg.sender] += msg.value;
        emit Deposited(msg.sender, msg.value);
    }

    function borrow(uint256 amount) public payable {
        require(msg.value >= (amount * liquidationThreshold) / 100, "Not enough collateral");
        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;
        payable(msg.sender).transfer(amount);
        emit Borrowed(msg.sender, amount, msg.value);
    }

    function liquidate(address borrower) public {
        require(collateral[borrower] < (borrowed[borrower] * liquidationThreshold) / 100, "Not eligible for liquidation");
        uint256 debt = borrowed[borrower];
        uint256 seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;
        payable(msg.sender).transfer(seizedCollateral);
        emit Liquidated(borrower, debt, seizedCollateral);
    }
}

```

# Output :

### Borrow
![Screenshot 2025-04-28 144014](https://github.com/user-attachments/assets/e7e2d7a0-853e-452c-a139-740318a8002a)

### Liquidate
![Screenshot 2025-04-28 144032](https://github.com/user-attachments/assets/ee77b646-7e92-48c6-a8b7-695c2f9b7e05)

### ReduceCollateral
![Screenshot 2025-04-28 144044](https://github.com/user-attachments/assets/d9331898-d857-41c7-83d6-be9bfa39b9ba)

### Borrowed
![Screenshot 2025-04-28 144055](https://github.com/user-attachments/assets/ce392cbd-0556-44a2-9652-63c124f2846d)

### Collateral
![Screenshot 2025-04-28 144107](https://github.com/user-attachments/assets/ebda449d-3e8a-4521-9f31-737cafb5d4e2)

### Deposits
![Screenshot 2025-04-28 144117](https://github.com/user-attachments/assets/f653e021-cfac-46be-8d8c-1e3768afc0c9)

### Interest rate
![Screenshot 2025-04-28 144128](https://github.com/user-attachments/assets/4a192789-731f-4414-85fa-8a47f2e08c59)

### Liquidation threshold call
![Screenshot 2025-04-28 144138](https://github.com/user-attachments/assets/52625466-d23d-40c7-a7f6-64db72e8b374)

### Owner
![Screenshot 2025-04-28 144151](https://github.com/user-attachments/assets/d00bad4e-8148-4f45-8277-1facb98866c0)

# RESULT : 

Thus, to build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi is executed successfully.
