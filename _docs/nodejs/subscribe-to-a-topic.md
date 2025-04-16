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
   
   Install the <a href="https://www.npmjs.com/package/dpsn-client" style="color: #0366d6;">DPSN Client SDK</a> using npm or yarn:
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
   

   To initialize and authenticate with DPSN services, you'll need an EVM wallet private key :
      
   
      Use a evm wallet: Create or use an existing Ethereum-based wallet, preferably on **testnet** for security.<br>
      Get your private key: Export it from your wallet's security settings.<br>
      Configure the client:<br>

   
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

   **Security note**: Your private key is used only for authentication. DPSN won't execute transactions, access funds, modify permissions, or permanently store your key.


4. ### Choose Your Data Stream
   
   Each stream has a unique topic identifier. For example, to subscribe to BTC/USDT price updates from Binance ,[View details](https://streams.dpsn.org/topic/0xe14768a6d8798e4390ec4cb8a4c991202c2115a5cd7a6c0a7ababcaf93b4d2d4):

  
   ```typescript
   
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

## Best Practices
   **Error Handling**: Always implement proper error handling to manage connection issues.<br>
   **Rate Limiting**: Be aware of any rate limits for your subscribed streams.<br>
   **Data Validation**: Validate incoming data before using it in critical applications.<br>
   **Reconnection Logic**: Implement automatic reconnection if the connection drops.<br>


## Need Help?

   For custom stream requirements, lower latency options, or other specialized needs, please reach out to us directly. We offer customized solutions for enterprise clients and high-frequency applications.
   If you need to connect to additional streams or require customization in terms of latency or other parameters, please contact us directly at [kunal@dpsn.org]


