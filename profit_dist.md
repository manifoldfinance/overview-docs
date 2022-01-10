sequenceDiagram autonumber Eth Client->>+Arbitrage Engine: Chain state for
latest block Arbitrage Engine->>+Arbitrage Engine: Analyze txs + receipts Note
over Arbitrage Engine,Arbitrage Engine: Looking for our bundles loop Each bundle
that we identify loop Each tx Arbitrage Engine->>+Arbitrage Engine: Evalute tx
status alt Tx reverted else Tx success Arbitrage Engine->>+Profit Distributor:
Request to refund part of the gas to the User Note over Arbitrage Engine,Profit
Distributor: It may be more efficient to batch up refunds etc. end end end
Profit Distributor->>+Profit Distributor: wait n confirmations Profit
Distributor->>+Eth Client: A series of txs for refunding gas to Users Note over
Profit Distributor,Eth Client: If User refund is ever something other than
Ether, perhaps the MEV relay would be better for this
