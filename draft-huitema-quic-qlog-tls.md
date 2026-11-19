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


* TLS Data Type Definitions {#data-types}

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
