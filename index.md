---
layout: default
title: Overview
---

# DPSN SDK Documentation

## Decentralized Pub/Sub Network

DPSN (Decentralized Pub/Sub Network) is a revolutionary infrastructure that enables decentralized publish/subscribe messaging across distributed systems. Unlike traditional centralized message brokers, DPSN operates on a peer-to-peer network, providing enhanced reliability, scalability, and security.

### Key Features

- **Decentralized Architecture:** No single point of failure or central authority
- **Cryptographic Security:** End-to-end encryption and secure message delivery
- **Scalable Design:** Horizontal scaling with network growth

## Authentication

DPSN leverages your existing EVM wallet  for secure authentication, but **only** for cryptographic proofs - no blockchain transactions or gas fees are involved. Your wallet is used solely for:

- Message signing to prove ownership
- Cryptographic verification of identity
- Secure key generation

> **Note:** We never initiate any blockchain transactions or request access to your funds. The wallet is used purely for its cryptographic capabilities.

## Available SDKs

<div class="sdk-grid">
<a href="{{site.baseurl}}/docs/nodejs/" class="sdk-card">
<h4>Node.js</h4>
</a>
</div>

