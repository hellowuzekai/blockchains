https://etherscan.io/address/0x1f31D1168efE4bD22D00D31FC425e5BcB54c75E7#code

```
    // Withdraw
    function withdrawToFounders(uint256 amount) public onlyOwner {
    	uint256 amount_to_withdraw = amount * 1000000000000000; // 0.001 ETH
        if (this.balance < amount_to_withdraw) revert();
        amount_to_withdraw = amount_to_withdraw / foundersAddresses.length;
        uint8 i = 0;
        uint8 errors = 0;
        
        for (i = 0; i < foundersAddresses.length; i++) {
			if (!foundersAddresses[i].send(amount_to_withdraw)) {
				errors++;
			}
		}
    }

```
This contract could be used to trade tokens. However there exists a integer overflow in function withdrawToFounders().

if owner set the value of amount to a large number like 0x800000000000000000000000000000000000000000000000000 in withdrawToFounders() and then the "amount * 1000000000000000" will cause a integer overflow in withdrawToFounders().
