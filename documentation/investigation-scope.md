# Investigation Scope and Lab Environment 

## 1. Overview

This project uses a small local network to capture and analyze DNS traffic between a Windows client and a Pi-hole DNS server running on Kali Linux.

## 2. Lab Components

| Component | Operating System and/or Tool | Role |
|---|---|---|
| DNS Server | Kali Linux and Pi-hole | Receives DNS queries and applies domain filtering |
| Client | Windows 10 Pro | Generates DNS queries for testing |
| Packet Capture | tcpdump | Captures DNS traffic on the server's Ethernet interface |
| Traffic Analysis | Wireshark | Inspects DNS packets, queries, responses and transaction IDs |

## 3. Network Configuration

The Client and DNS server are connected to the same local network.

The Windows client sends DNS queries directly to the Pi-hole server, which listens on its Ethernet interface and processes the requests.

Private network addresses are omitted from the document to avoid exposing unnecessary details about the local network.

## 4. DNS Server Configuration

Pi-hole is configured as the DNS resolver for the lab environment.

Its filtering functionality uses Gravity blocklists to identify and block selected domains.

The server is also configured with an upstream DNS resolver to handle queries that are not blocked.

## 5. Testing environment

Two types of DNS requests were generated:

- **Allowed queries:** Requests for example.net, example.org and example.edu.
- **Blocked queries:** Requests for doubleclick.net, which is a domain confirmed as blocked by Pi-hole Gravity.

The tests included both A (IPv4) and AAAA (IPv6) record queries.

## 6. Scope and Limitations

This is a controlled educational lab conducted on a local network. The results reflect the behavior observed during the tests and do not represent a complete assessment of the network's security. No malicious activity or real security incident was simulated.
