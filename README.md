# ErrorHandlingExample Smart Contract

This repository contains a Solidity smart contract that demonstrates the use of `require()`, `assert()`, and `revert()` statements for error handling. These statements are essential for writing secure smart contracts on the Ethereum and Avalanche networks.

## Contract Overview

The `ErrorHandlingExample` contract includes the following features:
- Sets and retrieves a value with validation.
- Ensures only the contract owner can set the value and withdraw funds.
- Uses `require()` to validate conditions.
- Uses `assert()` to check for conditions that should always be true.
- Uses `revert()` to handle errors and provide custom error messages.
- Includes a fallback function to receive Ether.

## Prerequisites

To deploy and interact with this contract, you will need:
- [Remix IDE](https://remix.ethereum.org/) or any other Ethereum development environment.
- An Ethereum wallet like [MetaMask](https://metamask.io/).
- Some Ether or AVAX to deploy the contract.

## Deployment

1. Open Remix IDE or your preferred Ethereum development environment.
2. Copy and paste the contract code into a new Solidity file.
3. Compile the contract using the Solidity compiler version `0.8.0` or later.
4. Deploy the contract to the desired network (Ethereum or Avalanche).

## Contract Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ErrorHandlingExample {
    address public owner;
    uint256 public value;

    constructor() {
        owner = msg.sender; // Set the owner as the contract deployer
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    function setValue(uint256 _value) public onlyOwner {
        // Use require to check for a valid condition
        require(_value > 0, "Value must be greater than zero");
        value = _value;
    }

    function incrementValue(uint256 _increment) public {
        // Use assert to check for a condition that should always be true
        assert(value + _increment > value); // This should always hold true unless overflow occurs
        value += _increment;
    }

    function resetValue() public {
        // Use revert to handle an error and provide a custom error message
        if (value == 0) {
            revert("Value is already zero");
        }
        value = 0;
    }

    function withdraw() public onlyOwner {
        // Use require to ensure the contract has a balance to withdraw
        require(address(this).balance > 0, "No balance to withdraw");
        payable(owner).transfer(address(this).balance);
    }

    // Fallback function to receive Ether
    receive() external payable {}
}
