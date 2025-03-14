---
layout: default
title: Node.js SDK
---

# Node.JS SDK Documentation

## Overview

`dpsn-client-nodejs` is an SDK for creating, accessing data streams on DPSN infrastructure. It allows you to connect to a DPSN broker, publish messages to topics, and subscribe to topics to receive messages.

For more information, visit:
* DPSN Official Website
* DPSN Streams Marketplace

## Installation

```bash
npm install dpsn-client
```

## Usage

### Importing the Library

```typescript
import { DpsnClient } from 'dpsn-client';
```

### Initializing the Client

> **Caution:** Ensure you use the appropriate RPC URL provided by DPSN to use the library correctly.

To initialize the DPSN client, create an instance of DpsnClient:

```typescript
const dpsn = new DpsnClient("https://testnet.dpsn.org", "WALLET_PRIVATE_KEY", {
  network: 'testnet',
  wallet_chain_type: 'ethereum',
  rpcUrl: "RPC_URL" // Optional, can be set later using setBlockchainConfig()
}, {
  ssl: true // Optional, defaults to true
});
```

To listen to the DPSN broker connection status, use:

```typescript
dpsn.onConnect((res: any) => console.log(res));
```

For logging errors, use:

```typescript
dpsn.onError((error: any) => console.log("[Error LOG]", error));
```

### Connecting to the DPSN Broker

The `init` method initiates connection to the DPSN broker with optional configuration:

```typescript
await dpsn.init({
  connectTimeout: 5000,
  retryOptions: {
    maxRetries: 3,
    initialDelay: 1000,
    maxDelay: 10000,
    exponentialBackoff: true
  }
});
```

### Setting Blockchain Configuration

You can configure the blockchain connection using the `setBlockchainConfig` method (recommended):

```typescript
dpsn.setBlockchainConfig("RPC_URL", "CONTRACT_ADDRESS");
```

> **Caution:** Ensure you use the contract address provided by DPSN to use the library correctly.

### Purchasing a Topic

To register a new topic in the DPSN infrastructure:

```typescript
const { receipt, topicHash } = await dpsn.purchaseTopic("TOPIC_NAME");
console.log("Purchased topic:", topicHash);
```

### Publishing Messages

To publish a message to a topic, use the publish method:

```typescript
await dpsn.publish("TOPIC_HASH", { key: "value" });
```

### Subscribing to Topics

To subscribe to a topic and handle incoming messages, use the subscribe method:

```typescript
await dpsn.subscribe("TOPIC_HASH", (topic, message, packet) => {
  console.log("Received message:", message);
});
```

### Fetching Owned Topics

To fetch the topics owned by the user, use the fetchOwnedTopics method:

```typescript
const topics = await dpsn.fetchOwnedTopics();
console.log("Owned topics:", topics);
```

### Fetching Topic Price

To fetch the current price of purchasing a topic in the DPSN infrastructure, use the getTopicPrice method:

```typescript
const price = await dpsn.getTopicPrice();
console.log("Topic price:", price);
```

## API Reference

### Classes

#### DpsnClient

##### Constructor

```typescript
constructor(dpsnUrl: string, privateKey: string, chainOptions: ChainOptions, connectionOptions: ConnectionOptions = { ssl: true })
```

##### Methods

* `init(options?: InitOptions): Promise<MqttClient>`
* `onConnect(callback: any): void`
* `onError(callback: any): void`
* `publish(topic: string, message: any, options?: Partial<mqtt.IClientPublishOptions>): Promise<void>`
* `subscribe(topic: string, callback: (topic: string, message: any, packet?: mqtt.IPublishPacket) => void, options?: mqtt.IClientSubscribeOptions): Promise<void>`
* `fetchOwnedTopics(): Promise<string[]>`
* `getTopicPrice(): Promise<ethers.BigNumberish>`
* `purchaseTopic(topicName: string): Promise<{ receipt: ethers.TransactionReceipt; topicHash: string }>`
* `setContractAddress(contractAddress: string): void`
* `setBlockchainConfig(rpcUrl: string, contractAddress: string): ethers.Contract`

### Types

#### ChainOptions

```typescript
interface ChainOptions {
  network: NetworkType;
  wallet_chain_type: string;
  rpcUrl?: string;
}
```

#### ConnectionOptions

```typescript
interface ConnectionOptions {
  ssl?: boolean;
}
```

#### NetworkType

```typescript
type NetworkType = 'mainnet' | 'testnet';
```

#### InitOptions

```typescript
interface InitOptions {
  connectTimeout?: number;
  retryOptions?: {
    maxRetries?: number;
    initialDelay?: number;
    maxDelay?: number;
    exponentialBackoff?: boolean;
  };
}
```

#### DPSNError

```typescript
interface DPSNError {
  code?: string;
  message?: string;
  status?: 'connected' | 'disconnected';
}
```

### Error Codes

* `DPSN_CONNECTION_ERROR`
* `DPSN_PUBLISH_ERROR`
* `DPSN_CLIENT_NOT_INITIALIZED`
* `DPSN_CLIENT_NOT_CONNECTED`
* `DPSN_SUBSCRIBE_ERROR`
* `DPSN_SUBSCRIBE_NO_GRANT`
* `DPSN_SUBSCRIBE_SETUP_ERROR`

## License

This project is licensed under the Apache-2.0 License. 