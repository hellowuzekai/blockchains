# blockchains

https://etherscan.io/address/0x9107C1B28d775E59f98BF3f4dE3b959816CF5526#code

```
    function setPrices(uint256 newSellPrice, uint256 newBuyPrice) onlyOwner public {
        sellPrice = newSellPrice;
        buyPrice = newBuyPrice;
    }
    function sell(uint256 amount) public {
        require(this.balance >= amount * sellPrice);      // checks if the contract has enough ether to buy
        _transfer(msg.sender, this, amount);              // makes the transfers
        msg.sender.transfer(amount * sellPrice);          // sends ether to the seller. It's important to do this last to avoid recursion attacks
    }
```
This contract could be used to trade tokens. However there exists a integer overflow in function sell().

if owner set the value of sellPrice to a large number like 0x8000000000000000000000000000000000000000000000000000000000000000 in setPrices() and then the "amount * sellPrice" will cause a integer overflow in sell().

