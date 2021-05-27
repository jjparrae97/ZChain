# Running Zchain Blockchain

The Proof of Authority (PoA) algorithm is typically used for private blockchain networks as it requires pre-approval of, or voting in of, the account addresses that can approve transactions (seal blocks).  

## Required Packages
___
* [MyCrypto](https://www.mycrypto.com/)
* [Go Ethereum](https://geth.ethereum.org/)

## Blockchain Instructions
___


1. Because the accounts must be approved, we will generate two new nodes with new account addresses that will serve as our pre-approved sealer addresses.

    * Create accounts for two nodes for the network with a separate `datadir` for each using `geth`.

         Example:

          ./geth --datadir zchain1 account new
          ./geth --datadir zchain2 account new

2. Next, generate your genesis block.

    * Run `puppeth`, name your network, and select the option to configure a new genesis block.

    * Choose the `Clique (Proof of Authority)` consensus algorithm.

    * Paste both account addresses from the first step one at a time into the list of accounts to seal.

            Zchain1: 0x12814335cefBB13704C5558614617c74f2b07f3c
            Zchain2: 0x04Ba3bb1D0f5BE3F30899a5746b7a13365DBa444

    * Paste them again in the list of accounts to pre-fund. There are no block rewards in PoA, so you'll need to pre-fund.

    * You can choose `no` for pre-funding the pre-compiled accounts (0x1 .. 0xff) with wei. This keeps the genesis cleaner.

    * Complete the rest of the prompts, and when you are back at the main menu, choose the "Manage existing genesis" option.

    * Export genesis configurations. This will fail to create two of the files, but you only need `zchain.json`.

3. With the genesis block creation completed, we will now initialize the nodes with the genesis' json file.

    * Using `geth`, initialize each node with the new `zchain.json`.

          ./geth --datadir zchain1 init zchain.json

          ./geth --datadir zchain2 init zchain.json

4. Now the nodes can be used to begin mining blocks.

    * Run the nodes in separate terminal windows with the commands:
        
        Zchain1:

          ./geth --datadir zchain1 --unlock "0x12814335cefBB13704C5558614617c74f2b07f3c" --mine --rpc --allow-insecure-unlock

        Zchain2:

          ./geth --datadir zchain2 --unlock "0x04Ba3bb1D0f5BE3F30899a5746b7a13365DBa444" --mine --port 30304 --bootnodes "enode://af0c903f49f28bdebd8fbace7227a1691cbcec245cbf198bb51e844b75f82e9c5c80d682c829ebd9decffd2372097bb32e2406b92489effbe83b0dbc76382481@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock
    * **NOTE:** Type your password and hit enter - even if you can't see it visually!

5. Your private PoA blockchain should now be running!

6. With both nodes up and running, the blockchain can be added to MyCrypto for testing.

    * Open the MyCrypto app, then click `Change Network` at the bottom left:

    ![change network](Resources/change-network.png)

    * Click "Add Custom Node", then add the custom network information that you set in the genesis.

    * Make sure that you scroll down to choose `Custom` in the "Network" column to reveal more options like `Chain ID`:

    ![custom network](Resources/custom-network.png)

    * Type `ETH` in the Currency box.
    
    * In the Chain ID box, type the chain id you generated during genesis creation.

    * In the URL box type: `http://127.0.0.1:8545`.  This points to the default RPC port on your local machine.

    * Finally, click `Save & Use Custom Node`. 

7. After connecting to the custom network in MyCrypto, it can be tested by sending money between accounts.

    * Select the `View & Send` option from the left menu pane, then click `Keystore file`.

    ![select_keystore_file](Resources/select_keystore_file.png)

    * On the next screen, click `Select Wallet File`, then navigate to the keystore directory inside your Node1 directory, select the file located there, provide your password when prompted and then click `Unlock`.

    * This will open your account wallet inside MyCrypto. 
    
    * Looks like we're filthy rich! This is the balance that was pre-funded for this account in the genesis configuration; however, these millions of ETH tokens are just for testing purposes.   

    ![keystore_unlock](Resources/keystore_unlock.gif)

    * In the `To Address` box, type the account address from zchain2, then fill in an arbitrary amount of ETH:

     ![transaction send](Resources/transaction-send.png)

    * Confirm the transaction by clicking "Send Transaction", and the "Send" button in the pop-up window.  

    ![Send transaction](Resources/send-transaction.gif)

    * Click the `Check TX Status` when the green (or black message if using dark mode) message pops up, confirm the logout

    * You should see the transaction go from `Pending` to `Successful` in around the same blocktime you set in the genesis.

    * You can click the `Check TX Status` button to update the status.

    ![successful transaction](Resources/transaction-status.png)

Congratulations, you successfully used the zchain blockchain!

## Hint
___
* For the commands and addresses of the nodes used in this article, see [Zchain Cheatsheet](ZchainCheatsheet.txt) 