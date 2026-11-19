---
title: "QLOG extension for TLS events"
abbrev: "qlog-tls"
category: info

docname: draft-huitema-quic-qlog-tls-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Web and Internet Transport"
workgroup: "QUIC"
keyword:
 - QUIC
 - TLS
 - QLOG
venue:
  group: "QUIC"
  type: "Working Group"
  mail: "quic@ietf.org"
  github: "huitema/qlog-tls-extension"

author:
 -
    fullname: Christian Huitema
    organization: Private Octopus Inc.
    email: huitema@huitema.net

normative:

   RFC9000:
   RFC9001:
   RFC9846:

   I-D.ietf-quic-qlog-main-schema:

informative:

...

--- abstract

When logging QUIC connections using QLOG, we want to capture TLS events
that help understand the behavior of the connection, such as for
example SNI selection or ALPN negotiation. This draft lists the
relevant events and proposes logging extensions.

--- middle

# Introduction {#intro}

This document defines a qlog event schema (Section 8 of
{{I-D.ietf-quic-qlog-main-schema}})
containing concrete events for the TLS events impacting
QUIC connections {{RFC9000}}. QUIC use TLS 1.3 ({{RFC9846}}) as defined in {{RFC9001}}.
In addition to session establishment and key negotiation, QUIC connections
use elements like Service Name Identification (SNI) or Application
Layer Protocol Name (ALPN) that are obtained through the TLS exchange.

The event namespace with identifier tls is defined; see {{event-schema}}.
In this namespace multiple events derive from the qlog abstract Event
class (Section 7 of {{I-D.ietf-quic-qlog-main-schema}}), each extending
the data field and defining their name field values and semantics.
The data types that the events use are defined in {{data-types}}


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Event Schema Definition {#event-schema}

This document describes how the core TLS protocol and selected extensions can be
expressed in qlog using a newly defined event schema. Per the requirements in
(Section 8 of {{I-D.ietf-quic-qlog-main-schema}}), this document registers the
tls namespace. The event schema URI is `urn:ietf:params:qlog:events:tls`.


## Draft Event Schema Identification

This section is to be removed before publishing as an RFC.

Only implementations of the final, published RFC can use the events
belonging to the event schema with the URI
urn:ietf:params:qlog:events:tls.  Until such an RFC exists,
implementations MUST NOT identify themselves using this URI.

Implementations of draft versions of the event schema MUST append the
string "-" and the corresponding draft number to the URI.  For
example, draft 00 of this document is identified using the URI
urn:ietf:params:qlog:events:tls-00.

# QUIC Event Overview

TODO: fill in the names of other events besides ALPN

{{tls-events}} summarizes the name value of each event type that is defined in
this specification.

| Name value                            | Importance |  Definition |
|:--------------------------------------|:-----------| :------------|
| tls:alpn_information         | Core       | {{tls-alpninformation}} |
{: #tls-events title="TLS Events"}

QUIC events extend the `$ProtocolEventData` extension point defined in
{{Section 8.2 of QLOG-MAIN}}. Additionally, they allow for direct extensibility by their use of
per-event extension points via the `$$` CDDL "group socket" syntax, as also
described in {{Section 8.2 of QLOG-MAIN}}.

~~~ cddl
TTLSEventData = TLSALPNInformation

$ProtocolEventData /= TLSEventData
~~~
{: #quicevent-data-def title="TLSEventData definition and ProtocolEventData
extension"}

The concrete TLS event types are further defined below, their type identifier
is the heading name. The subdivisions in sections on Connectivity, Transport,
Security, and Recovery are purely for readability.

# TLS Events {#tls-ev}

TODO: fill in other events besides ALPN

## alpn_information {#tls-alpninformation}

The `alpn_information` event supports Application-Layer Protocol Negotiation
(ALPN) over the QUIC transport; see {{?RFC7301}} and {{Section 8.1 of
RFC9001}}. It has Core importance level.

QUIC endpoints are configured with a list of supported ALPN identifiers. During
the TLS handshake, the client sends its list, and the server chooses one
identifier from both lists and sends it back. If no match is found, the
connection is closed.

ALPN identifiers are byte sequences, which are often valid UTF-8. The
`ALPNIdentifier` type can hold either form: `byte_value` holds the raw bytes,
and `string_value` holds the identifier as text. Implementations SHOULD log at
least one form and MAY log both.

~~~ cddl
TLSALPNInformation = {
    ? server_alpns: [* ALPNIdentifier]
    ? client_alpns: [* ALPNIdentifier]
    ? chosen_alpn: ALPNIdentifier

    * $$tls-alpninformation-extension
}

ALPNIdentifier = {
  ? byte_value: hexstring
  ? string_value: text
}
~~~
{: #tls-alpninformation-def title="TLSALPNInformation definition"}

A client usually logs this event twice: first with `client_alpns` when it
offers its list, then with `chosen_alpn` when it learns the server's choice. It
can instead log a single event with both fields once it learns the choice.

A server logs this event once, after it chooses, with `client_alpns`,
`server_alpns`, and `chosen_alpn`.


# TLS Data Type Definitions {#data-types}

TODO.

# Security Considerations {#security}

TODO Security


# IANA Considerations {#iana}

This document registers a new entry in the "qlog event schema URIs"
registry, which was created in Section 15 of {{I-D.ietf-quic-qlog-main-schema}}):

   Event schema URI:  urn:ietf:params:qlog:events:tls

Event-types: TODO. List the new events that are created here.

Description:  Event definitions related to the use of TLS 1.3 ({{RFC9846}}) as defined in {{RFC9001}}.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
