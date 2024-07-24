# solidity

## Overview

A statically-typed curly-braces programming language designed for developing smart contracts that run on Ethereum

In Solidity, view functions are used to read data without making any changes. You can identify these functions by the view modifier in their declaration, such as function isSolve() public view;. To call these functions, you don't need to sign a transaction; you can simply query for data using the cast tool with the following command:

	cast call $ADDRESS_TARGET "functionToCall()" --rpc-url $RPC_URL

https://github.com/Andre92Marcos/tools/tree/master/foundry

If the function requires arguments, you need to specify the argument types within braces and provide their values outside the string, like this:

	cast call $ADDRESS_TARGET "functionWithArgs(uint, bool)" 5 true --rpc-url $RPC_URL

**Calling a normal function**

To call a function that modifies data, you need to sign the transaction. These functions are any non-view and non-pure functions in Solidity. You can use the cast tool again with the following command:

	cast send $ADDRESS_TARGET "functionToCall()" --rpc-url $RPC_URL --private-key $PRIVATE_KEY

If the function has arguments, you follow the same pattern as before:

	cast send $ADDRESS_TARGET "functionWithArgs(uint)" 100 --rpc-url $RPC_URL --private-key $PRIVATE_KEY

Additionally, some functions may be marked as payable, which means they can accept Ether along with the call. You can specify the value using the --value flag:

	cast send $ADDRESS_TARGET "functionToCall()" --rpc-url $RPC_URL --private-key $PRIVATE_KEY --value 100 

**Initializing a forge project**

To create and deploy smart contracts, we will use another tool called forge from the foundry-rs suite. You can initialize an empty forge project using the command:

	forge init .

You can optionally use flags like --no-git to skip initializing a Git repository and other useful options. The project will contain the following directories and files:

    src/: This is where you write your smart contracts. It initially contains an example contract called Counter.sol.
    test/: This is where you write tests. There is an example test file called Counter.t.sol. You can run these tests using the forge test command. Feel free to explore this feature on your own as it is highly useful but beyond the scope of our discussion.
    script/: This folder is used for scripts, which are batch Solidity commands that run on-chain. An example script could be a deployment script. The folder contains an example script called Counter.s.sol. You can execute these scripts using forge script script/Counter.s.sol along with additional flags based on your requirements. Feel free to experiment with this feature as it is extremely useful.
    lib/: This is where you place any libraries. By default, there is only one library called forge-std, which includes useful functions for debugging and testing. You can download additional libraries using the forge install command. For example, you can install the commonly used openzeppelin-contracts library from the OpenZeppelin repository with forge install openzeppelin/openzeppelin-contracts.
    foundry.toml: This is the configuration file for forge. You usually don't need to deal with it during exploitation, but it is helpful for development purposes. 

**Deploying a Contract**

The final step is to deploy a smart contract after completing the coding. This can be done using the forge tool. The command is as follows:

	forge create src/Contract.sol:ContractName --rpc-url $RPC_URL --private-key $PRIVATE_KEY

After executing this command, the deployer's address (which is essentially our address), the transaction hash, and the deployed contract's address will be printed on the screen. The deployed contract's address is the one we need to use for interacting with it.

If our contract has a payable constructor, we can use the --value flag in the same way as in the cast send command:

	forge create src/Contract.sol:ContractName --rpc-url $RPC_URL --private-key $PRIVATE_KEY --value 10000

Additionally, if the constructor has arguments, we can specify them using the --constructor-args flag and provide the arguments in the same order they appear in the constructor. For example, if the constructor is defined as constructor(uint256, bytes32, bool), we would use the following command:

	forge create src/Contract.sol:ContractName --rpc-url $RPC_URL --private-key $PRIVATE_KEY --constructor-args 14241 0x123456 false

You can combine multiple flags and there are more flags available that we haven't mentioned here. However, these are the most common ones.