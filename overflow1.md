
https://etherscan.io/address/0x3fe3D6f405b5858A320B33FbcB0Bea3b2C2eB7BE#code
```
        function setPrices(uint256 newSellPrice, uint256 newBuyPrice) onlyOwner {
            sellPrice = newSellPrice;
            buyPrice = newBuyPrice;
        }
        function sell(uint amount) returns (uint revenue){
            if (balanceOf[msg.sender] < amount ) throw;        // checks if the sender has enough to sell
            balanceOf[this] += amount;                         // adds the amount to owner's balance
            balanceOf[msg.sender] -= amount;                   // subtracts the amount from seller's balance
            revenue = amount * sellPrice;                      // calculate the revenue
            msg.sender.send(revenue);                          // sends ether to the seller
            Transfer(msg.sender, this, amount);                // executes an event reflecting on the change
            return revenue;                                    // ends function and returns
        }
    
```

This contract could be used to trade tokens. However there exists a integer overflow in function sell().

if owner set the value of sellPrice to a large number like 0x8000000000000000000000000000000000000000000000000000000000000000 in setPrices() and then the "amount * sellPrice" will cause a integer overflow in sell().
