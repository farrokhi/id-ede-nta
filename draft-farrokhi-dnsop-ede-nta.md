---
title: "Disclosure of Negative Trust Anchors in DNS Responses"
abbrev: "EDE NTA"
docname: draft-farrokhi-dnsop-ede-nta-latest
category: info
stream: IETF
ipr: trust200902
area: Internet
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
    email: babak@farrokhi.net
  - ins: J. Abley
    name: Joe Abley
    org: Cloudflare
    email: jabley@cloudflare.com
  - ins: S. Neuteboom
    name: Sebastiaan Neuteboom
    org: Cloudflare
    email: sebastiaan@cloudflare.com
---

--- abstract

This document describes a mechanism for disclosing that a Negative
Trust Anchor (NTA) was in effect at the time that a DNS response
was generated, using an Extended DNS Error (EDE).

--- middle

# Introduction

{{!RFC8914}} defines the Extended DNS Error (EDE) mechanism, which
allows DNS servers to encode additional information in a DNS response.

{{!RFC7646}} defines the concept of a DNSSEC Negative Trust Anchor
(NTA), an operational mechanism by which a validating resolver can
be configured to temporarily disable DNSSEC validation for a specific
domain to mitigate misconfiguration.

A resolver with an NTA in effect might send a response that ordinarily
would have been suppressed because of validation failures.  This
document defines a new EDE that can be sent with a response to
indicate that the response was subject to an active NTA.

A further goal of this signal is transparency toward end users and
applications.  Section 3.1 of {{!RFC7646}} recommends that operators
disclose the NTAs they have in place, for example on a website, and
notes that no in-band DNS signal exists to indicate that an NTA is in
effect.  This document defines that in-band signal, complementing such
out-of-band disclosure.

## Terminology

{::boilerplate bcp14-tagged}

This document assumes a familiarity with common DNS terminology as
described in {{!RFC9499}}.

# Extended DNS Error Code TBD - Negative Trust Anchor in Effect

A response that includes one or more instances of EDE TBD was
generated with a covering NTA {{!RFC7646}} in effect.

As with all EDEs, the EDE defined in this document is diagnostic;
per Section 6 of {{!RFC8914}} a client MUST NOT use its presence
to alter protocol processing.  The inclusion of this EDE in a
response does not change AD bit processing in DNS messages in any
way. The only purpose of this EDE is to provide additional information
about the response in which it appears.

This EDE is intended for use in DNS responses sent by a DNS resolver
with a configured NTA and SHOULD NOT be included in other responses.
For example, a DNS response sent by an authoritative-only DNS server,
which does not perform validation and hence has no obvious use for
an NTA, SHOULD NOT include this EDE.

# Operational Considerations

An operator that applies an NTA SHOULD return this EDE in affected
responses, so that end users and applications can tell that the
response may not have been DNSSEC-validated.  This complements, and
does not replace, the disclosure recommended in Section 3.1 of
{{!RFC7646}}.

A response might be affected by an NTA for various reasons, not
just in the case where the QNAME is subordinate to the domain for
which an NTA has been configured. In any case, a resolver MAY include
this EDE on any responses while an NTA is in effect, regardless of
whether the presence of the NTA had a material effect on the contents
of the response.

A resolver with multiple NTAs in place simultaneously MAY include
multiple instances of this EDE, each representing a different NTA.

The operator MAY use the EXTRA-TEXT field to add context about the
NTA, such as the name at which it was configured, the reason it was
put in place, a reference where more information can be found, or
its expected duration.  As noted in Section 2 of {{!RFC8914}},
EXTRA-TEXT is intended for human consumption; operators SHOULD keep
it readable and SHOULD NOT include private or sensitive information.
Structured data MAY be included in the EXTRA-TEXT field, as described
in {{!I-D.ietf-dnsop-structured-dns-error}}.

A resolver with multiple NTAs in place simultaneously MAY include
multiple instances of this EDE in a single response, each corresponding
a different NTA.  In this case, the EXTRA-TEXT field in each instance
of this EDE SHOULD be populated.

# IANA Considerations

IANA has made the following allocation in the "Extended DNS Error
Codes" registry under the "Domain Name System (DNS) Parameters"
registry group:

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
