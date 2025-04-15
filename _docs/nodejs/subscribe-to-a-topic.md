---
layout: default
title: Subscribe to Topics (Node.js)

---

# Subscribe to DPSN Topics

## Overview

This guide shows you how to subscribe to DPSN data streams using the Node.js SDK.


## Available Streams

Visit [DPSN Streams Store](https://streams.dpsn.org) to explore all available data streams you can subscribe to.

Here's a popular example:

- **Top 10 tokens from Binance - Price feed** ([view details](https://streams.dpsn.org/topic/0xe14768a6d8798e4390ec4cb8a4c991202c2115a5cd7a6c0a7ababcaf93b4d2d4))
  
  Subscribe to BTC/USDT price updates from Binance:
  ```ts
  const TOPIC = "0xe14768a6d8798e4390ec4cb8a4c991202c2115a5cd7a6c0a7ababcaf93b4d2d4/BTCUSDT/ticker";
  ```


## Basic Subscription

Follow these steps to subscribe to a DPSN topic:

1. ### Install the package
   
   Install the <a href="https://github.com/dpsn-org/dpsn-client-nodejs" style="color: #0366d6;">DPSN Client SDK</a> using npm or yarn:
   ```shell
   npm install dpsn-client
   # or
   yarn add dpsn-client
   ```

2. ### Import the client
   ```typescript
   import DpsnClient from 'dpsn-client';
   ```


3. ### Initialize the client

   > **Note:** The private key required has to be of an  evm wallet on testnet preferably ,the provided evm-wallet private key is only used for authentication purposes. No on-chain transactions will be executed, and no funds will be accessed.

   ```typescript
   const dpsnService = new DpsnClient(
       "betanet.dpsn.org",     // DPSN network endpoint
       "<YOUR_EVM_WALLET_PRIVATE_KEY>",  // Your EVM wallet private key (for authentication only)
       {
           wallet_chain_type: 'ethereum',
           network: 'testnet'
       }
   );
   ```

4. **Define your topic**
   
   View topic details at [DPSN Streams Store](https://streams.dpsn.org/topic/0xe14768a6d8798e4390ec4cb8a4c991202c2115a5cd7a6c0a7ababcaf93b4d2d4)

   ```typescript
   // Example: Subscribe to BTC/USDT price feed from binance
   const TOPIC = "0xe14768a6d8798e4390ec4cb8a4c991202c2115a5cd7a6c0a7ababcaf93b4d2d4/BTCUSDT/ticker";
   ```

5. ### Create the subscription
   ```ts
   
        await dpsnService.subscribe(TOPIC, (topic, message, packetDetails) => {
            // Handle incoming messages
            console.log('💰 Received Binance Price Update:', message);
        });
        console.log('Successfully subscribed to BTC/USDT price feed');
        
       
   
   ```





