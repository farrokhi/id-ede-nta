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
    org: Quad9
    email: babak@quad9.net
  - ins: J. Abley
    name: Joe Abley
    org: Cloudflare
    email: jabley@cloudflare.com
  - ins: S. Neuteboom
    org: Cloudflare
    email: sebastiaan@cloudflare.com

normative:
  RFC8914:

informative:
  RFC4035:
  RFC7646:
  RFC6840:
  RFC7858:
  RFC8484:
  RFC9364:
  I-D.ietf-dnsop-structured-dns-error:
---

--- abstract

This document defines a new Extended DNS Error (EDE) INFO-CODE that a
validating resolver can use to signal that a response bypassed DNSSEC
validation because a Negative Trust Anchor (NTA) was in effect for the
queried name or one of its ancestors.

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

A further goal of this signal is transparency toward end users and
applications.  Section 3.1 of [RFC7646] recommends that operators
disclose the NTAs they have in place, for example on a website, and
notes that no in-band DNS signal exists to indicate that an NTA is in
effect.  This document defines that in-band signal, complementing such
out-of-band disclosure.

## Requirements Language

{::boilerplate bcp14-tagged}

# Extended DNS Error Code TBD - Negative Trust Anchor

The resolver returned an answer that bypassed DNSSEC validation
because a Negative Trust Anchor [RFC7646] was configured for the
queried name or one of its ancestors.  The response is therefore not
DNSSEC-validated and SHOULD be treated by the client as it would treat
any unsigned response.

Because this response bypassed DNSSEC validation, it is not
authentic, and the resolver does not set the AD bit (Section 3.2.3
of [RFC4035]; Section 1.1 of [RFC7646]).  This EDE conveys the
reason the AD bit is unset and does not itself change AD handling.
As with all EDE information, this INFO-CODE is diagnostic; per
Section 6 of [RFC8914] a client MUST NOT use its presence to alter
protocol processing, and relies on the AD bit and its own
validation to determine authentication status.

# Operational Considerations

An operator that applies an NTA SHOULD return this EDE in affected
responses, so that end users and applications can tell that the
response was not DNSSEC-validated because of an operator decision
rather than a validation failure.  This complements, and does not
replace, the disclosure recommended in Section 3.1 of [RFC7646].

The operator MAY use the EXTRA-TEXT field to add context about the
NTA, such as the name at which it was configured, the reason it was
put in place, a reference where more information can be found, or its
expected duration.  As noted in Section 2 of [RFC8914], EXTRA-TEXT is
intended for human consumption; operators SHOULD keep it readable and
SHOULD NOT include private or sensitive information.
[I-D.ietf-dnsop-structured-dns-error] describes a structured
convention for the EXTRA-TEXT field in a related context.

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

The AD bit and this EDE option are both carried between the resolver
and the client without cryptographic protection, so an on-path
attacker could add, remove, or modify either signal.  A client MUST
NOT treat this EDE as overriding the AD bit.  Clients that require
assurance of these signals should use an authenticated and encrypted
transport between client and resolver, such as DNS over TLS [RFC7858]
or DNS over HTTPS [RFC8484].

--- back
