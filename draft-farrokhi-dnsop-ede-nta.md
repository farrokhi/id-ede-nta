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

normative:
  RFC8914:

informative:
  RFC7646:
  RFC6840:
  RFC9364:
---

--- abstract

This document defines a new Extended DNS Error (EDE) INFO-CODE that a
validating resolver can use to signal that a response bypassed DNSSEC
validation because a Negative Trust Anchor (NTA) was in effect for the
queried name or an ancestor thereof.

--- middle

# Introduction

[RFC8914] defines the Extended DNS Error (EDE) mechanism, which allows
DNS servers to convey additional information about the cause of a DNS
response.  [RFC7646] defines the concept of a DNSSEC Negative Trust
Anchor (NTA), an operational mechanism by which a validating resolver
can be configured to temporarily disable DNSSEC validation for a
specific domain to mitigate misconfiguration.

When a resolver returns an answer that would otherwise have been
marked as bogus, but instead returns it as insecure because an NTA is
in effect, there is currently no standard way to signal this fact to
the client.  This document defines a new EDE INFO-CODE to convey that
a Negative Trust Anchor was applied to the response.

## Requirements Language

{::boilerplate bcp14-tagged}

# Extended DNS Error Code TBD - Negative Trust Anchor

The resolver returned an answer that bypassed DNSSEC validation
because a Negative Trust Anchor [RFC7646] was configured for the
queried name or an ancestor thereof.  The response is therefore not
DNSSEC-validated and SHOULD be treated by the client as it would treat
any unsigned response.

The EXTRA-TEXT field MAY contain the name at which the NTA was
configured, to aid operator troubleshooting.

# IANA Considerations

IANA is requested to allocate a new code in the "Extended DNS Error
Codes" registry under the "Domain Name System (DNS) Parameters"
registry group, with the following values:

| INFO-CODE | Purpose                | Reference     |
|-----------|------------------------|---------------|
| TBD       | Negative Trust Anchor  | This document |

Note that the policy for the "Extended DNS Error Codes" registry is
"First come, first served" so this document is not strictly necessary
for the code point to be assigned.

# Security Considerations

EDE options are sent with EDNS and are not signed.  They should be
used with care; see Section 6 of [RFC8914] for general EDE security
considerations.

An NTA represents an intentional, operator-driven suspension of DNSSEC
protection for a specific name.  Clients receiving this EDE should be
aware that the accompanying response has not benefited from DNSSEC
validation and should make trust decisions accordingly.  See [RFC7646]
for further discussion of the operational and security implications of
NTAs.

--- back
