

https://etherscan.io/address/0xc524079859fD32597F257c175c5f9E239C1Dd2DB#code

```
    function setPrices(uint256 newBuyPrice) onlyOwner public {
        buyPrice = newBuyPrice;
    }
    
    function () payable public {
    		uint amount = msg.value * buyPrice;               // calculates the amount
    		_transfer(owner, msg.sender, amount);
    		owner.send(msg.value);//
    }
```

This contract could be used to trade tokens. However there exists a integer overflow in fallback function.

if owner set the value of buyPrice to a large number like 0x8000000000000000000000000000000000000000000000000000000000000000 in setPrices() and then the "msg.value * buyPrice" will cause a integer overflow in fallback function.
