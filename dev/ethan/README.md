# Ethan Kim

- Blog  : [hackability.kr](http://hackability.kr)
- Email : <hackability@i2sec.co.kr>

## Dev issues

- UnhandledPromiseRejectionWarning: Error: The contract code couldn't be stored, please check your gas limit.
  - `https://hanezu.github.io/posts/Gas-Limit-Error-when-deploy-Truffle-project-on-Private-Network.html`

```js
// original code
const result = await new web3.eth.Contract( JSON.parse(interface))
    .deploy({ data: bytecode })
    .send({
        from: accounts[0],
        gas: '2000000'
    });

// resolved code
const result = await new web3.eth.Contract( JSON.parse(interface))
    .deploy({ data: bytecode })
    .send({
        from: accounts[0]
        //gas: '2000000'
    });
```

- UnhandledPromiseRejectionWarning: Error: invalid argument 0: json: cannot unmarshal hex string without 0x prefix into Go struct field CallArgs.data of type hexutil.Bytes
  - `https://github.com/ConsenSys/ethjsonrpc/issues/26`
  - Add the prefix `0x` to the hexstring

- Warning: Using contract member "balance" inherited from the address type is deprecated. Convert the contract to "address" type to access the member.
  - `https://ethereum.stackexchange.com/questions/42326/warning-using-contract-member-balance-inherited-from-the-address-type-is-depr`

```js
// warning code
players[index].transfer(this.balance);

// resolved code
players[index].transfer(address(this).balance);
```

- [Solidity] Keyword `var` issue
  - `http://solidity.readthedocs.io/en/latest/security-considerations.html#minor-details`
  - In `for (var i = 0; i < arrayName.length; i++) { ... }`, the type of i will be `uint8`, because this is the smallest type that is required to hold the value 0. If the array has more than 255 elements, the loop will not terminate.

- [Solidity] a file not found when you import other solidity file
  - The issues is when you import other solidity file and compile it with solc, there is an error said the file not found.
  - For example,
    - `https://github.com/dicoblock/Dicoblock/tree/master/dev/ethan/ethereum/gembling/ethereum/contracts`
    ```
    \- ethereum
      \- contracts
        \- games
        | \- Zerosum.sol
        \- GemFactory.sol
    ```
  - The `GemFactory.sol` try to include `games/Zerosum.sol`. It works where you trying on remix but, it failed my `compile.js` (solc)
    - `https://github.com/dicoblock/Dicoblock/blob/master/dev/ethan/ethereum/gembling/ethereum/compile.js`
  - This is because when you compile `GemFactory.sol` that is include `games/Zerosum.sol` but they can't found the file.
  - I can resolve this issue by adding souces as dictionary which has key as the path.
    ```js
    var targets = {
      "GemFactory.sol": fs.readFileSync(contractPath + '/GemFactory.sol', 'utf8'),
      "ownership/Ownable.sol": fs.readFileSync(contractPath + '/ownership/Ownable.sol', 'utf8'),  
      "games/Zerosum.sol": fs.readFileSync(contractPath + '/games/Zerosum.sol', 'utf8')
    };

    //const gemPath = path.resolve(__dirname, 'contracts', 'GemFactory.sol');
    //const source = fs.readFileSync(gemPath, 'utf-8');

    const output = solc.compile({sources: targets}, 1).contracts;
    ```
  - As you can see, the `targets` dictionary has key as path that we want to specify in the solidity file.

## Vulnerability Research

- [Array Length Over/Underflow Bug](https://github.com/dicoblock/Dicoblock/blob/master/dev/ethan/research/vulnerability/01_Array_Length_Underflow/README.md)

## CTF

- [2018_Defcon_sag](https://github.com/dicoblock/Dicoblock/blob/master/dev/ethan/CTF/2018_defcon_sag/README.md)

## ETC

- (encrypt and sign) and (decrypt and verify)
  - `https://github.com/pubkey/eth-crypto/blob/master/tutorials/encrypted-message.md`
- EVM deompiler
  - `https://ethervm.io/decompile`
  - `https://github.com/ajlopez/Yasold`
