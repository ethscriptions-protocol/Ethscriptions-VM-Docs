# VM API Data Model

### Database Schema

The VM relies on three primary database tables to capture the contract information, contract states, and call receipts.

#### `contracts` Table

* **Fields:**
  * `contract_id`: A unique identifier for the smart contract.
  * `type`: Categorizes the contract, such as ERC20 or ERC721.
  * `created_at` & `updated_at`: Standard timestamps.
* **Role**: Maintains a registry of all smart contracts deployed or interacted with.

#### `contract_states` Table

* **Fields:**
  * `contract_id`: Reference to the `contracts` table.
  * `ethscription_id`: The ID of the Ethscription transaction that led to this particular contract state.
  * `state`: JSON object containing the contract's current state.
  * `block_number` & `transaction_index`: Ethereum block and transaction index that capture the state.
* **Role**: Holds historical states of smart contracts, enabling time-travel queries for auditing or state reversion.

#### `contract_call_receipts` Table

* **Fields:**
  * `contract_id`: Reference to the `contracts` table.
  * `ethscription_id`: ID of the initiating Ethscription.
  * `caller`: Ethereum address of the entity initiating the function call.
  * `status`: Execution status code.
  * `function_name` & `function_args`: Function details.
  * `logs`: Execution logs.
  * `timestamp`: Time of call.
  * `error_message`: Stored if the call results in an error.
* **Role**: Records the outcomes of all contract function calls, enabling debugging, auditing, and transaction history views.

### Execution Flow

1. **Transaction Initialization**: Upon receiving a new Ethscription, initial validation occurs. If a new contract is deployed, a new row is created in the `contracts` table.
2. **Pre-Execution State**: The latest `contract_state` is fetched based on the `contract_id` to set the initial state of the smart contract.
3. **Function Execution**: The specified function in the smart contract is called, potentially altering its state.
4. **Post-Execution State**: A new row is added to `contract_states` capturing the updated contract state.
5. **Receipt Logging**: A new row is logged in `contract_call_receipts` with all the details of the function call, including its success or failure status.
6. **API Exposure**: This data is now accessible via various API endpoints, like `ContractsController#show_call_receipt`, which queries `contract_call_receipts` based on `ethscription_id` to deliver detailed transaction receipts.

By harmonizing the capabilities of Ethereum smart contracts with the efficiency of Ethscriptions, the VM creates a powerful environment for developing, deploying, and interacting with decentralized applications, all at a fraction of the cost usually associated with on-chain operations.
