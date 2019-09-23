### Gear SDK Documentation

___
## Home
<h1>Gear: smart contracts on bitcoin</h1>

![](worm_gear.gif)
<button name="button"  
  style="background-color: #00FF00;"
  onclick="window.location.href='https://github.com/gear-sv/gear-contracts'">
  GEAR CONTRACTS
</button>
<button name="button"
  style="background-color: #00FF00;"
  onclick="window.location.href='https://github.com/gear-sv/gear-nano'">
  GEAR NANO
</button>
<button name="button"  
  style="background-color: #00FF00;"
  onclick="window.location.href='https://gear.computer'">
  GEAR COMPUTER
</button>
<button name="button"  
  style="background-color: #00FF00;"
  onclick="window.location.href='https://medium.com/@_seanavery/gearsv-smart-contracts-for-bitcoin-68ee92a2e66e'">
  BLOG
</button>

### what?
![](bitcoin.png) ![](planaria.png) ![](wasm.png) ![](cpp.png)

<p>Gear is general purpose computation engine that runs on top of bitcoin.<p>
<p>This means you can create an immortal piece of code whose state is 100% auditable.</p>

<p>The Gear SDK lets your write arbitrary programs, publish them to the blockchain, and handle reads and writes.</p>
<p>Although the sdk currently only supports CPP, it can be extended to handle Python, Go, Rust, C, and much more.</p>


### how?
1. Bitcoin as the consensus engine, payment system, and write interface.
1. Web Assembly as the compilation target.
2. Planaria to scrape the blockchain and update contract state.
3. Planarium to store state to disk and serve over HTTP.


### why?

## Gear Contracts

Compile, test, and deploy contracts to bitcoin.

<p align="left">
  <img src="./gear_docs.png" width="600" syle="padding: 40px"
</p>

### Setup
1. `npm i gear-contracts -g`
2. Install emscripten.

### CLI
1. `gear-contracts init`
2. `gear-contracts keys`
3. `gear-contracts compile`
4. `gear-contracts test`
5. `gear-contracts deploy`

## Gear Nano

### Setup

### CLI
1. `gear-nano init [contractID]`
2. `gear-nano processor`
3. `gear-nano transactions`
4. `gear-nano state`
5. `gear-nano app`

## Contracts

### FungibleToken

```c++
FungibleToken::FungibleToken(string owner) {
  this->supply = 0;
  this->owner = owner;
}

const string& FungibleToken::setOwner(string SENDER, string newOwner) {
  // check that SENDER is the current owner
  if (SENDER != this->owner) {
    return "fail";
  }

  this->owner = newOwner;
  return "pass";
}

const string& FungibleToken::mint(string SENDER, unsigned int amount) {
  // only the owner can mint
  if (SENDER != this->owner) {
    return "fail";
  }

  // mint tokens, assign to owner
  this->supply = this->supply + amount;

  // increment owner balance
  if (this->balances.find(SENDER) == this->balances.end()) {
    this->balances.insert(pair<string, unsigned int>(SENDER, amount));
  } else {
    unsigned int balance = this->balances[SENDER];
    this->balances[SENDER] = balance + amount;
  }

  return "pass";
}

const string& FungibleToken::transfer(string SENDER, string recipient, unsigned int amount) {
  // check if SENDER has sufficient funds
  if (this->balances[SENDER] < amount) {
    return "fail";
  }

  // increment recipient balance
  if (this->balances.find(recipient) == this->balances.end()) {
    this->balances.insert(pair<string, unsigned int>(recipient, amount));
  } else {
    unsigned int recipientBalance = this->balances[recipient];
    this->balances[recipient] = recipientBalance + amount;
  }

  // decrement SENDER balance
  int senderBalance = this->balances[SENDER];
  this->balances[SENDER] = senderBalance - amount;

  return "pass";
}

const unsigned int& FungibleToken::getSupply() {
  return this->supply;
}

const string& FungibleToken::getOwner() {
  return this->owner;
}

const unsigned int& FungibleToken::getBalance(string address) {
  return this->balances[address];
}

const map<string, unsigned int>& FungibleToken::getBalances() {
  return this->balances;
}
```
### NonFungibleToken

### NamingService
