# DNS Traffic Analysis and Investigation

## 1. Overview

This project documents a controlled DNS traffic analysis conducted in a local lab environment. The investigation focused on observing DNS queries and responses between a Windows Client and a Pi-hole DNS server. Wireshark and tcpdump were used to capture and inspect network traffic, while Pi-hole's Query Log was used to verify DNS filtering results. The analysis covered both allowed DNS requests and other requests blocked by Pi-hole Gravity. Particular attention was given to DNS transaction IDs, record types, response codes, returned resource records, and the differences between normal resolution and blocked responses. The objective was to understadn how DNS traffic behaves under these conditions and demonstrate basic network traffic analysis techniques.

## 2. Lab Environment

The lab consisted of two systems connected to the same local network.

| Component | Configuration |
|---|---|
| DNS Server | Kali Linux running Pi-hole |
| Client | Windows 10 Pro |
| DNS Filtering | Pi-hole Gravity |
| Packet capture | tcpdump |
| Packet analysis | Wireshark |

The Windows Client was configured to use the Pi-hole server as its DNS resolver, while DNS queries were generated from the Windows Client using **nslookup.** Traffic was captured on the Kali Linux system and analyzed with Wireshark. Pi-hole's Query Log was used as an additional source of evidence to verify the filtering status of tested domains.

## 3. Analysis Results

### 3.1 Allowed DNS Queries

Three domains were used to test normal DNS resolution:

- example.net
- example.org
- example.edu

DNS queries for both A and AAAA records were generated for each domain. The packet captures showed successful DNS responses for all three domains. The observed responses cointaied:

| Domain | Record Type | Response Code | Answer RRs | Result |
|---|---|---|---|---|
| example.net | A | NOERROR | 2 | Two IPv4 addresses returned |
| example.net | AAAA | NOERROR | 2 | Two IPv6 addresses returned |
| example.org | A | NOERROR | 2 | Two IPv4 addresses returned |
| example.org | AAAA | NOERROR | 2 | Two IPv6 addresses returned |
| example.edu | A | NOERROR | 2 | Two IPv4 addresses returned |
| example.edu | AAAA | NOERROR | 2 | Two IPv6 addresses returned |

Query and response packets were correlated using their DNS transaction IDs, queried domain names and record types.

The results confirmed that Pi-hole successfully forwarded and resolved the tested DNS requests.

### 3.2 Blocked DNS Queries

Another test was performed using doubleclick.net, which is a domain confirmed as blocked by Pi-hole Gravity. Both A and AAAA queries were generated and captured, with the following results:

| Record Type | Response Code | Answer RRs | Returned Value |
|---|---|---|---|
| A | NOERROR | 1 | 0.0.0.0 |
| AAAA | NOERROR | 1 | :: |

The responses were correlated with their corresponding queries using DNS transaction IDs, domain names and record types. Pi-hole's Query Log confirmed that both requests were classified as Blocked (Gravity). Instead of returning the actual IP addresses associated with the domain, Pi-hole returned sinkhole addresses:

- 0.0.0.0 for the A record
- :: for the AAAA record

Although both responses contained the NOERROR status, the requests were still blocked. This demonstrates that a successful DNS protocol response does not necessarily indicate that a domain was allowed.

### 3.3 Additional Observed DNS Traffic

The packet captures also contained DNS traffic generated outside of the primary domain resolution tests.

In the allowed-domain capture, a PTR query was observed before each of the three domain tests. The PTR response resolved the local DNS server's IP address to the hostname pi.hole.

In the blocked-domain capture, the first observed exchange involved a Sentry subdomain. This was followed by a PTR query and response before the doubleclick.net queries.

These additional exchanges were not part of the planned test cases and were excluded from the primary analysis. The PTR queries were consistent with reverse DNS resolution within the local network. The Sentry exchange was treated as incidental traffic. Its presence alone does not indicate malicious activity.

### 3.4 Evidence 

The investigation was documented using selected screenshots from Wireshark and Pi-hole. The screenshots provide visual evidence of:

- Allowed DNS queries and responses
- Blocked DNS responses
- Pi-hole Gravity filtering results

The original PCAP files were retained locally and were not included in the public repository.

## 4. Findings

The analysis produced the following findings:

1. Pi-hole successfully resolved the three tested allowed domains.
2. Both A and AAAA records were returned for each allowed domain.
3. Pi-hole Gravity blocked the tested doubleclick.net requests.
4. Blocked A and AAAA queries returned 0.0.0.0 and :: respectively.
5. DNS responses with the NOERROR status can still represent blocked requests.
6. DNS transaction IDs helped correlate queries and responses, but were used together with domain names and record types rather than as unique identifiers.
7. Additional DNS traffic, including PTR lookups, was identified and distinguished from the primary test traffic.

The results demonstrate how DNS filtering behavior can be identified by examining packet-level responses and comparing them with DNS server logs.

## 5. Limitations

This investigation was conducted in a controlled local lab environment and focused on a limited number of DNS queries. The results represent the behavior observed during these tests and should not be interpreted as a comprehensive assessment of Pi-hole's filtering effectiveness. The analysis was limited to DNS traffic. It did not examine subsequent application layer connections or determine whether the tested domains were associated with malicious activity. The packet captures also contained additional DNS traffic outside of the primary tests, which was identified and excluded from the analysis.

## 6. Conclusion

This project demonstrated the process of capturing and analyzing DNS traffic in a local network environment using tcpdump and Wireshark to inspect queries and responses at a packet level. The analysis identified differences between normal DNS resolution and Pi-hole's blocking behavior.

Comparing packet captures with Pi-hole's Query Log provided additional confirmation of the filtering results. The exercise reinforced practical knowledge of DNS record types, transaction IDs, responde codes, resource records and basic network traffic analysis techniques.
