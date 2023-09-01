# Events

In Rubidity, events serve as mechanisms to log and emit changes or activities that occur within a smart contract. Here is how you can define and emit events in Rubidity.

**Event Definition**

To define an event in your Rubidity contract, you use the `event` keyword followed by the event name and a hash describing the expected arguments:

```ruby
event :Transfer, { from: :addressOrDumbContract, to: :addressOrDumbContract, amount: :uint256 }
```

In this example, a `Transfer` event is defined with three arguments—`from`, `to`, and `amount`. Their types are also specified, making it strongly-typed just like any other aspect of Rubidity.

**Event Emission**

To emit an event, you call the `emit` method within your contract's methods. The `emit` method requires two arguments: the name of the event to emit and a hash containing the arguments to pass to the event:

```ruby
def some_transfer_function(from, to, amount)
  # Perform transfer logic here
  emit(:Transfer, { from: from, to: to, amount: amount })
end
```

**Event Validation**

As with functions, Rubidity performs argument validation when emitting an event. Specifically, it checks:

* If the event is defined in the contract.
* If any required arguments are missing.
* If any unexpected arguments are included.

**Example**

Here is a small example that demonstrates defining and emitting an event in a Rubidity contract:

```ruby
class Contracts::MyToken < Contract
  event :Transfer, { from: :address, to: :address, amount: :uint256 }
  
  function :do_transfer, { from: :address, to: :address, amount: :uint256 }, :public do
    # Perform the transfer logic here
    emit(:Transfer, { from: from, to: to, amount: amount })
  end
end
```
