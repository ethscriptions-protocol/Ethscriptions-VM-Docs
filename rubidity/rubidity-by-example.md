# Rubidity by Example

Let's start with some examples from [Solidity's introduction](https://docs.soliditylang.org/en/v0.8.21/introduction-to-smart-contracts.html).

### Storage Example

This basic Solidity contract sets the value of a variable and then exposes it to others to access:

```solidity
contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

Here is how this logic translates to Rubidity:

```ruby
class Contracts::SimpleStorage < Contract
  uint256 :storedData
  
  function :set, { x: :uint256 }, :public do
    s.storedData = x
  end
  
  function :get, {}, :public, :view, returns: :uint256 do
    return s.storedData
  end
  
  constructor() {}
end
```

Looking point by point, here's what the Solidity version does and how Rubidity translates it.

### **Contract Definition**

* **Solidity**: `contract SimpleStorage { ... }`
*   **Rubidity**: `class Contracts::SimpleStorage < Contract`

    In Solidity, you define a contract using the `contract` keyword. In Rubidity, you define a contract as a Ruby class that inherits from a base class called `Contract`.

**State Variable**

* **Solidity**: `uint storedData;`
*   **Rubidity**: `uint256 :storedData`

    Solidity uses the `uint` keyword to define a state variable. Rubidity uses a Ruby symbol to define a state variable along with its type, in this case, `uint256`.

### **Function Definitions**

#### **Set Function**

**Solidity**:\


```solidity
function set(uint x) public {
  storedData = x;
}
```

**Rubidity**:\


```ruby
function :set, { x: :uint256 }, :public do
  s.storedData = x
end
```

The `set` function is public in both languages and takes an unsigned integer as an argument. In Rubidity, the function signature also specifies its visibility (`:public`) and arguments (`{ x: :uint256 }`).

#### **Get Function**

**Solidity**:

```solidity
function get() public view returns (uint) {
  return storedData;
}
```

**Rubidity**:

```ruby
function :get, {}, :public, :view, returns: :uint256 do
  return s.storedData
end
```

The `get` function is public and has a `view` property in both languages. The Solidity version uses the `returns` keyword to specify the return type, while Rubidity does it via the `returns: :uint256` option.

**Constructor**

* **Solidity**: No explicit constructor.
*   **Rubidity**: `constructor() {}`

    Both Solidity and Rubidity examples don't utilize a constructor for any initial setup, but the Rubidity code explicitly includes an empty constructor for clarity.

### **Accessing State**

In Rubidity, state variables are accessed using the `s.` prefix, as seen in `s.storedData = x` and `return s.storedData`. Writing to state using an unprefixed variable is not possible in Ruby and the explicitness of `s.` is nice anyway.

### Token Minting Example

Let's break down this `OpenMintToken` Rubidity contract line-by-line, focusing on how it might translate to a Solidity contract. The contract is an ERC20 token with a capped supply and additional limitations on individual mints.

```ruby
class Contracts::OpenMintToken < Contract
  is :ERC20
  
  uint256 :public, :maxSupply
  uint256 :public, :perMintLimit
  
  constructor(
    name: :string,
    symbol: :string,
    maxSupply: :uint256,
    perMintLimit: :uint256,
    decimals: :uint256
  ) {
    ERC20(name: name, symbol: symbol, decimals: decimals)
    s.maxSupply = maxSupply
    s.perMintLimit = perMintLimit
  }
  
  function :mint, { amount: :uint256 }, :public do
    require(amount > 0, 'Amount must be positive')
    require(amount <= s.perMintLimit, 'Exceeded mint limit')
    
    require(s.totalSupply + amount <= s.maxSupply, 'Exceeded max supply')
    
    _mint(to: msg.sender, amount: amount)
  end
  
  function :airdrop, { to: :addressOrDumbContract, amount: :uint256 }, :public do
    require(amount > 0, 'Amount must be positive')
    require(amount <= s.perMintLimit, 'Exceeded mint limit')
    
    require(s.totalSupply + amount <= s.maxSupply, 'Exceeded max supply')
    
    _mint(to: to, amount: amount)
  end
end
```

1. **Inheritance**: `is :ERC20`
   * This line means that the `OpenMintToken` contract inherits from an existing ERC20 contract, inheriting its state variables, functions, and logic.
2. **State Variables**: `uint256 :public, :maxSupply` and `uint256 :public, :perMintLimit`
   * These lines define two public state variables, `maxSupply` and `perMintLimit`. Public state variables are accessible to external contracts and can also have getter methods generated automatically.
3.  **Constructor**:

    ```ruby
    ruby constructor(
      name: :string,
      symbol: :string,
      maxSupply: :uint256,
      perMintLimit: :uint256,
      decimals: :uint256
    )
    ```

    * The constructor function initializes the contract. It takes the token's name, symbol, maximum supply, per-mint limit, and decimals as parameters.
4. **State Variable Initialization**: `s.maxSupply = maxSupply` and `s.perMintLimit = perMintLimit`
   * These lines initialize the state variables using the `s.` prefix, which is specific to Rubidity for accessing and manipulating state variables.
5.  **Mint Function**:

    ```ruby
    function :mint, { amount: :uint256 }, :public do
      require(amount > 0, 'Amount must be positive')
      require(amount <= s.perMintLimit, 'Exceeded mint limit')
      require(s.totalSupply + amount <= s.maxSupply, 'Exceeded max supply')
      _mint(to: msg.sender, amount: amount)
    end
    ```

    * This is a public function to mint new tokens. The `require` statements serve as checks that the `amount` is positive, within the per-mint limit, and won't exceed the max supply. `_mint` is a likely internal function inherited from the ERC20 contract that actually mints the tokens.
6.  **Airdrop Function**:

    ```ruby
    function :airdrop, { to: :addressOrDumbContract, amount: :uint256 }, :public do
      require(amount > 0, 'Amount must be positive')
      require(amount <= s.perMintLimit, 'Exceeded mint limit')
      require(s.totalSupply + amount <= s.maxSupply, 'Exceeded max supply')
      _mint(to: to, amount: amount)
    end
    ```

    * Similar to the `mint` function but allows specifying a recipient address `to`. It performs the same `require` checks and then invokes `_mint` to create the tokens for the target address.

The Rubidity code uses specific language constructs that mirror Solidity but in a Ruby-like syntax, making it more expressive while maintaining similar logic and functionalities.
