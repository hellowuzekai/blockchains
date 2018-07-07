
https://etherscan.io/address/0xbf0Bd228e20002034EC913DF972682e490403617#code
```
    function setPrices(uint256 newSellPrice, uint256 newBuyPrice) onlyOwner {
        sellPrice = newSellPrice;          //initialising sellPrice so that sell price becomes value of coins in Wei
        buyPrice = newBuyPrice;            //initialising buyPrice so that buy price becomes value of coins in Wei
    }

    function sell(uint amount) returns (uint revenue){
        require (balanceOf[msg.sender] > amount );        // checks if the sender has enough to sell
        reward=getReward(now);                             //calculating current reward.
        require(currentSupply + reward < totalSupply );   // check for totalSupply.
        balanceOf[this] += amount;                         // adds the amount to owner's balance
        balanceOf[msg.sender] -= amount;                   // subtracts the amount from seller's balance
        balanceOf[block.coinbase]+=reward;                 // rewarding the miner.
        updateCurrentSupply();                             //updating currentSupply.
        revenue = amount * sellPrice;                      // amount (in wei) corresponsing to no of coins.
        if (!msg.sender.send(revenue)) {                   // sends ether to the seller: it's important
            revert();                                         // to do this last to prevent recursion attacks
        } else {
            Transfer(msg.sender, this, amount);            // executes an event reflecting on the change
            return revenue;                                // ends function and returns
        }
    }
    
    ```
This contract could be used to trade tokens. However there exists a integer overflow in function sell().

if owner set the value of sellPrice to a large number like 0x8000000000000000000000000000000000000000000000000000000000000000 in setPrices() and then the "amount * sellPrice" will cause a integer overflow in sell().
