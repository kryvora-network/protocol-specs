# RFC-0003: Hub Coordination and Task Dispatch

* Status: Accepted
* Category: Standards Track
* Author: Dave Antoine
* Created: 2026-06-03

## Abstract

This RFC specifies the remote procedure call (RPC) conventions used between the central coordination hub and distributed worker nodes.

## Communication Standard

Hubs communicate over TLS on port 4188 using JSON-RPC 2.0 payloads.

### Standard Methods

* `kryvora_registerWorker`: Registers new node key with hub.
* `kryvora_submitProof`: Submits task output and cryptographic receipt.
* `kryvora_getNetworkStatus`: Retrieves network-wide worker capacity.
