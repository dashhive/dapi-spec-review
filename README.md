# DAPI Spec Review

Re:
- [HTTPS for DAPI Spec (for external)](https://docs.google.com/document/d/1windZMB8ojj4y-i5WHH8UNvNdw9-nVG8DPfq7Ya404g/edit)
- [Decentralized TLS/HTTPS for DAPI](https://trello.com/c/99FAe1iC/39-decentralized-tls-https-for-dapi)
- [“Research HTTPS for DAPI” bounty
](https://docs.google.com/document/d/14JxxwvM4CeyGyZmiXAwd08fEMJoBkhd6yR1Ve5Pvxi8/edit#)
- [feat: set up SSL/TLS using ZeroSSL REST API #196
](https://github.com/dashevo/dashmate/pull/196)

## Concerns

My primary concern with the HTTPS for DAPI spec is the fundamental misunderstanding of the benefits and detriments of IP-based systems w.r.t. decentralization and security.

- Availability
  - The IPv4 address space is already completely sold out [as of 2018](https://www.theregister.com/2018/04/18/last_ipv4_address/).
  - The IPv6 space is (near) infinitely available and should be used in tandem with IPv4 wherever possible
- Decentralization
  - IP addresses are more centralized than domain names
  - Domain names can be transferred between providers
  - Some TLDs are more censorship resistant than others (i.e. .com vs .io vs .gop)
- Security
  - certificates for IP addresses can be spoofed by *ANY* certificate authority
  - Domain Names can be protected by CAA records, which practically eliminate the ability to spoof certificates in browsers (and some API clients)

My primary concern with the current PR is that it does not use ACME (RFC8555 / Let's Encrypt), but relies on the proprietary ZeroSSL API which cannot be adapted to other providers like the official spec.

My primary concerns with DAPI as a whole are that:
- it isn't well-defined:
  - https://github.com/dashevo/platform/tree/master/packages/dapi
  - https://github.com/dashevo/platform/blob/master/packages/dapi/doc/REFERENCE.md
- it's carrying over the RPC baggage of the original C code rather than bridging it to standard the HTTPS / REST that web developers will be most accustomed to (and which is easier to document and understand).

## Proposal

1. Primary use should be domain names (stay more decentralized, not less)
  - Suggested free domains
  - [CAA](https://en.wikipedia.org/wiki/DNS_Certification_Authority_Authorization) records for security
  - duckdns.org
  - dash.io??
  - Buy domains with Dash
2. Either use [caddy](https://caddyserver.com/download) + Relevant DNS Plugin(s) (simple, lightweight, low effort, probably the right thing) or [greenlock](https://github.com/therootcompany/greenlock.js) (custom node integration, but portable across ACME providers)
