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
  eIDAS:
     target: https://eur-lex.europa.eu/eli/reg/2024/1183/oj/eng
     title: Regulation (EU) 2024/1183 of the European Parliament and of the Council of 11 April 2024 amending Regulation (EU) No 910/2014 as regards establishing the European Digital Identity Framework

...

--- abstract

Zero Knowledge Proof technology relies on mathematical constructs that enable a sender to proove to a receiver that a piece of information has a certain agreed property without revealing that information or its source to the sender. An example commonly given is the problem of prooving that a subject is older than a certain age without revealing the exact age or any other information to a receiver. This document attempts to catalogue real world usecases for such technology.

--- middle

# Introduction

There are several ways to define the concept of zero knowledge proofs. In this document we will rely on the SPICE architecture document to provide us with the basic terminology and use the following definition: A zero knowledge proof (zkp) is a mechanism by which the holder can proove to the verifier that a statement (S) is true without providing the verifier with any additional information other than the truthfullness of S. A zkp mechanism is usually required to satisfy three properties:

* completeness: if S is true, then a compliant implementation of the mechanism will accept the proof presented by another compliant implementation.
* soundness: a non-compliant holder can't make a compliant implementation accept that S is true when it is in fact false.
* zero knowledge: if S is true, then a verifier is not able to derive any other information than the fact that S is true from the mechanism.

Zero knowledge mechanisms fulfil many privacy requirements, for instance:

* holder-verifier unlinkability
* issuer-verifier unlinkability (aka collusion-resistant unlinkability)

These properties come at a cost in terms of implementation complexity. Zero knowledge proofs are both different from conventional cryptography and often hard to implement and understand. Additionally, the fact that zkp ensures unlinkability means that some use-cases that depend on linkability may not translate into architectures that rely on zkp. This means that even if zkp is deployed, the information required in the proofs (i.e., the S) may in some cases be enough to fully identify the data subject, in which case the deployment of zkp serves little purpose.

This document aims to describe some real-world use cases where the deployment of zkp makes sense from a business and technical perspective.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

We also rely on the terminology from the SPICE architecture document.

# Why deploy ZKP?

ZKP mechanisms are fundamentally about proving claims about data subjects with no (zero) auxiliary information leakage. However, ZKP isn't the only method for addressing information leakage in the three-party architecture. Using [RFC9901], also known as selective disclosure JSON Web Tokens (SD-JWT), it is possible to achieve many, but not all, common privacy requirements. Specifically, it is not possible to prevent information leakage that results from collusion between the issuer and verifier.

In situations where collusion resistance is required and SD-JWTs (and similar solutions) are ineffective, ZKP becomes a clear alternative. However, even in situations where collusion resistance isn't necessary, ZKP is sometimes a convenient alternative to other selective disclosure solutions for operational reasons. For instance, a common way to deploy SD-JWTs is to use so-called batch issuance. In order to achieve verifier-verifier unlinkability, the issuer provides the holder with a set of identical copies (a batch) of a given SD-JWT, each signed with a unique key. Whenever the holder generates a presentation to the verifier, one of the SD-JWTs is "consumed", making them effectively one-time-use.

There are situations where the salted hash approach used in SD-JWTs and mdocs [TK:add ISO reference] provides adequate privacy protection. However, in order to achieve verifier-verifier unlinkability, the issuer creates N copies and N signatures each time an SD-JWT is issued. Since this has to be repeated every time the batch "runs dry" at the holder, the issuer has to perform a large and recurring number of signatures. Depending on how the infrastructure is set up, it may be cheaper from an operational point of view to use a ZKP mechanism, where a single credential can be issued and reused by the holder for as long as it remains valid.

In some jurisdictions, legal requirements are being established to ensure enhanced privacy protection for the use of online services. Some legal regimes establish requirements that imply that only the minimal set of information required for each transaction must be transmitted. In such situations, it may be useful (but certainly not always required) to consider deploying ZKP technology. Some legal requirements take this further and outright require ZKP technology. One such example is the [eIDAS] regulation. However, even in the case of eIDAS, the development of zkp schemes has effectively led to a "soft" requirement until development and standardization efforts have caught up with legal requirements.

# Use cases

## Proof of age

Proof of age means the ability to proove that a subject is above or below a required age. Commonly cited use case include access to mature or age-limited content (eg related to online gambling). Proof of age is also relevant when the subject needs to proove that he or she is below a certain age-limit, for instance when the subject is accessing an online service intended for, and meant to be a safe space for, children.

A proof of age can be produced using a so called range-proof from a credential that contains an authentic date of birth or an authentic age claim for the subject.

## Proof of human

Proof of human means the ability to proove that a subject is human. This could be achived by producing a zkp proof from a credential only given to humans. It is often presumed that a passport or similar travel document, or the Personal Identity Document (PID) credentials envisioned by the eIDAS regulation will only be provided to humans but it is by no means certain that all countries and organizations that issue recognized ICAO travel documents would only issue them to persons (humans). ICAO guidelines make no explicit requirement that only humans are allowed to obtain a passport or an emergency travel document but the term "person" indicates that this is the intent.

The reason proof of human may be interesting follows from the increased use of semi-autonomous agents that sometimes acts independently from, or on loosely formulated instructions (prompts) from a human. It may be useful to distinguish actions taken by a human from actions taken by an agent as the result of a prompt and also from actions taken independently from any human interaction. Such actions are sometimes, but not always recorded together with identifiable information and the use of zkp allows the trust in the "human:ness" to be seperated from any trust in the agent holding the authorzation for the act itself.

## Proof of liveness

In the 3 party model a credential is issued to a digital "wallet" under the control of the subject. Cryptographic controls - aka holder binding - is used to ensure that a given credential is issued to one specific wallet. Cryptographic binding cannot however ensure that the subject is the only one able to control the wallet and it is sometimes necessary to know that a particular subject is in direct control of the wallet device. This is sometimes called liveness checks and is commonly used when issuing credentials based on travel documents, drivers licenses and other documents that carry some form of biometric data that can be matched with a subject. Liveness checks are carried out in order to ensure (to some degree) that the subject is physically present instead of (say) having their actions emulated by software.

A liveness credential is a representation of a recent liveness check and the use of zkp enables a verifier to have confidence in that a trusted liveness verification has been done without providing any additional information. It would be possible to combine the liveness check with an identity verification in the holder to be able to proove that the same individual holding an particular form of identity document (say a PID) was recently present and in control of the holder device.

## Proof of membership

Many relationships can be represented as membership in a group; a subject is a student if they are a member of the group of students (at a particular institution), a subject is Danish if they are member of the group of Danish citizens etc. The tautology aside, proof of membership is a very common requirement for things like discount systems, elligibility for certain business transactions etc. Examples include discounts to senior citizens, proof of a valid drivers license before renting a trailer etc. Many of these cases do not require any proof of identity other than the proof of membership and whatever the economic transaction requires.

# Security Considerations

This document does not specify any protocols or interoperability requirements.

# IANA Considerations

This document has no IANA actions.

--- back

# Acknowledgments
{:numbered="false"}

Peter Altman
Drummond Reed
