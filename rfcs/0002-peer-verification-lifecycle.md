# RFC-0002: Peer Verification and Heartbeat Lifecycle

* Status: Accepted
* Category: Core Protocol
* Author: Leon Maximilien
* Created: 2026-04-25

## Abstract

This specification details the peer discovery, gossip propagation, and heartbeat validation mechanisms utilized by worker nodes to maintain topology consensus.

## Heartbeat Mechanics

Workers ping designated hub endpoints at a default frequency of 30 seconds. Pings contain cryptographic attestations confirming that the host daemon remains responsive and ready to accept task assignments.

### State Transitions

* Connected: Node successfully handshake with hub endpoint.
* Active: Regular heartbeats received within expected window.
* Degraded: One or two heartbeat windows dropped due to latency.
* Pruned: Inactive for more than 180 seconds.
