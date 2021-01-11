# Cloud DNS
https://cloud.google.com/dns/docs/overview

- DNS is a hierarchical distributed database that lets you store IP addresses and other data, and look them up by name
- Cloud DNS lets you publish your zones and records in DNS without the burden of managing your own DNS servers and software
- offers both public zones and private managed DNS zones
  - A public zone is visible to the public internet
  - a private zone is visible only from one or more Virtual Private Cloud (VPC)

### Key Temrms
https://cloud.google.com/dns/docs/key-terms

### Shared VPC considerations

- you must create the zone in the host project, and then add one or more Shared VPC networks to the list of authorized networks for that zone.

### DNS forwarding methods

- Google Cloud offers inbound and outbound DNS forwarding for private zones
  - Inbound
    - Create an inbound server policy to enable an on-premises DNS client or server to send DNS requests to Cloud DNS
    - The DNS client or server can then resolve records according to a VPC network's name resolution order
  - Outbond
    - Send DNS requests to DNS name servers of your choosing
      - located in the same VPC network
      - in an on-premises network
      - or on the internet
    - Resolve records hosted on name servers configured as forwarding targets of a forwarding zone authorized for use by your VPC network
    - Create an outbound server policy for the VPC network to send all DNS requests an alternative name server
      - When using an alternative name server, VMs in your VPC network are no longer able to resolve records in Cloud DNS private zones, forwarding zones, or peering zones
- You can simultaneously configure inbound and outbound DNS forwarding for a VPC network
  - lets VMs in your VPC network resolve records in an on-premises network or in a network hosted by a different cloud provider
  - enables hosts in the on-premises network to resolve records for your Google Cloud resources

### PTR records for RFC 1918 addresses in private zones
https://cloud.google.com/dns/docs/overview#ptr_records_in_private_zones

### Supported DNS record types
 A, AAAA, CAA, CNAME, DNSKEY, DS, IPSECKEY, MX, NAPTR, NS, PTR, SOA, SPF, SRV, SSHFP, TLSA, TXT

### Wildcard DNS records

Wildcard records are supported for all record types, except for NS records.

### DNSSEC
- Cloud DNS supports managed DNSSEC, protecting your domains from spoofing and cache poisoning attacks
- DNSSEC provides strong authentication (but not encryption) of domain lookups

### Forwarding zones
- configure target name servers for specific private zones
- one way to implement outbound DNS forwarding from your VPC network
- 