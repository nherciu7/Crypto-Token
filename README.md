![image](https://github.com/nherciu7/Crypto-Token/assets/170099479/f0ed1a97-4e9f-46c2-8c0b-3fe41ed8b6e4)

![image](https://github.com/nherciu7/Crypto-Token/assets/170099479/a355a641-2902-4b24-bff6-eff502a1ae04)

# Token Project

This project implements a simple token contract in Motoko.

## Overview

The `Token` actor defines a basic token with the following features:

- Total supply of 1,000,000,000 tokens
- Balance tracking
- Basic transfer functionality
- Initial distribution of tokens to the contract owner
- Claiming mechanism for new users

## Dependencies

The project imports the following modules from the Motoko base library:

- `Principal`
- `HashMap`
- `Debug`
- `Nat`
- `Iter`

## Actor Implementation

### State Variables

- `owner`: The initial owner of all tokens.
- `totalSupply`: The total supply of the tokens.
- `symbol`: The symbol of the token.
- `balanceEntries`: A stable variable to store balance entries for upgrade purposes.
- `balances`: A hash map to store the balances of each principal.

### Methods

#### `balanceOf(who: Principal): async Nat`

Returns the balance of the specified principal.

#### `getSymbol(): async Text`

Returns the symbol of the token.

#### `payOut(): async Text`

Allows a caller to claim tokens if they haven't done so before. They receive 10,000 tokens upon claiming.

#### `transfer(to: Principal, amount: Nat): async Text`

Transfers the specified amount of tokens from the caller to the specified principal. Returns "Success" if the transfer is successful and "Insufficient Funds" if the caller does not have enough tokens.

### System Methods

#### `preupgrade()`

Prepares the contract for an upgrade by saving the current state of balances.

#### `postupgrade()`

Restores the state of balances after an upgrade.

## Usage

1. **Initialize the Contract**: Upon deployment, the contract initializes the balances with the owner holding all tokens.
2. **Check Balance**: Use `balanceOf` to check the balance of any principal.
3. **Transfer Tokens**: Use `transfer` to send tokens to another principal.
4. **Claim Tokens**: Use `payOut` to claim tokens if you are a new user.

## Example

```motoko
import Principal "mo:base/Principal";

let owner = Principal.fromText("lnc5u-bwybm-r5d56-dut5f-cuvhw-elmff-crjnq-3cu6p-jb2fo-xghum-4ae");
let token = Token();
token.getSymbol(); // "THEO"
token.balanceOf(owner); // 1000000000
token.payOut(); // "Succes" or "Already Claimed"
token.transfer(someOtherPrincipal, 500); // "Success" or "Insufficient Funds"
