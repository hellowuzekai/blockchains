https://etherscan.io/address/0x7dc4f41294697a7903c4027f6ac528c5d14cd7eb#code


```
    function transferFrom(address from, address to, uint value) returns (bool success) {

        //checking account is freeze or not
        if (frozenAccount[msg.sender]) return false;

        //checking the from should have enough coins
        if(balances[from] < value) return false;

        //checking for allowance
        if( allowed[from][msg.sender] >= value ) return false;

        //checking for overflows
        if(balances[to] + value < balances[to]) return false;
        
        balances[from] -= value;
        allowed[from][msg.sender] -= value;
        balances[to] += value;
        
        // Notify anyone listening that this transfer took place
        Transfer(from, to, value);

        return true;
    }

```

In this contract, The 'if( allowed[from][msg.sender] >= value ) return false;' will cause an arbitrary transfer in function transferFrom because the '>=' instead of '<='. Attacker can transfer from any address to his address and does not need to meet the conditions of ‘allowed > value’.
