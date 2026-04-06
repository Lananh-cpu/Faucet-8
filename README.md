# Faucet-8
Faucet.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Faucet {
    uint256 public constant WITHDRAW_AMOUNT = 0.1 ether;
    uint256 public constant LOCK_TIME = 1 days;

    mapping(address => uint256) public lastWithdraw;

    function withdraw() public {
        require(block.timestamp >= lastWithdraw[msg.sender] + LOCK_TIME, "Please wait 24h");
        require(address(this).balance >= WITHDRAW_AMOUNT, "Faucet is empty");

        lastWithdraw[msg.sender] = block.timestamp;
        payable(msg.sender).transfer(WITHDRAW_AMOUNT);
    }

    receive() external payable {}
}
