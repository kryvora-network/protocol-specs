# RFC-0001: Node Telemetry and Availability Probes

* Status: Accepted
* Category: Standards Track
* Author: Dave Antoine, Leon Maximilien
* Created: 2026-03-19

## Abstract

This document defines the wire format and validation semantics for node availability probes in Kryvora Network. Nodes must emit structured telemetry to demonstrate uptime and operational readiness without leaking internal worker state.

## Protocol Mechanics

Nodes host an HTTP/1.1 endpoint on port 4177 by default. Coordinating hubs issue periodic availability challenges across deterministic intervals.

### Telemetry Payload Schema

```json
{
  "node_id": "0x71C4...3a9F",
  "version": "0.2.0",
  "uptime_sec": 86400,
  "sync_height": 142050,
  "hardware": {
    "cores": 4,
    "memory_mb": 8192
  },
  "timestamp": 1773907200
}
```

### Verification Criteria

1. Timestamps must align within 120 seconds of hub consensus time.
2. Signatures must verify against the registered worker public key.
3. Availability scores decay linearly upon missing three consecutive challenge rounds.

## Security Considerations

Nodes must reject inbound probe challenges lacking valid hub signatures to prevent denial of service vectors.
