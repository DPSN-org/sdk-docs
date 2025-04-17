---
layout: default
title: Mint a Topic and Publish Data
---

# Mint a Topic and Publish Data

## Overview

This guide shows you how to mint a topic on DPSN infrastructure and publish data to it using the Node.js SDK.

## Understanding DPSN Topics

Topics in DPSN are data distribution channels that enable secure, permissioned data streams. They function as:

* **Ownership-based channels**: Each topic is owned by the wallet that purchases it
* **Data streams**: Publishers push data to topics, and subscribers receive data from topics they're following
* **Permissioned resources**: Only the owner can publish to a topic, while anyone can subscribe to it
* **Authenticated channels**: The blockchain provides the authentication and ownership layer

Think of topics as secure broadcast channels where data integrity and publisher authenticity are guaranteed by blockchain verification.

## How Topic Ownership Works

When you purchase a topic:

1. Your wallet initiates a blockchain transaction to the DPSN smart contract on the Base Sepolia network.
2. This transaction:  
   * Requires at least **0.002 Base Sepolia ETH**
   * Associates the topic name with your wallet's address in the contract  
   * Generates a unique topicHash that becomes owned by your wallet
3. Authentication works through:  
   * Your wallet's private key signing messages when publishing  
   * The dpsn nodes verifying that only the registered owner can publish to that topic
4. When publishing data, the publishing client uses the same private key that purchased the topic to authenticate the request, ensuring only authorized wallets can publish to their owned topics.

## Prerequisites

* DPSN URL: betanet.dpsn.org
* DPSN Smart Contract Address: 0xC4c4CFCB2EDDC26660850A4f422841645941E352 (on Base Sepolia testnet)
* Wallet private key: Your private key for a wallet on Base chain
* BASE RPC URL: URL from any Base testnet RPC provider
* Minimum balance: 0.002 Base Sepolia ETH to register a topic

## Step-by-Step Guide

Follow these steps to mint a topic and publish data:

1. ### Install the package
   
   Install the <a href="https://www.npmjs.com/package/dpsn-client" style="color: #0366d6;">DPSN Client SDK</a> using npm or yarn:
   ```shell
   npm install dpsn-client
   # or
   yarn add dpsn-client
   ```

2. ### Import the client
   ```typescript
   import { DpsnClient } from 'dpsn-client';
   ```

3. ### Initialize the client
   
   To initialize and authenticate with DPSN services, you'll need an EVM wallet private key:
      
      Use a evm wallet: Create or use an existing Ethereum-based wallet, preferably on **testnet** for security.<br>
      Get your private key: Export it from your wallet's security settings.<br>
      Configure the client:<br>

   ```typescript
   const dpsnClient = new DpsnClient(
       "https://betanet.dpsn.org",  // DPSN network endpoint
       "<YOUR_EVM_WALLET_PRIVATE_KEY>",  // Your EVM wallet private key (for authentication only)
       {
           network: 'testnet',
           wallet_chain_type: 'ethereum'
       }
   );
   ```

  

4. ### Configure Blockchain Settings
   ```typescript
   // Set blockchain configuration
   dpsnClient.setBlockchainConfig(
       "YOUR_TESTNET_BASE_RPC_URL",           // Your TESTNET_BASE_RPC URL
       "0xC4c4CFCB2EDDC26660850A4f422841645941E352"   // DPSN contract address
   );
   ```

5. ### Check Topic Price
   ```typescript
   // Get the current price for minting a topic
   const price = await dpsnClient.getTopicPrice();
   console.log("Topic price:", price.toString());
   ```

6. ### Mint (Purchase) a Topic
   ```typescript
   try {
       // Mint a new topic
       const { receipt, topicHash } = await dpsnClient.purchaseTopic("YOUR_TOPIC_NAME");
       console.log("Successfully minted topic:", topicHash);
       console.log("Transaction receipt:", receipt);
   } catch (error) {
       console.error("Failed to mint topic:", error);
   }
   ```

7. ### Fetch Your Owned Topics
   ```typescript
   // Fetch all topics you own
   const ownedTopics = await dpsnClient.fetchOwnedTopics();
   console.log("Your topics:", ownedTopics);
   ```

8. ### Publish Data to Your Topic
   ```typescript
   try {
       // Publish data to your topic
       // A message can be JSON, string, or binary data
       const message = {
           timestamp: Date.now(),
           data: "Your message content",
           metadata: {
               source: "your-app",
               version: "1.0"
           }
       };

       await dpsnClient.publish(topicHash, message);
       console.log("Successfully published message to topic:", topicHash);
   } catch (error) {
       console.error("Failed to publish message:", error);
   }
   ```


## Best Practices

**Error Handling**: Always implement proper error handling for blockchain transactions.<br>
**Gas Management**: Monitor gas prices and set appropriate gas limits for transactions.<br>
**Data Validation**: Validate data before publishing to ensure it meets your topic's requirements.<br>
**Transaction Monitoring**: Track transaction status and implement retry mechanisms for failed transactions.<br>

## Need Help?

For custom topic requirements, lower latency options, or other specialized needs, please reach out to us directly. We offer customized solutions for enterprise clients and high-frequency applications.

If you need assistance with topic minting or require customization in terms of latency or other parameters, please contact us directly at [kunal@dpsn.org]
