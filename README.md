# Unified ID 2.0 Overview
English | [Japanese](README-ja.md)

This page provides the following information about the Unified ID 2.0 (UID2) framework:
- [Introduction](#introduction)
- [UID2 Infrastructure](#uid2-infrastructure)
  - [UID2 Identifier Types](#uid2-identifier-types)
  - [Components](#components)
  - [Participants](#participants)
  - [Workflows](#workflows)
- [FAQs](#faqs)
- [License](#license)

For integration guides, supported SDKs, and endpoint reference, see [Getting Started](/api/README.md).

## Introduction

UID2 is a framework that enables deterministic identity for advertising opportunities on the open internet for many [participants](#participants) across the advertising ecosystem. The UID2 framework enables logged-in experiences from publisher websites, mobile apps, and Connected TV (CTV) apps to monetize through programmatic workflows. Built as an open-source, standalone solution with its own unique namespace, the framework offers the user transparency and privacy controls designed to meet local market requirements. 

>NOTE: The term "UID2" can refer to either the framework or an actual identifier. Unless otherwise indicated, this page provides an overview of the UID2 framework. 

### Guiding Principles

The UID2 framework has the following principles as its foundation:

- **First-party relationships**: UID2 enables advertisers to activate their first-party data on publisher websites across the open internet.

- **Non-proprietary (universal) standard**: All [participants](#participants) in the advertising ecosystem who agree toabide by the code of conduct can access UID2.

- **Open source**: The source code for the UID2 [components](#components) is publicly available.

- **Interoperable**: The framework allows other identity solutions (commercial and proprietary) to integrate and provide UID2 tokens with their offerings.

- **Secure and encrypted data**: UID2 leverages multiple layers of security to protect user and other participant data.

- **Consumer control**: Consumers can opt out of UID2 at any time through the [Transparency and Control Portal](https://transparentadvertising.org).

### Technical Design Principles

The UID2 framework is built on the following technical principles:

- **Distributed integration**: Multiple certified integration paths provide options for publishers, advertisers, and third-party data providers to manage and exchange UID2 tokens.

- **Decentralized storage**: The framework does not have a centralized storage for personal data mappings. All participants maintain only their own data.

- **Lean infrastructure**: The UID2 system is light and inexpensive to operate.

- **Internet scale**: The UID2 infrastructure can scale to address the continuously increasing needs of [participants](#participants) and to meet performance demands of specific geographic regions.

- **Self-reliant**: UID2 does not rely on external services for processing of real-time bidding (RTB) data.


## UID2 Infrastructure

The following sections explain and illustrate the key elements of the UID2 framework infrastructure:

  - [UID2 Identifier Types](#uid2-identifier-types)
  - [Components](#components)
  - [Participants](#participants)
  - [Workflows](#workflows)

### UID2 Identifier Types

UID2 is a deterministic ID that is based on personally identifiable information (PII), such as email address or phone number. There are two types of UID2s: raw UID2s and UID2 tokens (also known as advertising tokens). The following table explains each type.

| ID Type | Shared in Bid Stream? | Description |
| :--- | :--- | :--- |
| **Raw UID2** | No | An unencrypted alphanumeric identifier created through the UID2 APIs or SDKs with the user's verifiable personal data, such as an email address or a phone number, as input.<br/><br/>To prevent re-identification of the original personal data, each raw UID2 utilizes hashing and salting. Raw UID2s are designed to be stored by advertisers, third-party data providers, and demand-side platforms (DSPs).|
| **UID2 Token (Advertising Token)** | Yes | An encrypted form of a raw UID2. UID2 tokens are generated from hashed or unhashed email addresses or phone numbers that are converted to raw UID2s and then encrypted to ensure protection in the bid stream.<br/><br/>UID2 tokens are designed to be used by publishers or publisher service providers. Supply-side platforms (SSPs) pass UID2 tokens in the bid stream and DSPs decrypt them at bid request time. |


### Components

The UID2 framework consists of the following components, all of which are currently managed by The Trade Desk.

| Component | Description |
| :--- | :--- |
| **Core Service**  | A centralized service that stores salt secrets and encryption keys and manages access to the distributed UID2 system. | 
| **Operator Service**  | A service that enables the management and storage of encryption keys and salts from the UID2 Core Service, hashing of users' personal data, encryption of raw UID2s, and decryption of UID2 tokens. There can be multiple instances of the service (public or private) operated by multiple [participants](#participants), known as operators.<br/><br/>Open operators run publicly available instances of the Operator Service and make them available to all relevant UID2 [participants](#participants). There might also be closed operators that run private instances of the Operator Service exclusively for their own use. All instances are designed with protections to keep critical UID2 data secure and interoperable, regardless of who operates the service. | 
| **Opt-Out Service**  | A global service that manages and stores user opt-out requests and disseminates them to publishers, operator service instances, and DSPs. | 
| **Transparency and Control Portal**  | A user-facing website, [https://transparentadvertising.org](https://transparentadvertising.org), that allows consumers to opt out of UID2 at any time. | 


### Participants 

With its transparent and interoperable approach, UID2 provides a collaborative framework for many participants across the advertising ecosystem—advertisers, publishers, DSPs, SSPs, single sign-on (SSO) providers, customer data platforms (CDPs), consent management providers (CMPs), identity providers, third-party data providers, and measurement providers.  

The following table lists the key participants and their roles in the UID2 [workflows](#workflows).

| Participant | Role Description |
| :--- | :--- |
| **Core Administrator**  | An organization (currently, The Trade Desk) that manages the UID2 Core Service and other [components](#components). For example, it distributes encryption keys and salts to UID2 operators and sends user opt-out requests to operators and DSPs. |  
| **Operators**  | Organizations that run the Operator Service (via the UID2 APIs). Operators receive and store encryption keys and salts from the UID2 Core Service, salt and hash personal data to return UID2 tokens, encrypt raw UID2s to generate UID2 tokens, and distribute UID2 token decryption keys.<br/><br/>Open operators run public instances of the Operator Service. For example, The Trade Desk currently serves as an open operator for the UID2 framework, available to all participants. If other open operators are available, a participant can choose which operator to work with.<br/><br/>Any participant can also choose to become a closed operator to generate and manage UID2s. | 
| **DSPs**  | DSPs integrate with the UID2 system to receive UID2s from advertisers (as first-party data) and third-party data providers (as third-party data) and leverage them to inform bidding on UID2s in the bid stream. | 
| **Data Providers**  | Organizations that collect user data and push it to DSPs—for example, advertisers, identity graph providers, and third-party data providers. | 
| **Advertisers**  | Organizations that buy impressions across a range of publisher sites and use DSPs to decide which ad impressions to purchase and how much to bid on them. | 
| **Publishers**  | Organizations that propagate UID2 tokens to the bid stream via SSPs—for example,  identity providers, publishers, and SSO providers. Publishers can choose to work with an SSO provider or an independent ID provider that is interoperable with UID2. The latter can handle the UID2 integration on behalf of publishers. | 
| **Consumers**  | Users who engage with publishers or their identity providers. Users can opt out of UID2 in the [Transparency and Control Portal](https://transparentadvertising.org). | 

## Workflows

The following table lists four key workflows in the UID2 framework with links to their high-level overviews. It also provides links to the respective integration guides, which include diagrams, integration steps, FAQs, and other relevant information for each workflow.

| Workflow | Intended Primary Participants |  Integration Guide |
| :--- | :--- | :--- |
| **Buy-Side**<br/>[Overview](./workflow-overview-buy-side.md) | DSPs who transact on UID2 tokens in the bid stream. |  [DSP](./api/v2/guides/dsp-guide.md) |
| **Data Provider**<br/>[Overview](./workflow-overview-3p-data-provider.md) | Organizations that collect user data and push it to DSPs. | [Advertiser and Third-Party Data Provider](./api/v2/guides/advertiser-dataprovider-guide.md) |
| **Supply-Side**<br/>[Overview](./workflow-overview-supply-side.md) | Organizations that propagate UID2 tokenss to the bid stream via SSPs.<br/> NOTE: Publishers can choose to leverage the [UID2 SDK](./api/v2/sdks/client-side-identity.md) or complete their own custom, server-only integration. | [Publisher (with UID2 SDK)](./api/v2/guides/publisher-client-side.md)<br/>[Publisher (Server-Only)](./api/v2/guides/custom-publisher-integration.md) |
| **Opt-Out**<br/>[Overview](./workflow-overview-opt-out.md) | Consumers who engage with publishers or their SSO providers and other identity providers. | N/A |


The following diagram summarizes all four workflows. For each workflow, the [participants](#participants), [components](#components), [UID2 identifier types](#uid2-identifier-types), and numbered steps are color-coded.

![The UID2 Ecosystem](/images/UID2-workflows.jpg)

## FAQs

Here are some commonly asked questions regarding the UID2 framework.

#### Will all integration partners in the EUID infrastructure (SSPs, third-party data providers, measurement providers) be automatically integrated with UID2? 

No. UID2 functions as its own framework, which is separate from EUID. As such, paperwork relating to the usage and access to the EUID framework does not automatically grant usage and access to the UID2 framework. New contracts are required to be signed for UID2.

#### Can users opt out of targeted advertising tied to their UID2 identity?

Yes. Through the [Transparency and Control Portal](https://transparentadvertising.org), users can opt out from being served targeted ads tied to their UID2 identity. Each request is distributed through the UID2 Opt-Out Service and UID2 Operators to all relevant participants. 

Some publishers and service providers have the option to limit access to their products based on a user’s participation in the UID2 framework, and it is the publisher’s responsibility to communicate this as part of their value exchange dialog with the user.

#### How does a user know where to access the opt-out portal?

Publishers, SSO providers, or consent management platforms disclose links to the [Transparency and Control Portal](https://transparentadvertising.org) in their login flows, consent flows, privacy policies, and by other means.

#### Why do advertisers and third-party data providers not need to integrate with the opt-out feed?

Opt-outs relate to opting out of targeted advertising, which is handled through the publisher and DSP opt-out [workflows](#workflows). To disengage from a specific advertiser, a consumer must contact the advertiser directly.


## License
All work and artifacts are licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.txt).
