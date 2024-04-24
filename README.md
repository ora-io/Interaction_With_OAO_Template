## Introduction
This repository contains a simple template for interaction with OAO (On-chain AI Oracle). There are 3 user facing contracts: `Prompt.sol`, `PromptWithCallbackData.sol` and `PromptDifferentModels.sol`. 
- Prompt is a simple contract that can interact with OAO through *`calculateAIResult`* method.
- PromptWithCallbackData is an extension of Prompt, which passes callback data in the *`requestCallback`* method when calling OAO. This allows execution of arbitrary logic, after the OAO returns the result to the chain.
- PromptDifferentModels is an extension of Prompt, which executes different actions in the callback depending on the modelId.

## Setup
Clone the repository and install submodules.
```bash
git clone -b OAO_interaction_tutorial git@github.com:ora-io/Interaction_With_OAO_Template.git --recursive
```
> Note: make sure to update all submodules to the latest version. 

## Test Guide
To execute tests run `forge test`, or `forge test -vvvv` for more info.
### Prompt.t.sol 
- **test_SetUp** - checks if the Prompt contract is successfully created and set up.
- **test_CallbackGasLimit** - in order to return data to the Prompt, OAO system needs to execute callback transaction. To do this, a user needs to provide a fee as a bounty to the Prompt contract. The gas fee required for this transaction is dependent on the gas price and the amount of gas used in the transaction. Hence, gas limit is set for each model to limit amount of gas that can be spent on a single interaction with OAO. This test checks the current gas limit of the model and sets the new one. Note that only owner of the Prompt contract can update the gas limit.
- **test_OAOInteraction** - in this test we interact with OAO by calling *`calculateAIResult`* method. This method initiates *`requestCallback`* call to the OAO system. OAO system calculates the result and sends it back to the Prompt contract along with the proof, by calling *`aiOracleCallback`* method.
- **test_CallbackGasLimit** - Checks if OAO is able to call back into the Prompt contract

## Deployment Guide
To deploy Prompt contract, set the necessary environment variables and run the following commands: <p>
```bash
source .env
``` 
<p>

```bash
forge script script/Prompt.s.sol --rpc-url $RPC_URL --broadcast --verify --etherscan-api-key $ETHERSCAN_KEY
```

Same can be done for other contracts.

