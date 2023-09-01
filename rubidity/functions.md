# Functions

### Function Definition

You define functions with the `function` method with the following signature:

```ruby
function :function_name, {arg1: :type1, arg2: :type2, ...}, *options, returns: return_type do
  # function body
end
```

For example:

```ruby
function :tokenURI, { id: :uint256 }, :public, :view, :override, returns: :string do
  require(_exists(id: id), 'ERC721Metadata: URI query for nonexistent token')
  # ...
end
```

**Parameters**

* `:function_name` - The name of the function
* `{arg1: :type1, arg2: :type2, ...}` - A hash of argument names to types.
* `*options` - Zero or more flags that specify additional function characteristics like visibility (`:public`, `:private`), state mutability (`:view`, `:pure`, `:payable`), etc.
* `returns: return_type` - Specifies the return type. This is optional but if provided, the function must return a value that matches this type.

**Inside the Function Body**

* `s.variableName` can be used to read from and write to state variables.
* If you write an identifier like `variableName` without the `s.` prefix, the system will look for it among the function's arguments first, and then among the contract's functions.

### Calling Functions in Other Contracts

To invoke functions from another contract, the contract ID is needed. You can create an instance of that contract using `DumbContract(id)` and then call its methods.

Example:

```ruby
DumbContract(id).transfer(
  to: msg.sender,
  amount: output_amount
)
```

#### Automatically Generated Functions

If you have a public state variable, a getter function is automatically generated. So, there's no need to write a separate getter function for that variable.
