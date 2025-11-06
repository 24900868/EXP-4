# Experiment 4: DeFi Lending and Borrowing Protocol 

# Aim: 
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi. 

# Algorithm: 
Step 1: Setup Lending and Borrowing Mechanism 
1. Users deposit ETH into the contract as liquidity. 
2. Depositors receive interest based on their deposits. 
3. Borrowers can borrow ETH but must provide collateral (e.g., 150% of the borrowed amount). 
4. Interest on borrowed funds is calculated dynamically based on utilization rate. 
Step 2: Implement Overcollateralization 
5. If a borrower’s collateral value drops below a certain liquidation threshold, their collateral is liquidated 
to repay the debt. 
Step 3: Allow Liquidation 
6. If collateral < liquidation threshold, liquidators can repay the borrower's debt and claim their collateral 
at a discount. 
 
# Code: 
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
    event Liquidated(address indexed user, uint256 debtRepaid, uint256 
collateralSeized); 
 
    constructor() { 
        owner = msg.sender; 
    } 
 
    function deposit() public payable { 
        require(msg.value > 0, "Deposit must be greater than zero"); 
        deposits[msg.sender] += msg.value; 
        emit Deposited(msg.sender, msg.value); 
    } 
 
    function borrow(uint256 amount) public payable { 
        require(msg.value >= (amount * liquidationThreshold) / 100, "Not enough 
collateral"); 
        borrowed[msg.sender] += amount; 
        collateral[msg.sender] += msg.value; 
        payable(msg.sender).transfer(amount); 
        emit Borrowed(msg.sender, amount, msg.value); 
    } 
 
    function liquidate(address borrower) public { 
        require(collateral[borrower] < (borrowed[borrower] * 
liquidationThreshold) / 100, "Not eligible for liquidation"); 
        uint256 debt = borrowed[borrower]; 
        uint256 seizedCollateral = collateral[borrower]; 
 
        borrowed[borrower] = 0; 
        collateral[borrower] = 0; 
        payable(msg.sender).transfer(seizedCollateral); 
        emit Liquidated(borrower, debt, seizedCollateral); 
    } 
} 
``` 
 
# Expected Output: 
● Users can deposit ETH and earn interest. 
 
● Users can borrow ETH by providing collateral. 
 
● If collateral < 150% of borrowed amount, liquidators can seize the collateral.

# Output :

<img width="1469" height="994" alt="image" src="https://github.com/user-attachments/assets/0c2924d3-240a-4dba-8586-b50b2dcfad2b" />


# Result :
Thus the code is compiled and verified successfully.
