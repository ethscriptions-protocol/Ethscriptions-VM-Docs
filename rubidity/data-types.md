# Data Types

Rubidity is strongly-typed. This documentation aims to give you an overview of Rubidity's type system so you can create more robust and reliable applications.

### Available Types in Rubidity

* `:string`: Text-based data. Rubidity strings are immutable.
* `:mapping`: Key-value storage for different types.
* `:address`: Ethereum address (in hexadecimal).
* `:dumbContract`: A specific type of contract ID (hexadecimal).
* `:addressOrDumbContract`: Either an Ethereum address or a specific type of contract ID.
* `:ethscriptionId`: Unique identifiers for Ethscriptions (hexadecimal).
* `:bool`: Boolean values (true or false).
* `:uint256`: Unsigned 256-bit integers.
* `:int256`: Signed 256-bit integers.
* `:array`: Lists of other types.
* `:datetime`: Date and time (stored as unsigned 256-bit integers).

Every Rubidity variable has a type. This means that whenever you declare a variable in an event, function, or state variable, you must include its type. Here's an example of declaring the type of a state variable:

```ruby
uint256 :public, :totalSupply
```

You don't have to declare the type of a local variable, so you can just write `val = s.totalSupply` (recall `s.` is how you access state variables). When you do this val takes its type from `totalSupply`.

You also don't have to declare the type of literals like "I'm a string". Literals have their types inferred when they are assigned to typed variables. For example, "100" will be interpreted as the integer 100 when assigned to a uint256 and as the string "100" when assigned to a string.

Example:

```ruby
ruby code# State variable with type
uint256 :public, :totalSupply

# Local variable without explicit type
val = s.totalSupply
```

Most of the time a variable can remain untyped until its value is assigned to a typed variable. However, sometimes you must explicitly cast variables. For example, typed variables can only be equal to other typed variables. In these cases you can cast a variable with `7.cast(:uint256)`.

### Type Coercion and Validation in Rubidity

Rubidity employs a strong system of type validation and coercion to ensure that variables adhere to their declared types. This involves transforming literal values into the corresponding Rubidity types and reporting type mismatches.

Here's a brief rundown of Rubidity's type coercion rules:

* **:address**: Accepts hexadecimal strings that match the Ethereum address format (`0x` followed by 40 hexadecimal characters). The address is then normalized to lowercase.
* **:uint256 and :int256**: These types accept both integer and string representations. Strings are attempted to be coerced into integers. uint256 and int256 cannot be out of the range of their Solidity counterparts.
* **:string**: Only accepts string literals. Starting from the latest update, strings are immutable.
* **:bool**: Accepts only `true` or `false`.
* **:dumbContract and :ethscriptionId**: Accepts hexadecimal strings matching specific patterns (`0x` followed by 64 hexadecimal characters).
* **:addressOrDumbContract**: Accepts either an address or a `:dumbContract`, again matching the relevant hexadecimal patterns.
* **:datetime**: Relies on `:uint256` type coercion, as it's represented as an unsigned integer internally.
* **:mapping**: Accepts a Hash and ensures that keys and values match the specified types. Coerces these into a special mapping proxy object.
* **:array**: Accepts an array and ensures that the values match the specified type. Coerces these into a special array proxy object.

### Default Values

In Rubidity, every type comes with a default value that gets assigned when a variable is declared but not initialized. Understanding these defaults is crucial for avoiding unintended behavior in your dApps. Here is the rundown:

* **Integers (:int256, :uint256, :datetime)**: Default to `0`.
* **Address Types (:address, :addressOrDumbContract)**: Default to a zero-address, which is `0x0000000000000000000000000000000000000000`.
* **Contract Identifiers (:dumbContract, :ethscriptionId)**: Default to a zero-identifier, `0x0000000000000000000000000000000000000000000000000000000000000000`.
* **String (:string)**: Default to an empty string `''`.
* **Boolean (:bool)**: Default to `false`.
* **Mapping (:mapping)**: Default to an empty mapping proxy object. The key and value types are set according to your specifications.
* **Array (:array)**: Default to an empty array proxy object. The value type is set according to your specification.
