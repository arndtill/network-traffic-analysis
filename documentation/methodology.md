# Methodology

## 1. Lab Preparation

A local DNS testing environment was prepared using a Kali Linux system running Pi-hole and a Windows 10 Pro client connected to the same local network.

The Windows client was configured to send DNS queries directly to the Pi-hole server.

## 2. DNS Traffic Generation

DNS queries were generated from the Windows client using nslookup, with the following domains used for allowed resolution tests:

- example.net
- example.org
- example.edu

A separate test was performed using doubleclick.net, which is confirmed to be blocked by Pi-hole Gravity.

Both A (IPv4) and AAAA (IPv6) queries were observed.

## 3. Packet Capture

DNS traffic was captured on the Kali Linux server using tcpdump. The capture filter was set to port 53 to collect DNS traffic on the selected network interface. Separate PCAP files were used for the allowed-domain and blocked-domain tests to preserve each capture independently.

## 4. Packet Analysis

Wireshark was used to inspect the PCAP files, with their DNS queries and responses.

The following fields were examined:

- Source and destination IP addresses.
- UDP source and destination ports.
- DNS transaction IDs
- Queried domain names.
- DNS record types.
- Response codes.
- Answer resource records (Answer RRs)
- Returned IP addresses.

Query and response packets were correlated using their transaction IDs, domain names and record types.

## 5. DNS filtering Analysis

Allowed and blocked DNS responses were compared to identify differences in Pi-hole's filtering behavior. Its Query Log was also reviewed to verify whether blocked requests were classified as Blocked (Gravity).

## 6. Evidence Collection

Packet captures were saved in PCAP format for analysis and retained locally. Selected Wireshark and Pi-hole screenshots were prepared as public evidence for the repository. Relevant packet details and filtering results were recorded for inclusion in the investigation report.
