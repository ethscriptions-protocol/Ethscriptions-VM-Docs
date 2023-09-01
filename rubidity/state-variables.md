# State Variables

## Defining State Variables in Rubidity

### Overview

State variables are data elements that store the contract's state. In Rubidity, you can define state variables, specify their types, and set visibility options (`public`, `private`, or `internal`) along with additional flags like `immutable` and `constant`.

### Basic Syntax

The basic syntax to define a state variable is as follows:

```ruby
type :visibility, :variable_name, :flags
```

* `type`: The type of the variable (`uint256`, `string`, etc.)
* `visibility`: Visibility of the variable (`public`, `private`, or `internal`)
* `variable_name`: The name of the state variable
* `flags`: Additional flags like `:immutable` or `:constant`

### Examples

Declare a public string variable named `name`:

```ruby
string :public, :name
```

Declare a public unsigned integer named `totalSupply`:

```ruby
uint256 :public, :totalSupply
```

### Advanced Types

For complex types like mappings and arrays, special syntax is used:

#### **Mappings**

```ruby
mapping ({ key_type: :value_type }), :visibility, :variable_name
```

#### **Arrays**

```ruby
array :value_type, :visibility, :variable_name
```

### Automatically Generated Getters

Much like Solidity, for every public state variable, Rubidity automatically generates a getter function.

### Accessing State Variables in Contract Logic

In your contract logic, you can access state variables using the `s` object:

```ruby
s.variable_name
```

Example:

```ruby
# Accessing a state variable
function :get_balance, { addr: :address }, :public, :view, returns: :uint256 do
  return s.balances[addr] # Accessing s.balances
end
```

#### Flags (Not Yet Implemented)

* `:immutable`: The variable can only be set once, typically in the constructor.
* `:constant`: The variable's value is set at compile time and cannot be changed.

***

\
