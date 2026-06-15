---
title: "DNS EDE INFO-CODE for Negative Trust Anchor"
abbrev: "EDE NTA"
docname: draft-farrokhi-dnsop-ede-nta-latest
category: info
stream: IETF
ipr: trust200902
area: Internet
workgroup: DNSOP Working Group
keyword:
  - DNS
  - EDE
  - Extended DNS Errors
  - DNSSEC
  - NTA
  - Negative Trust Anchor

author:
  - ins: B. Farrokhi
    name: Babak Farrokhi
    org: "Perforlabs"
    email: "babak@perforlabs.com"
  - ins: J. Abley
    name: Joe Abley
    org: Cloudflare
    email: jabley@cloudflare.com
---

--- abstract

This document defines a new Extended DNS Error (EDE) INFO-CODE that
a validating resolver can include in a response to signal that
validation was not performed because a Negative Trust Anchor (NTA)
was in effect.

--- middle

# Introduction

{{!RFC8914}} defines the Extended DNS Error (EDE) mechanism, which
allows DNS servers to encode additional information in a DNS response.

{{?RFC7646}} defines the concept of a DNSSEC Negative Trust Anchor
(NTA), an operational mechanism by which a validating resolver can
be configured to temporarily disable DNSSEC validation for a specific
domain to mitigate misconfiguration.

A resolver with an NTA in effect might send a response that ordinarily
would have been suppressed because of validation failures.  This
document defines a new EDE INFO-CODE that can be sent with a response
to indicate that the response was subject to an active NTA.

## Terminology

{::boilerplate bcp14-tagged}

This document assumes a familiarity with common DNS terminology as
described in {{!RFC9499}}.

# Extended DNS Error Code TBD - Negative Trust Anchor in Effect

A response that includes EDE TBD was generated with a covering NTA
{{!RFC7646}} in effect; that is, an NTA for a domain was configured
on the resolver and the QNAME is subordinate to that domain.
Validation of the data contained within the response has not taken
place.

The EXTRA-TEXT field of the EDE MAY contain the domain name for
which the NTA was configured, as an aid to troubleshooting.

As with all EDE information, this INFO-CODE is diagnostic; per
Section 6 of {{!RFC8914}} a client MUST NOT use its presence to
alter protocol processing.  The presence of this EDE in a response
MUST NOT modify AD bit processing in DNS messages. The only purpose
of this EDE is to provide additional information about the response
in which it appears.

This EDE is intended for use in DNS responses sent by a DNS resolver
with a configured NTA and SHOULD NOT be included in other responses.
For example, a DNS response sent by an authoritative-only DNS server,
which does not perform validation and hence has no obvious use for
an NTA, SHOULD NOT include this EDE.

# IANA Considerations

IANA has made the following allocation in the in "Extended DNS Error
Codes" registry under the "Domain Name System (DNS) Parameters"
registry group, with the following values:

| INFO-CODE | Purpose                | Reference     |
|-----------|------------------------|---------------|
| TBD       | Negative Trust Anchor  | This document |


# Security Considerations

An NTA represents an intentional, operator-driven suspension of
DNSSEC validation for a specific domain. See {{!RFC7646}} for further
discussion of the operational and security implications of NTAs.

The presence of the EDE defined in this document does not modify
AD bit processing in DNS messages, and its only purpose is to provide
additional information.

EDEs encoded in DNS messages carry no cryptographic signatures and
hence enjoy no inherent integrity protection.  An on-path attacker
could add, remove, or modify an EDE.  Clients that require integrity
protection of these signals should use an authenticated and encrypted
transport between client and resolver, such as DNS over TLS
{{?RFC7858}} or DNS over HTTPS {{?RFC8484}}.  See Section 6 of
{{!RFC8914}} for more discussion.

--- back

# Acknowledgements

Your name here, etc.

