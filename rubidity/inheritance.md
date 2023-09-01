# Inheritance

#### Overview

In Rubidity, inheritance allows you to create new contracts that inherit the properties and methods of existing contracts. This is similar to how inheritance works in Solidity. The `is` keyword is used to denote inheritance. The concept of `virtual` and `override` modifiers is also present to provide fine-grained control over inherited methods.

#### Syntax

To declare that one contract inherits from another, the `is` keyword is used:

```ruby
class YourContract < Contract
  is :ParentContract
  ...
end
```

This causes all of `ParentContract's` functions, events, and state variables to be merged into `YourContract`.

Conversely, if a parent contract marks itself abstract, it can only be inherited from, but never deployed:

```ruby
class Contracts::ERC20 < Contract
  abstract
  
  event :Transfer, { from: :addressOrDumbContract, to: :addressOrDumbContract, amount: :uint256 }
  event :Approval, { owner: :addressOrDumbContract, spender: :addressOrDumbContract, amount: :uint256 }
end
```

#### Virtual and Override Modifiers

When a contract inherits from another, Rubidity automatically merges functions from parent contracts. If both parent and child contracts have a function with the same name, the child's function will override the parent's function, provided that the parent function is marked as `virtual` and the child's function is marked as `override`.

* `virtual`: Denotes that a function can be overridden in derived contracts.
* `override`: Used in a derived contract to specify that the function intentionally overrides a `virtual` function in the parent contract.

#### Super

Within a child function you can call the parent implementation with a **`_super_`** prefix. For example, `_super_transferFrom`. Ordinarily only the immediate parent can be called, but in constructors any parent can be called with this more explicit syntax: `ERC721(name: name, symbol: symbol)`.

### Example

This example shows off inheritance, overriding, and the two different forms of super:

```ruby
class Contracts::ERC721 < Contract
  string :public, :name

  constructor(name: :string, symbol: :string) {
    s.name = name
    s.symbol = symbol
  }
  
  function :transferFrom, { from: :addressOrDumbContract, to: :addressOrDumbContract, id: :uint256 }, :public, :virtual do
    # Transfer logic
  end
end
```

```ruby
class Contracts::TransferTracker < Contract
  is :ERC721
  
  uint256 :public, :transferCount
  
  constructor(
    name: :string,
    symbol: :string,
    # rest of args
  ) {
    ERC721(name: name, symbol: symbol)
    # Rest of the constructor code goes here
  }
  
  function :transferFrom, { from: :addressOrDumbContract, to: :addressOrDumbContract, id: :uint256 }, :public, :override do
    s.transferCount += 1
    s.name = "I have been transferred #{string(s.transferCount)} times!"
    
    _super_transferFrom(from: from, to: to, id: id)
  end
end
```

\
