# Global Variables

In Rubidity, you have access to several built-in global objects within Dumb Contracts to interact with the Ethereum network and transactions. These globals are analogous to the `msg`, `tx`, and `block` objects in Solidity.

Here's an example:

```ruby
class ExampleContract < Contract
  constructor() {}

  function :interact, {}, :public do
    sender = msg.sender
    origin = tx.origin
    block_num = block.number
    block_time = block.timestamp

    # Log an event or do something with these variables
  end

  # Demonstrating the use of esc
  function :findAnEthscription, { id: :ethscriptionId }, :public, returns: :string do
    ethscriptionDetails = esc.getEthscriptionById(ethscriptionId: id)
    
    # Here you can access individual fields
    id = ethscriptionDetails.ethscriptionId
    creator = ethscriptionDetails.creator

    # Return current owner
    return ethscriptionDetails.currentOwner
  end
end

```

Here's a look at some of these globals:

**`msg`**

The `msg` global provides access to the message sender's details.

**Properties**

* `msg.sender`: Represents the address of the account that is directly responsible for this transaction. Its type is either `addressOrDumbContract`.

**Usage**

```ruby
msg.sender
```

**`tx`**

The `tx` global offers details about the transaction.

**Properties**

* `tx.origin`: Indicates the address of the externally owned account that initiated the transaction. Its type is `address`.

**Usage**

```ruby
tx.origin
```

**`block`**

The `block` global provides details about the current block.

**Properties**

* `block.number`: The block number. Its type is `uint256`.
* `block.timestamp`: The block timestamp. Its type is `datetime`.

**Methods**

* `block.blockhash(block_number)`: Retrieves the hash of a block by its number.

**Usage**

```ruby
block.number
block.timestamp
block.blockhash(some_block_number)
```

**`esc`**

The `esc` global is unique to Rubidity and is tailored for Ethscriptions.

**Methods**

* `esc.getEthscriptionById(ethscription_id)`: Finds an Ethscription by its ID and returns a structured response containing details like block number, creator, current owner, and so on.

**Usage**

```ruby
esc.getEthscriptionById("some_ethscription_id")
```

Rubidity doesn't currently support structs, but the return value of getEthscriptionById is a special case. It behaves as if it was this struct:

```ruby
struct EthscriptionDetails {
  ethscriptionId: :ethscriptionId,
  blockNumber: :uint256,
  blockBlockhash: :string,
  transactionIndex: :uint256,
  creator: :address,
  currentOwner: :address,
  initialOwner: :address,
  creationTimestamp: :uint256,
  previousOwner: :address,
  contentUri: :string,
  contentSha: :string,
  mimetype: :string
}
```
