## 📚 Summary

> OpenMEV is a (permission-based) RPC network layer connected to MEV-enabled block producer pools. By having a private connection we can enable protocols and dapps a consistent protection against MEV (maximal extracted value). MEV is a category of transactions that include 'sandwich trades, front running, back running, arbitrage, etc'. Additional services can be built and offered, such as 'Pay for Order Flow', 'Account Abstraction (Buterin, EIP 4337), Carrier Transactions, etc. By privatizing user transaction flow we can enable the recapture of arbitrage/slippage back to the originating user trade.

## ✅ Overview

## Comparison

[see docs.openmev.org/compare](https://docs.openmev.org/compare)

## SushiRelay.com - OpenMEV for SushiSwap

[see the available RPC methods](https://docs.sushirelay.com)

[integration branch on the sushiswap frontend](https://github.com/manifoldfinance/sushiswap-interface/tree/feat/openmev-relay)



## 🧰 Specification

Below are select high level sequence diagrams of core workflow / logic of OpenMEV's application bus

### Handling Forks

![](handling_fork.svg)

### Submitting Transactions

![](submit_tx.svg)


### Submitting Bundles

> Note this does not include 'mega bundles' which are different

![](submit_bundle.svg)


### Profit Distribution / Rebating

![](profit_dist.svg)

### 📐 Technical