# RFC-0004: Verification Challenge Protocol and Penalty Calibration

* **Status:** Accepted
* **Authors:** Dave Antoine (`@daveantoine12`), Leon Maximilien (`@leonmaxim8`)
* **Category:** Standards Track
* **Created:** 2026-07-14
* **Updated:** 2026-09-18

## Abstract

This document defines the formal protocol for challenging worker nodes, validating zero-knowledge and deterministic compute receipts, and calibrating stake penalties for non-responsive or malicious participants within the Kryvora Network.

## Motivation

Decentralized physical infrastructure requires deterministic verification of worker availability. While periodic heartbeats (RFC-0002) confirm network presence, they do not guarantee execution integrity. This specification defines cryptographic challenge probes executed asynchronously by coordination hubs and validator peers.

## Protocol Lifecycle

The challenge lifecycle consists of four discrete stages:

1. **Challenge Issuance:** The hub or verifier peer issues a randomized challenge payload containing a target nonce and state root hash.
2. **Execution Window:** The target worker node must compute the required state proof and respond within the timeout threshold (500 milliseconds for low-latency workers).
3. **Receipt Validation:** The verifier checks the signature against the worker public ed25519 identity key and verifies state transition validity.
4. **Settlement:** Successful responses reward verification credits. Expired or invalid responses increment failure counters and apply penalty slashing.

## State Transitions

```
[IDLE] ----> (Challenge Issued) ----> [CHALLENGED]
                                            |
         +----------------------------------+----------------------------------+
         |                                                                     |
(Valid Proof < 500ms)                                                 (Timeout / Invalid Proof)
         |                                                                     |
         v                                                                     v
     [VERIFIED]                                                             [SLASHED]
         |                                                                     |
         +-------------------------> [COOLDOWN] <------------------------------+
                                         |
                                (Lease Renewed)
                                         v
                                      [IDLE]
```

## Penalty Calibration

Nodes failing consecutive challenges incur exponential penalties according to the following schedule:

* 1 failure: Warning flag logged in telemetry ledger
* 2 consecutive failures: Temporary 10 minute task quarantine
* 3 consecutive failures: 5 percent stake burn and worker eviction from active routing table

## Security Considerations

To prevent denial of service through challenge flooding, nodes enforce rate limits on incoming challenge probes based on verifier stake weight.
