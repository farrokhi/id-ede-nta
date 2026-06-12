<!-- regenerate: off (set to on to let the I-D template rebuild this file from the draft) -->

# DNS EDE INFO-CODE for Negative Trust Anchor

This is the working area for the individual Internet-Draft, "DNS EDE
INFO-CODE for Negative Trust Anchor".

The draft defines a new Extended DNS Error (EDE) INFO-CODE that a
validating resolver can use to signal that a response bypassed DNSSEC
validation because a Negative Trust Anchor (NTA) was in effect for the
queried name or one of its ancestors. Today there is no standard way to
tell a client that an NTA, rather than a validation failure, is the
reason a response is not DNSSEC-validated; this code point fills that
gap.

* [Editor's Copy](https://farrokhi.github.io/id-ede-nta/#go.draft-farrokhi-dnsop-ede-nta.html)
* [Datatracker Page](https://datatracker.ietf.org/doc/draft-farrokhi-dnsop-ede-nta)
* [Individual Draft](https://datatracker.ietf.org/doc/html/draft-farrokhi-dnsop-ede-nta)
* [Compare Editor's Copy to Individual Draft](https://farrokhi.github.io/id-ede-nta/#go.draft-farrokhi-dnsop-ede-nta.diff)

## Contributing

See the
[guidelines for contributions](https://github.com/farrokhi/id-ede-nta/blob/main/CONTRIBUTING.md).

The contributing file also has tips on how to make contributions, if you
don't already know how to do that.

## Command Line Usage

Formatted text and HTML versions of the draft can be built using `make`.

```sh
$ make
```

This repository is managed with Martin Thomson's
[I-D template](https://github.com/martinthomson/i-d-template). Command
line usage requires that you have the necessary software installed. See
[the instructions](https://github.com/martinthomson/i-d-template/blob/main/doc/SETUP.md).
