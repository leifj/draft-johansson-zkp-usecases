---
title: "Use cases for Zero Knowledge Proofs"
abbrev: "zkpusecases"
category: info

docname: draft-johansson-zkp-usecases-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
# area: SEC
# workgroup: WG Working Group
keyword:
 - zero knowledge proof
 - use cases
venue:
#  group:
#  type: Mailing List
#  mail: zip@ietf.org
#  arch: https://example.com/WG
  github: "leifj/draft-johansson-zkp-usecases"
  latest: "https://leifj.github.io/draft-johansson-zkp-usecases/draft-johansson-zkp-usecases.html"

author:
 -
    fullname: Leif Johansson
    organization: SIROS Foundation
    email: leifj@siros.org

normative:

informative:

...

--- abstract

Zero Knowledge Proof technology relies on mathematical constructs that enable a sender to proove to a receiver that a piece of information has a certain agreed property without revealing that information or its source to the sender. An example commonly given is the problem of prooving that a subject is older than a certain age without revealing the exact age or any other information to a receiver. This document attempts to catalogue real world usecases for such technology.

--- middle

# Introduction

There are several ways to define the concept of zero knowledge proofs. In this document we will rely on the SPICE architecture document to provide us with the basic terminology and use the following definition: A zero knowledge proof (zkp) is a mechanism by which the holder can proove to the verifier that a statement (S) is true without providing the verifier with any additional information other than the truthfullness of S. A zkp mechanism is usually required to satisfy three properties:

* completeness: if S is true then a compliant implementation of the mechanism will accept the proof presented by a another compliant implementation.
* soundness: a non-compliant holder can't make a compliant implementation accept that S is true when it is in fact false.
* zero knowledge: if S is true then a verifier is not able to derive any other information than the fact that S is true from the mechanism.

Zero knowledge mechanisms fulfil many privacy requirements for instance:

* holder-verifier unlinkability
* issuer-verifier unlinkability (aka collusion-resistant unlinkability)

These properites come at a cost and zero knowledge proofs are both different from conventional cryptography aswell as often hard to implement and understand. Additionally, the fact that zkp ensures unlinkability means that some use-cases that depend on linkability may not translate into architectures that rely on zkp. This means that even if zkp is deployed, the information present in the proofs (ie the statement S) may in some cases be enough to fully identify the data subject in which case the deployment of zkp serves little purpouse.

This document aims to describe some real-world usecases where the deployment of zkp makes sens from a business and technical perspective.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

We also rely on the terminilogy from the SPICE architecture document.

# Why deploy zkp?

Zero Knownledge Proof mechanisms are fundamentally about proving claims about data subjects with no (zero) auxilliary information leakage. However zkp isn't the only method for addressing information leakage in the 3 party architecture. Using [RFC9901], aka selective disclosure json web tokens (SD-JWT) it is possible to achive many but not all common privacy requirements. Specifically it is not possible to prevent information leakage that results from the issuer and verfier colluding.

In situations where collusion resistance is needed and SD-JWTs (and similar solutions) don't work zkp becomes a clear alternative. However even in situations where collusion resistance isn't necessary, zkp is sometimes a convenient alternative to other selective disclosure solutions for operational reasons. For instance, a common way to deploy SD-JWTs is to use so called batch issuance. In order to achive holder-verifier unlinkability, the issuer provides the holder with a set of identical copies (a batch) of a given SD-JWT each signed with a unique key. Whenever the holder generates a presentation to the verifier one of the SD-JWTs is "consumed", making them effectively one-time-use.

There are many situations where this provides adequate privacy protection. However, since the issuer creates N copies and N signatures each time a SD-JWT is issued and since this has to be repeated every time the batch "runs dry" at the holder, the issuer has to perform a large and recurring number of signatures. Depending on how the infrastructure is setup, it may simply be cheaper from an operational point of view to use a zkp mechanism where a single credential can be issued and re-used by the holder for as long as it is valid.

## Regulatory requirements

## Business requirements

## Cross-realm trust

# Use cases

## Proof of age

## Proof of human

## Proof of liveness

## Proof of membership

## Revocation

# Security Considerations

This document does not specify any protocols or interoperability requirements.

# IANA Considerations

This document has no IANA actions.

--- back

# Acknowledgments
{:numbered="false"}

Peter Altman
Drummond Reed
