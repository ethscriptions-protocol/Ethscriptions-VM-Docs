# Error Handling

Rubidity provides a set of tools to help you write secure and reliable smart contracts. This includes native support for preconditions via `require` statements and automatic type checking for variables and parameters.

#### Require Statements

The `require` function is used to ensure that certain conditions are met before a function proceeds. If the condition specified in `require` is false, the function will throw an error, and all changes to the state will be reverted.

Here's an example that demonstrates the use of `require` in a `mint` function:

```ruby
function :mint, { amount: :uint256 }, :public do
  # Ensure the mint amount is positive
  require(amount > 0, 'Amount must be positive')
  
  # Ensure the mint amount does not exceed a pre-defined per mint limit
  require(amount <= s.perMintLimit, 'Exceeded mint limit')

  # Ensure the total supply won't exceed the max supply
  require(s.totalSupply + amount <= s.maxSupply, 'Exceeded max supply')
  
  # Proceed to mint
  _mint(to: msg.sender, amount: amount)
end
```

In this example, the `require` statements act as guards that prevent undesirable actions. If any of the `require` statements fail, the function will terminate, providing the string message as an error reason.

#### Type Checking

Rubidity also performs automatic type checking on function parameters and state variables. This means that if you declare a variable as a `:uint256`, Rubidity will ensure that you can only assign unsigned 256-bit integers to that variable. Any attempt to assign a different type will result in a runtime error.
