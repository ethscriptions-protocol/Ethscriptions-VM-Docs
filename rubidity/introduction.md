# Introduction

Rubidity is a high-level, object-oriented programming language designed for creating Dumb Contracts. The goal of Rubidity is to closely mimic Solidity so Dumb Contracts are easy to write for Smart Contract developers.

### Core Ruby Concepts You Should Know

**Blocks**: In Rubidity, blocks of code are encapsulated between `do` and `end` keywords. These blocks are often used to define the bodies of functions, loops, and control structures.

```ruby
rubyCopy codefunction :mint, { amount: :uint256 }, :public do
  require(amount > 0, 'Amount must be positive')
  _mint(to: msg.sender, amount: amount)
end
```

**Symbols**: Symbols are similar to strings but are prefixed with a colon. They are essentially the same thing as strings but by custom are used for identifiers.

```ruby
ruby string :public, :name
```
