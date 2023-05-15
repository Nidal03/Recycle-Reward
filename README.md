# Recycle-Reward
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract RecyclingRewards {
    // Variables
    mapping(address => uint256) public userPoints;
    mapping(address => uint256) public userTokens;
    mapping(string => uint256) public itemPoints;
    mapping(string => uint256) public itemTokens;
    mapping(string => bool) public acceptedItems;
    address public owner;
    
    // Events
    event ItemAccepted(string item);
    event ItemRejected(string item);
    event PointsIssued(address user, uint256 points);
    event TokensIssued(address user, uint256 tokens);
    
    // Constructor
    constructor() {
        owner = msg.sender;
        itemPoints["glass"] = 10;
        itemPoints["plastic"] = 5;
        itemPoints["aluminum"] = 3;
        itemPoints["cardboard"] = 5;
        itemPoints["paper"] = 2;
        itemTokens["glass"] = 10;
        itemTokens["plastic"] = 5;
        itemTokens["aluminum"] = 3;
        itemTokens["cardboard"] = 5;
        itemTokens["paper"] = 2;
        acceptedItems["glass"] = true;
        acceptedItems["plastic"] = true;
        acceptedItems["aluminum"] = true;
        acceptedItems["cardboard"] = true;
        acceptedItems["paper"] = true;
    }
    
    // Functions
    function acceptItem(string memory item) public {
        require(acceptedItems[item], "Item not accepted");
        emit ItemAccepted(item);
    }
    
    function rejectItem(string memory item) public {
        require(acceptedItems[item], "Item not accepted");
        emit ItemRejected(item);
    }
    
    function issuePoints(address user, string memory item) public {
        require(acceptedItems[item], "Item not accepted");
        uint256 points = itemPoints[item];
        userPoints[user] += points;
        emit PointsIssued(user, points);
    }
    
    function issueTokens(address user, string memory item) public {
        require(acceptedItems[item], "Item not accepted");
        uint256 tokens = itemTokens[item];
        userTokens[user] += tokens;
        emit TokensIssued(user, tokens);
    }
    
    function redeemPoints(address user, uint256 amount) public {
        require(userPoints[user] >= amount, "Insufficient points");
        userPoints[user] -= amount;
        // TODO: Implement redemption logic
    }
    
    function redeemTokens(address user, uint256 amount) public {
        require(userTokens[user] >= amount, "Insufficient tokens");
        userTokens[user] -= amount;
        // TODO: Implement redemption logic
    }
    
    function updateAcceptedItems(string memory item, bool accepted) public {
        require(msg.sender == owner, "Unauthorized");
        acceptedItems[item] = accepted;
    }
}
