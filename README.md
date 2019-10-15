# dExg | Decentralized Cryptocurrency Exchange
dExg is an official [dBnk](https://github.com/dbnks/dbnk) addon, as well as a completely separate decentralized web application (dApp), that is completely integrated with dBnk's API via [dBnkJS](https://github.com/dbnks/dbnkjs) and built entirely on top of dBnk's decentralized crypto banking platform. dExg brings to life atomic transactions, so that cryptocurrency from one ```blockchain_type```  can be successfully exchanged for a cryptocurrency of a different ```blockchain_type```, on a peer-to-peer basis. dExg is a cross-platform dApp that will be hosted on a peer-to-peer basis over the [dWeb Protocol](https://dweb.network) and is considered to be a completely decentralized cryptocurrency exchange that operates solely on a peer-to-peer basis. 

## Background
dExg was formerly known as aExchange on the [AriseBank](https://github.com/arisebank) platform that was seized by the SEC and a court-appointed receiver, before it could be launched in January 2018. The AriseBank project, now known as [dBnk](https://github.com/dbnks/dbnk), was famously called a fraudulent project by the SEC in one of the first cryptocurrency-based litigations in US history. The project is now currently under development as an open source project, by the same developers, some of which are still coding on notebook paper from jail. The project's development team has grown from 2 to 9 team members and in several months dBnk's development team plan on launching the dBnk platform in Alpha, along with many of its components including [dPay](https://github.com/dbnks/dpay) (formerly AriseCard and ArisePay), [dATM](https://github.com/dbnks/datm) (formerly aTM) and [dExg](https://github.com/dbnks/dexg) (formerly aExchange and aIExchanger). 

## What's Under The Hood 
[Arisen Blockchain](https://github.com/arisenio/arisen) - Blockchain software and smart contract platform for building decentralized economies.
[dWeb Protocol](https://github.com/distributedweb) - Protocol for Web 4.0
[FlockCore](https://github.com/flockcore) - dWeb-based libraries for peer-to-peer revelation.
[dDNS](https://github.com/distdns) - dWeb-based distributed DNS protocol.
[dDrive](https://github.com/ddrives) - dWeb-based p2p storage protocol.
[dDatabase](https://github.com/ddatabase) - dWeb abstraction layer used by dWeb-ready dDBMS'.
[dAppDB](https://github.com/dappdb) - Decentralized database management system (dDBMS).
[dBnk Smart Contract](https://github.com/dbnks/dbnk-contract) - Official smart contract for the dBnk dApp.
[dExg Smart Contract](https://github.com/dbnks/dexg-arisen-contract) - Official smart contract for the dExg dApp.
[dExg Off-Chain Contracts](https://github.com/dbnks/dexg-offchain-contracts) - Other dExg contracts used for atomic transactions on blockchains like Ethereum, TRON, EOS and others.
[React.JS](https://github.com/reactjs) - JavaScript framework for creating powerful web applications.
[Electron](https://github.com/electron) - Node framework for exporting Node applications as desktop applications on MacOSX, Linux and Windows with the same codebase.
[React Native](https://github.com/reactnative) - Node framework for exporting React.JS-based web and desktop application, as a mobile application on iOS and Android.

## Atomic Transactions
dExg brings to life atomic transactions across all ```blockchain_type```s that are integrated with the dBnk platform on a completely p2p basis, without a single central point of failure. Below is a short example of Bob and Alice using dExg, to exchange Bitcoin and Ethereum. We'll use Bitcoin as an example, so that it is proven that dExg can work with blockchains that are unable to handle application logic like Bitcoin, which has no ability to process the code within a smart contract for example, due to it lacking a virtual machine like EOS' WASM or Ethereum's EVM. 

### BTC -> ETH or ETH -> BTC EXAMPLE
- Alice places a sell order of 20 ETH, wanting 1 BTC in return. 
- An hour later, Bob places a buy order for 20 ETH, where he is willing to exchange 1 BTC for 20 ETH.
- dExg upon submission of Bob's buy order, finds Alice's sell order as a ```match``` by searching for matches throughout the ```dexg``` SCOPE of the [dexg contract](https://github.com/dbnks/dexg-arisen-contract), by filtering through the ```buy_orders``` and ```sell_orders``` tables.
- dExg, realizing that an attempted atomic swap will take place, between completely separate ```blockchain_type```s places the trade in the ```pending_order``` table with a unique ```_swapID```. This also means that both Bob's buy order and Alice's sell order are removed from dExg and locked so that the atomic swap process can begin and there are no ```double orders``` that take place.
- Bob is sent a notification on all devices dExg is installed on that there is a match for his trade and is asked to enter his ```CoinPin``` and ```CoinPass``` in order to begin the trade. A random ```secret_key``` is generated for Bob and then hashed with ```SHA256``` to create a ```secret_lock```.
- dExg uses Bob's ```secret_lock```  and dExg's ```Bitcoin Script``` and creates a transaction for 1 BTC on the Bitcoin blockchain. dExg by using the CoinPin and CoinPass, is able to decrypt the locally encrypted Mnenomic phrase, derive the private key of the particular dBnk ```account``` the Bitcoin is being sent from, creates the transaction and signs it using Bob's private key for the associated Bitcoin-based dBnk ```account```. Bob's ```receiving``` Ethereum account's private key is also derived and stores the ```secret_lock``` on Ethereum's blockchain, by interfacing with [dExg's Ethereum Contract](https://github.com/dbnks/dexg-offchain-contracts).
- dExg checks Bob's transaction on the Bitcoin blockchain and verifies the details of the trade.
- Alice is sent a notification on all devices dExg is installed on that Bob has sent his Bitcoin and is awaiting action from her.
- Alice is asked to enter her ```CoinPin``` and ```CoinPass``` so that her locally encrypted Mnemonic phrase can be decrypted, in order to derive the private key of her Ethereum wallet, so that she can interface with ```dExg's Ethereum Contract```. Alice's ```sending``` Ethereum account calls ```open``` using the unique ```_swapID``` that has been agreed to by both traders. Alice's 20 ETH is sent to ```dExg's Ethereum Contract```. 
**NOTE:** ***The ```open``` call is a ```payable``` call.***
- Alice receives the secret_lock which was stored on Ethereum's blockchain by Bob, in return.
- Bob is notified that Alice has sent the 20 ETH to dExg's Ethereum Contract and is asked to verify the details. Bob is asked to enter his ```CoinPin``` and ```CoinPass``` in order to decrypt his locally stored Mnemonic phrase, to derive the private key for his ```receiving``` Ethereum account. Using his private key, Bob calls ```check``` to verify the details of the trade and to confirm the 20 ETH exist on the Ethereum blockchain.
**NOTE:** ***If Bob doesn't ```check``` to verify the details of the trade, after 24 hours, Alice can call ```expire``` and get her ETH back from the ```dExg Ethereum Contract```.
- Bob is notified that the transaction of 20 ETH was successfully confirmed and is asked ```close```.
- Bob is asked to enter his ```CoinPin``` and ```CoinPass``` once more, which requires that he submits the ```secret_key``` associated with the ```secret_lock```. If Bob has provided the correct ```secret_key``` it will transfer Alice's 20 ETH to Bob's ```receiving``` Ethereum address.
- Alice is notified that Bob has received the 20 ETH and is asked to finalize the transaction of the 1 BTC. 
- Alice is asked to enter her ```CoinPin``` and ```CoinPass``` once more, where her ```sending``` Ethereum accounts' private key is derived from the locally encrypted Mnemonic phrase, in order to call the ```checkSecretKey``` function via the ```dExg Smart Contract```.
- Alice at this point acquires the ```secret_key``` where dExg provides the ```secret_key``` to dExg's ```Bitcoin Script```, where Alice successfully receives Bob's 1 BTC. 
- The trade is now placed in the ```confirmed_trades``` table of the ```dexg``` SCOPE of the ```dexg``` CONTRACT, so that it no longer is seen as a ```pending_trade```. 
- Alice and Bob are both notified that the trade has completed. 
- Alice and Bob can now see their trade under ```Confirmed Trades``` in the dExg dApp. 

## Development Stages
Development is currently underway, where several stages will have to be completed before dExg's ```Alpha``` release. Below are descriptions of each development stage.

### UI Design & Development (Stage 1)
After development of the [dExg Smart Contract](https://github.com/dbnks/dexg-arisen-contract), as well as it's various [off-chain contracts](https://github.com/dbnks/dexg-offchain-contracts), dExg's UI will be implemented on top of the [React.JS](https://github.com/reactjs) web framework.

This stage will cover the following areas:
- UI Wireframe Design.
- UI Design via Sketch.
- Convert UI Components to JSX for React.JS Development Stage.

### dApp Development Stage (Stage 2)
- Development of React/Electron/ReactNative framework for cross-platform deployment.
- Develop dBnk account import process and account settings framework that works with [dBnkID](https://github.com/dbnks/dbnkid).
- Develop initial framework around the creation of both ```buy_orders``` and ```sell_orders```. 
- Develop atomic transaction frameworks, scripts and contracts around dBnk-ready blockchains, so that unrelated cryptocurrencies can be traded p2p, like the example above.
- Develop air-gapped protocol for tunneled communications required for offline signing.
- Development of simple Arisen-based token creation via the dExg UI.
- Development of notification framework for web, desktop and mobile notifications.

### Atomic Transaction Testing (Stage 3)
Stage 3 includes heavy testing of different kinds of atomic transactions across many ```blockchain_type```s, as well as atomic token-for-token transactions to ensure all atomic trades that occur on the same ```blockchain_type``` are able to execute without error. This means trading against every possible pair on the exchange.

### Alpha Release (Stage 4)
Stage 4 will allow for the debut of dExg's ```Alpha``` release. 

## You Can Contribute 
dExg is a grassroots software project that anyone can contribute to. Whether you're looking to fix bugs, add features, design UI components or even offer up a new translation, all contributions are welcome. If you would like to contribute, please join the [Peeps Developer Channel](https://t.me/peepsdev) on Telegram and let our developers know that you would like to contribute to dExg's development, along with what you would like to contribute.

## Contributors
[Jared Rice Sr.](jared@dpeeps.com)
[NMH](nmh@dpeeps.com)
[M](m@dpeeps.com)
[Graham](g@dpeeps.com)
[Michael Taggart](michael@dpeeps.com)

## License
```dExg``` is covered under the [MIT LICENSE](LICENSE.md) and is a copyright of [Peeps](https://dpeeps.com).

## Important
See LICENSE for copyright and license terms. Peeps makes its contribution on a voluntary basis as a member of the Arisen and dBnk communities and is not responsible for ensuring the overall performance of the software or any related applications. We make no representation, warranty, guarantee or undertaking in respect of the software or any related documentation, whether expressed or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose and noninfringement. In no event shall we be liable for any claim, damages or other liability, whether in an act ion of contract, tort or otherwise, arising from, out of or in connection with the software or documentation or the user or other dealings in the software or documentation. Any test results or performance figures are indicative and will not reflect performance under all conditions. Any reference to any third party or third-party product, service or other resource is not an endorsement  or recommendation by Peeps. We are not responsible, and disclaim any and all responsibility and liability, for your user of or reliance on any of these resources. Third-party resources may be updated, changed or terminated at any time, so the information here may be out of date or inaccurate. Any person using or offering this software in connect ion with providing software, goods or services to third parties shall advise such third parties of these license terms, disclaimers and exclusions of liability. Peeps, Peeps Labs, dBnk, Arisen and associated logos are copyright of Peeps. 

Wallets and related components are complex software that requires the highest levels of security. If incorrectly built or used, they may compromise users' private keys and digital assets. Wallet applications and related components should undergo thorough security evaluations before being used. Only experienced developers should work with this software.
