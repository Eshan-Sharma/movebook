# Setting up an Account and Interacting with a package on Sui Blockchain

## Setting up an account

### Commands to run

```
sui client
sui client active-address
sui client faucet
sui client gas
sui client balance
```

### Steps

1. Install Sui client (if not already installed).
2. Run sui client to connect to a Sui Full Node server and generate a new keypair.
   ![image](https://github.com/Eshan-Sharma/movebook/assets/43044334/08217293-f1f4-4452-aabc-1a183a8b5d66)
3. Verify the account setup by running sui client active-address.
4. Request coins for your account using sui client faucet.
   ![image](https://github.com/Eshan-Sharma/movebook/assets/43044334/4509cfae-2237-4fdd-aa54-f88c666c002e)
   ![image](https://github.com/Eshan-Sharma/movebook/assets/43044334/7f9d9257-5087-4d25-99c5-c68ffc8d0b06)
   You can also run `sui client objects` to see the Coin objects you are an owner of

## Publishing a Package

### Commands to run

```
# run this from the `todo_list` folder
sui client publish --gas-budget 100000000
# or use
sui client publish --gas-budget 100000000 --json
```

- Use `sui client publish` command to publish the package.
- The command will build the package, verify dependencies, and publish the package to the network.
- The output will include the transaction digest, transaction data, and transaction effects.
- `gas-budget` is specified in MISTs. 1 SUI equals 10^9 MISTs.
- After the package is published on chain, we can interact with it. To do this, we need to find the address (object ID) of the package. It's under the Published Objects section of the Object Changes output. The address is unique for each package, so you will need to copy it from the output.

## Interacting with a package

1. Find the address of the published package from the output of the `sui client publish` command. Set it to PACKAGE_ID using `export PACKAGE_ID = "address you got"`
2. Use `sui client ptb` command to build a transaction for interacting with the package.

Command

```
export MY_ADDRESS=$(sui client active-address)

sui client ptb \
--gas-budget 100000000 \
--assign sender @$MY_ADDRESS \
--move-call $PACKAGE_ID::todo_list::new \
--assign list \
--transfer-objects "[list]" sender
```

3. The command allows calling functions defined in the package and transferring objects.
   If you run `sui client objects` you will see your published package now.
![image](https://github.com/Eshan-Sharma/movebook/assets/43044334/8c6efc86-28e4-42d1-8f03-d824c98aef1f)
Use the ```objectId``` of the object with ```objectType 0xa666..4a14::todo_list::Todolist``` and set ```export LIST_ID="objectId here"```

4. You can chain multiple commands in a single transaction block. This shows the power of Transaction Blocks! Using the same list object, we will add three more items and remove one. The command will look like this:
```
sui client ptb \
--gas-budget 100000000 \
--move-call $PACKAGE_ID::todo_list::add @$LIST_ID "'Finish Concepts chapter'" \
--move-call $PACKAGE_ID::todo_list::add @$LIST_ID "'Read the Move Basics chapter'" \
--move-call $PACKAGE_ID::todo_list::add @$LIST_ID "'Learn about Object Model'" \
--move-call $PACKAGE_ID::todo_list::remove @$LIST_ID 0
```

## Documentation
https://move-book.com/your-first-move/hello-sui.html#prepare-the-variables
