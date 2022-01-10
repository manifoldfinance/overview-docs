sequenceDiagram autonumber User->>+Web3: submit tx bundle with target block
number Web3->>+Web3: validate target number is for a block that has not been
mined alt Target block number has already been mined Web3->>+User: Return error
end loop Each tx in bundle Web3->>+Web3: validate tx is for a supported market
alt Unsupported market Web3->>+User: Return error end

    end
    alt All markets supported and target block has not been mined
        Web3->>+Bundler: forward user bundle
        Bundler-->>-Web3: ack user bundle
        Web3-->>-User: ack tx bundle
        Bundler->>+Bundler: Aggregate & partition by (block number, market)
        Note over Bundler,Bundler: User bundles are preserved atomically
        Bundler->>+Arbitrage Engine: forward one or more bundles
        Eth Client->>+Arbitrage Engine: Target block number parent is mined
        Note over Eth Client,Arbitrage Engine: Latest market state is included in this update
        loop For each bundle targetting the next block number
            Arbitrage Engine->>+Arbitrage Engine: estimate price impact & interleave arbitrage orders
            Arbitrage Engine->>+Arbitrage Engine: determine profitability
            alt Profitable
                Arbitrage Engine->>+Arbitrage Engine: determine miner bribe
                Arbitrage Engine->>+Eth Signer: forward backbone bundle
                Eth Signer->>+Eth Signer: de-reference arbitrage orders for real orders
                Eth Signer->>+Eth Signer: generate miner bribe tx
                Eth Signer->>+MEV Relay: forward bundle
                MEV Relay->>+Miner: relay bundle
            end
        end
    end
