sequenceDiagram autonumber Eth Client->>+Arbitrage Engine: The block number of
the earliest common ancestor Arbitrage Engine->>+Arbitrage Engine: Arbitrage
Engine enters fork mode Arbitrage Engine->>+Arbitrage Engine: Reverse out any
state back to the earliest common ancestor Note over Arbitrage Engine, Arbitrage
Engine: Things such as User positions, refunds, stats and so on Eth
Client->>+Arbitrage Engine: Fork blocks in order with txs and receipts etc. loop
each fork block Arbitrage Engine->>+Arbitrage Engine: Analyze txs + receipts
Note over Arbitrage Engine,Arbitrage Engine: Looking for our bundles loop Each
bundle that we identify loop Each tx Arbitrage Engine->>+Arbitrage Engine:
Evalute tx status alt Tx reverted else Tx success Arbitrage Engine->>+Profit
Distributor: Request to refund part of the gas to the User Note over Arbitrage
Engine,Profit Distributor: It may be more efficient to batch up refunds etc. end
end end end Eth Client->>+Arbitrage Engine: A full snapshot of market data for
the new head Arbitrage Engine->>+Arbitrage Engine: Arbitrage Engine leaves fork
mode Profit Distributor->>+Profit Distributor: wait n confirmations Profit
Distributor->>+Eth Client: A series of txs for refunding gas to Users Note over
Profit Distributor,Eth Client: If User refund is ever something other than
Ether, perhaps the MEV relay would be better for this
