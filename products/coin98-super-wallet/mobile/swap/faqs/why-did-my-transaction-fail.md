# Why did my transaction fail?

There are many reasons why a transaction on Coin98 Super Wallet may be failed by the following reason:

### **Trading tokens with an insufficient balance of gas fee** <a href="#b3vdq4n4j3vu" id="b3vdq4n4j3vu"></a>

\=> You don’t have the native coin/ token on the corresponding blockchain to pay for the gas fee or the amount of this coin/token is not enough to conduct a swapping order.

Despite the fact that Coin98 has integrated the gas fee optimization mechanism, swapping via AMM goes through many routes and costs a lot of fees. So remember to save yourself a decent amount of this coin/token to avoid interrupting your transaction.

### **The number of tokens is insufficient for swap.** <a href="#id-39lampvlmd9v" id="id-39lampvlmd9v"></a>

\=> You don't have enough coins/ tokens balance compared with your entered amount. Please reduce the token amount (should be smaller a little bit than the quantity you have in the account) or deposit more coins/ tokens to the wallet.

### **Trading tokens with low liquidity or during a volatile market.** <a href="#id-53bq5jnynpxn" id="id-53bq5jnynpxn"></a>

\=> The tokens you're trying to swap are probably small-cap tokens that few people are trading or you're trying to buy or sell tokens during a big price movement.

Although the default Slippage on Coin98 Super Wallet is set at the standard rate of 1%, which is quite safe and applicable to most coins on the market, in the case of volatile markets or low-liquidity pools, you can complete the transactions faster and avoid failures by accepting a higher slippage and slippage tolerance percentage.

### **Trading tokens with too low gas fee** <a href="#ivpggixf609l" id="ivpggixf609l"></a>

\=> Slow gas fee leads to slow network and time-out. Also, transaction fees can be unexpectedly high when the network is congested.

If you have approved your transaction but still failed, you just need to swap with the same amount of token again without spending any extra approval fee. You should refer to the average transaction fees on blockchain explorers to adjust the reasonable gas fee, for example: [Ethereum Gas Tracker ](https://etherscan.io/gastracker)

### Reverted <a href="#reverted" id="reverted"></a>

In the event of a transaction marked as "Reverted," the transaction did not execute and all states have been reverted to the state before the transaction. Error messages may be included as defined in the contract.

![](<../../../../../.gitbook/assets/Screen Shot 2021-12-13 at 13.40.41.png>)

{% hint style="info" %}
**Note**:

* You can check your transaction status on the corresponding Blockchain Explorer such as [**Etherscan**](http://etherscan.io/),[ **BscScan**](https://bscscan.com/),[ **HECOScan**](https://scan.hecochain.com/)...
* Avoid trading when the market has major fluctuations.
* None of the decentralized exchanges could secure a 100% successful transaction. Coin98 Exchange offers the Gas Engine mechanism which helps to optimize the trading fee and reduces failed transactions.
* The gas fee and the processing time are different on each blockchain. You need to double-check the information carefully before approving any transaction. For example, BNB (for PancakeSwap), ETH (for UniSwap, SushiSwap), HT (for MDEX), etc
{% endhint %}
