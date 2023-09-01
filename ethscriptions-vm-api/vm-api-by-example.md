# VM API By Example

Given this contract:

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

What happens when the Ethscriptions VM receives an ethscription with this data uri?

{% code overflow="wrap" %}
```
data:application/vnd.esc.contract.call+json,{"contractId":"0xdb6c87fd9b5e1aafdbc9cf4550856abde133e08e04910fe5fc319b63f5c08b3f","functionName":"mint","args":{"amount":5},"salt":"010e00f8408679670da12261c013b222"}
```
{% endcode %}

#### Step 1: Ethscription Validation

* The Ethscriptions VM first validates the incoming Ethscription transaction.
* It extracts the `content_uri` and decodes it to obtain the JSON payload containing `contractId`, `functionName`, and `args`.

#### Step 2: Contract Lookup

* The VM queries the `contracts` table to find the corresponding contract using the `contractId` provided (`0xdb6c87fd9b5e1aafdbc9cf4550856abde133e08e04910fe5fc319b63f5c08b3f` in this case).
* If the contract exists the VM proceeds to the next step.

#### Step 3: Pre-Execution State

* The VM fetches the latest state of the contract from the `contract_states` table based on the `contract_id`.
* The VM uses this state to set the initial values of state variables such as `maxSupply`, `perMintLimit`, and `totalSupply`.

#### Step 4: Function Parameters Validation

* The VM determines that the mint function exists on the contract.
* The VM then validates the parameters passed to the `mint` function, in this case, `amount: 5`. This includes type validations.
* Checks are made according to the conditions in the smart contract:
  * `amount` must be positive
  * `amount` must be less than or equal to `perMintLimit`
  * `totalSupply` + `amount` must be less than or equal to `maxSupply`

#### Step 5: Execute Mint Function

* If the validation checks pass, the VM calls the `mint` function.
* `_mint` is internally invoked, which increases the `totalSupply` and updates the balance of `msg.sender`.

#### Step 6: Post-Execution State

* A new entry is added to the `contract_states` table to reflect the state change post-minting.
* This includes the updated `totalSupply`, and the updated balance for `msg.sender`.

#### Step 7: Log Receipt

* A new row is inserted into the `contract_call_receipts` table, containing details of the function call, including:
  * `contract_id`
  * `ethscription_id`
  * `caller`
  * `status` (indicating success or failure)
  * `function_name` (in this case, "mint")
  * `function_args` (in this case, `{"amount": 5}`)
  * `logs` (any events emitted or other logs)
  * `timestamp`

#### Step 8: API Availability

* After the execution and logging, all this newly created data becomes available via API endpoints for querying or analysis.

Through these steps, the Ethscriptions VM efficiently executes the contract method, updates the state, and logs all necessary information for further interactions or audits.
