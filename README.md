# Manifold Finance - OpenMEV Platform Overview

---

> Documentation for Integration Partners, Application Partners, End Users and Searchers

> v2022.01.09

## Table of Contents

- [📚 Summary](#---summary)
- [✅ Overview](#--overview)
- [Comparison](#comparison)
- [SushiRelay.com - OpenMEV for SushiSwap](#sushirelaycom---openmev-for-sushiswap)
- [🧰 Specification](#---specification)
  * [Dashboard](#dashboard)
      - [Grafana - 1](#grafana---1)
      - [Grafana - 2](#grafana---2)
  * [Design goals](#design-goals)
  * [Architecture Goals](#architecture-goals)
    + [Redundant resources (trade cost)](#redundant-resources--trade-cost-)
    + [Degraded results (trade quality)](#degraded-results--trade-quality-)
    + [Retry transient failures (trade latency)](#retry-transient-failures--trade-latency-)
- [Application State / Workflows](#application-state---workflows)
  * [Handling Forks](#handling-forks)
  * [Submitting Transactions](#submitting-transactions)
  * [Submitting Bundles](#submitting-bundles)
  * [Profit Distribution / Rebating](#profit-distribution---rebating)
  * [📐 Technical Integration / SDK](#---technical-integration---sdk)
    + [Protobuf Schemas](#protobuf-schemas)
    + [Ethers.js Web3 Provider](#ethersjs-web3-provider)



## 📚 Summary

> OpenMEV is a (permission-based) RPC network layer connected to MEV-enabled block producer pools. By having a private connection we can enable protocols and dapps a consistent protection against MEV (maximal extracted value). MEV is a category of transactions that include 'sandwich trades, front running, back running, arbitrage, etc'. Additional services can be built and offered, such as 'Pay for Order Flow', 'Account Abstraction (Buterin, EIP 4337), Carrier Transactions, etc. By privatizing user transaction flow we can enable the recapture of arbitrage/slippage back to the originating user trade.

## ✅ Overview



**[docs.openmev.org](https://docs.openmev.org)**

## Comparison

[see docs.openmev.org/compare](https://docs.openmev.org/compare)

> Note, we are in the process of overhauling our documentation process - please only refer to docs.openmev.org, docs.sushirelay.com or this repo

## SushiRelay.com - OpenMEV for SushiSwap

[see the available RPC methods](https://docs.sushirelay.com)

[integration branch on the sushiswap frontend](https://github.com/manifoldfinance/sushiswap-interface/tree/feat/openmev-relay)


## 🧰 Specification

> Engineering Goals and Specification

OpenMEV is built ontop of our 'backbone platform'. Backbone is built on baremetal instances, with OVH, equinix bare metal, and hetzner as the main providers. Nix/Kotlin/Kubernetes/kdb+/redpanda are the primary tech stack components.

### Dashboard

A Grafana dashboard utilizing OAuth2 via GitHub is available for partners and integration users.

Example screenshots with information redacted are provided below

##### Grafana - 1

> Click To Open Image

<details>
<summary>Dashboard Overview</summary>

![](grafana1.png)

</details>



##### Grafana - 2

> Click To Open Image

<details>
<summary>RPC Overview</summary>

![](grafana2.png)

</details>



### Design goals

- **Pre-trade privacy**
Pre-trade privacy implies transactions only become publicly known after they have been included in a block. Note, this type of privacy does not exclude privileged actors such as transaction aggregators / gateways / miners.
- **Failed trade privacy**
Failed trade privacy implies loosing bids are never included in a block, thus never exposed to the public. Failed trade privacy is tightly coupled to extraction efficiency.
- **Complete privacy**
Complete privacy implies there are no privileged actors such as transaction aggregators / gateways / miners who can observe incoming transactions.
- **Finality**
Finality implies it is infeasible for MEV extraction to be reversed once included in a block. This would protect against time-bandit chain re-org attacks.

### Architecture Goals

#### Redundant resources (trade cost)
Having redundant resources to avoid single points of failure.
 Every component can fail, but the system is robust enough that an individual outage can be tolerated.

#### Degraded results (trade quality)
For some services, it might be acceptable to trade quality for reliability. 
Instead of expecting every transaction to succeed, it can be tolerable for a business to see some requests fail.

#### Retry transient failures (trade latency)
If a response isn't returned in the expected time, the system sends the same request again.

## Application State / Workflows

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

### 📐 Technical Integration / SDK

#### RPC

##### HTTPS
https://api.sushirelay.com/v1

##### WebSocket

```sh
$ wscat -c wss://api.sushirelay.com/v1
```
```sh
< {"method":"manifold_motd","jsonrpc":"2.0","params":{"result":{"notice":"THIS IS A NOTICE OF MONITORING OF MANIFOLD FINANCE, INC NETWORK INFORMATION SYSTEMS  By logging into Manifold Finance, Inc computer systems, you acknowledge and consent to monitoring of this system.  Network Policy <https://docs.manifoldfinance.com/network/policy>  By using this network, you certify that you have read, understand, and agree to abide by the Rules of Behavior for Manifold Finance Network Platform."}}}
>
```


We provide:

- nodejs sdk / npm package
- Direct Websocket / authenticated endpoint
- Kafka Broker / Site to Site connection 
- Protobuf schema [see openmev/protobufs](https://github.com/manifoldfinance/openmev-sdk/tree/master/packages/protobufs/src)

Additionally we provide for operations:

- OAuth2 Auth / user management (GitHub)
- OTEL Tracing 
- Green/Blue deploys for testing 



> Note, we are in the process of consolidating and moving to a new git repo
> https://github.com/manifoldfinance/disco3-react

Disco3 will provide the same functionality as @openmev/sdk-connect but with 
additional components / tooling for easier DApp integrations. It uses the same interfaces
as web3-react, except provides modern implementations and solutions (better state management, strict dependency enforcement, etc)


#### Protobuf Schemas

https://github.com/manifoldfinance/openmev-sdk/tree/master/packages/protobufs/src

#### Ethers.js Web3 Provider

@openmev/sdk-connect
https://github.com/manifoldfinance/openmev-sdk/tree/master/packages/sdk-connect

## Roadmap

- Carrier Transaction support
- Account Abstraction Support (EIP 4337)
- Real Time Protocol Monitoring and Alerts
- Multichain support (Avalanche, Fantom, Polkadot - subjet to change)

- **[Manifold Finance GitHub Project Board - Roadmap, issues, etc](https://github.com/orgs/manifoldfinance/projects/8) 
> note: does not include private repo tracking for now!
