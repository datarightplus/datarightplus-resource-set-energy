%%%
title = "DataRight+: Energy Resource Set"
area = "Internet"
workgroup = "datarightplus"
submissionType = "independent"

[seriesInfo]
name = "Internet-Draft"
value = "draft-authors-datarightplus-resource-set-energy-latest"
stream = "independent"
status = "experimental"

date = 2024-03-28T00:00:00Z

[[author]]
initials="S."
surname="Low"
fullname="Stuart Low"
organization="Biza.io"
[author.address]
email = "stuart@biza.io"

%%%

.# Abstract

This is the resource set profile outlining the energy sector related endpoints. In addition to outlining Initiator and Provider provisions it also specifies requirements for the Energy Authority (responsible for electricity assets and usage) and Energy Plan Website (responsible for retail electricity plan information).

.# Notational Conventions

The keywords "**REQUIRED**", "**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**",  "**MAY**", and "**OPTIONAL**" in this document are to be interpreted as described in [@!RFC2119].

{mainmatter}

# Scope

The scope of this document is intended to be limited to the resource server endpoints related to energy, and their associated authorisation contexts.

# Terminology

This specification utilises the various terms outlined within [@!DATARIGHTPLUS-ROSETTA].

# Authorisation Scopes

Technical authorisation scopes are defined between Provider and Initiator and permit access to resource server endpoints. In addition, this specification also outlines the title and a simple description of the language to use to describe the data set referred to as Data Set Language.

Providers and Initiators **SHALL** utilise the prescribed authorisation scopes and Data Set Language when seeking Consumer authorisation:

| `scope` value                                  | Data Set Language                          |
|------------------------------------------------|--------------------------------------------|
| `energy:accounts.basic:read`                   | **Accounts and plans**                     |
|                                                | Account and plan information;              |
| `energy:accounts.detail:read`                  | **Account and plan details**               |
|                                                | Account type;                              |
|                                                | Fees, features, rates, and discounts;      |
|                                                | Additional account users;                  |
| `energy:accounts.concessions:read`             | **Concessions and assistance**             |
|                                                | Concession type;                           |
|                                                | Concession information;                    |
| `energy:accounts.paymentschedule:read`         | **Payment preferences**                    |
|                                                | Payment and billing frequency;             |
|                                                | Any scheduled payment details;             |
| `energy:billing:read`                          | **Billing payments and history**           |
|                                                | Account balance;                           |
|                                                | Payment method;                            |
|                                                | Payment status;                            |
|                                                | Charges, discounts, credits;               |
|                                                | Billing date;                              |
|                                                | Usage for billing period;                  |
|                                                | Payment date;                              |
|                                                | Invoice number;                            |
| `energy:electricity.servicepoints.basic:read`  | **Electricity connection**                 |
|                                                | National Meter Identifier (NMI);           |
|                                                | Customer type;                             |
|                                                | Connection point details;                  |
| `energy:electricity.servicepoints.detail:read` | **Electricity meter**                      |
|                                                | Supply address;                            |
|                                                | Meter details;                             |
|                                                | Associated service providers;              |
| `energy:electricity.der:read`                  | **Energy generation and storage**          |
|                                                | Generation information;                    |
|                                                | Generation or storage device type;         |
|                                                | Device characteristics;                    |
|                                                | Devices that can operate without the grid; |
|                                                | Energy conversion information;             |
| `energy:electricity.usage:read`                | **Electricity usage**                      |
|                                                | Usage;                                     |
|                                                | Meter details;                             |

## Overlapping Scope Optimisation

In certain situations multiple technical scopes overlap which can lead to confusion by the User granting permission for the Consumer (which may be themselves or an Entity they represent).

Data Cluster Language presentation **SHALL** be collapsed for the following pairs of `scope` values:

| `scope` pairing                                   | Data Set Language                     |
|---------------------------------------------------|---------------------------------------|
| `energy:accounts.basic:read` and                  | **Account and plan details**          |
| `energy:accounts.detail:read`                     | Account and plan information;         |
|                                                   | Account type;                         |
|                                                   | Fees, features, rates, and discounts; |
|                                                   | Additional account users;             |
| `energy:electricity.servicepoints.basic:read` and | **Electricity connection and meter**  |
| `energy:electricity.servicepoints.detail:read`    | National Meter Identifier (NMI);      |
|                                                   | Supply address;                       |
|                                                   | Customer type;                        |
|                                                   | Connection point details;             |
|                                                   | Meter details;                        |
|                                                   | Associated service providers;         |

# Providers

Providers are **REQUIRED** to deliver a number of authorisation and resource server capabilities.

## Authorisation Server

In addition to other provisions incorporated within the relevent ecosystem set, the Provider authorisation server **SHALL**:

1. Support the `scope` parameters outlined within [Authorisation Scopes];

## Resource Server

The Provider **SHALL** deliver the following authorisation enabled endpoints, in accordance with [@!DATARIGHTPLUS-REDOCLY]:

| Resource Server Endpoint                                       | Authorisation Scope                            | OpenAPI Operation ID                       | `x-v`       |
|----------------------------------------------------------------|------------------------------------------------|--------------------------------------------|-------------|
| `GET /energy/accounts`                                         | `energy:accounts.basic:read`                   | `listEnergyAccounts`                       | `2`         |
| `GET /energy/accounts/{accountId}`                             | `energy:accounts.detail:read`                  | `getEnergyAccountDetail`                   | `2` and `3` |
| `GET /energy/accounts/{accountId}/concessions`                 | `energy:accounts.concessions:read`             | `getEnergyAccountConcessions`              | `1`         |
| `GET /energy/accounts/balances`                                | `energy:billing:read`                          | `getEnergyAccountBalances`                 | `1`         |
| `GET /energy/accounts/{accountId}/balance`                     | `energy:billing:read`                          | `getEnergyAccountBalance`                  | `1`         |
| `POST /banking/accounts/balances`                              | `energy:billing:read`                          | `listBalancesSpecificEnergyAccounts`       | `1`         |
| `GET /energy/accounts/{accountId}/payment-schedule`            | `energy:accounts.paymentschedule:read`         | `getEnergyAccountPaymentSchedule`          | `1`         |
| `GET /energy/accounts/{accountId}/invoices`                    | `energy:billing:read`                          | `listEnergyAccountInvoices`                | `1`         |
| `GET /energy/accounts/invoices`                                | `energy:billing:read`                          | `listEnergyInvoices`                       | `1`         |
| `POST /energy/accounts/invoices`                               | `energy:billing:read`                          | `listEnergyInvoicesSpecificAccounts`       | `1`         |
| `GET /energy/accounts/{accountId}/billing`                     | `energy:billing:read`                          | `getEnergyAccountBilling`                  | `1`         |
| `GET /energy/accounts/billing`                                 | `energy:billing:read`                          | `getEnergyAccountBillingBulk`              | `1`         |
| `POST /energy/accounts/billing`                                | `energy:billing:read`                          | `listBillingSpecificEnergyAccounts`        | `1`         |
| `GET /energy/electricity/servicepoints/{servicePointId}/usage` | `energy:electricity.usage:read`                | `getElectricityServicePointUsage`          | `1`         |
| `GET /energy/electricity/servicepoints/usage`                  | `energy:electricity.usage:read`                | `getElectricityServicePointUsageBulk`      | `1`         |
| `POST /energy/electricity/servicepoints/usage`                 | `energy:electricity.usage:read`                | `getElectricityUsageSpecificServicePoints` | `1`         |
| `GET /energy/electricity/servicepoints`                        | `energy:electricity.servicepoints.basic:read`  | `listElectricityServicePoints`             | `1`         |
| `GET /energy/electricity/servicepoints/{servicePointId}`       | `energy:electricity.servicepoints.detail:read` | `getElectricityServicePointDetail`         | `1`         |
| `GET /energy/electricity/servicepoints/der`                    | `energy:electricity.der.basic:read`            | `getElectricityServicePointDERBulk`        | `1`         |
| `POST /energy/electricity/servicepoints/der`                   | `energy:electricity.der.basic:read`            | `getElectricityDERSpecificServicePoints`   | `1`         |

In addition, the Provider **MAY** deliver the following unauthenticated and generally available endpoints, in accordance with [@!DATARIGHTPLUS-REDOCLY]:

| Resource Server Endpoint     | OpenAPI Operation ID  | `x-v` |
|------------------------------|-----------------------|-------|
| `GET /energy/plans`          | `listEnergyPlans`     | `1`   |
| `GET /energy/plans/{planId}` | `getEnergyPlanDetail` | `1`   |

# Initiators

This specification contains no additional provisions for Initiators.

# Electricity Authority

The Electricity Authority **SHALL** deliver the electricity asset and usage information to the Provider.

## Authorisation Server

The Electricity Authority **SHALL** authorise Providers using existing information security protocols. The specific details of this are outside the scope of this document.

## Resource Server

The Electricity Authority **SHALL** make the following restricted endpoints available to Providers, using existing authentication and authorisation channels, and in accordance with [@!DATARIGHTPLUS-REDOCLY]:

| Resource Server Endpoint                                                     | OpenAPI Operation ID                               | `x-v` |
|------------------------------------------------------------------------------|----------------------------------------------------|-------|
| `GET /secondary/energy/electricity/servicepoints/{nationalMeteringId}/usage` | `getElectricalAuthorityServicePointUsage`          | `1`   |
| `POST /secondary/energy/electricity/servicepoints/usage`                     | `getElectricalAuthorityUsageSpecificServicePoints` | `1`   |
| `POST /secondary/energy/electricity/servicepoints`                           | `getElectricalAuthorityServicePoints`              | `1`   |
| `GET /secondary/energy/electricity/servicepoints/{nationalMeteringId}`       | `getElectricalAuthorityServicePointDetail`         | `1`   |
| `GET /secondary/energy/electricity/servicepoints/{nationalMeteringId}/der`   | `getElectricalAuthorityServicePointDER`            | `1`   |
| `POST /secondary/energy/electricity/servicepoints`                           | `getElectricalAuthorityDERSpecificServicePoints`   | `1`   |

# Electricity Plan Website

The Electricity Plan Website **SHALL** deliver the following unauthenticated and generally available endpoints, in accordance with [@!DATARIGHTPLUS-REDOCLY]:

| Resource Server Endpoint     | OpenAPI Operation ID  | `x-v` |
|------------------------------|-----------------------|-------|
| `GET /energy/plans`          | `listEnergyPlans`     | `1`   |
| `GET /energy/plans/{planId}` | `getEnergyPlanDetail` | `1`   |

# Acknowledgement

The following people contributed to this document:

- Stuart Low (Biza.io) - Editor

We acknowledge the contribution to the [@!CDS] of the following individuals:
- James Bligh (Data Standards Body) - Lead Architect for the Consumer Data Right
- Mark Verstege (Data Standards Body) - Lead Architect, Banking & Information Security for the Consumer Data Right
- Ivan Hosgood (formerly Data Standards Body & ACCC) - Solutions Architect

{backmatter}

<reference anchor="DATARIGHTPLUS-REDOCLY" target="https://datarightplus.github.io/datarightplus-redocly/"> <front><title>DataRight+: Redocly</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author><author initials="B." surname="Kolera" fullname="Ben Kolera"><organization>Biza.io</organization></author>
<author initials="W." surname="Cai" fullname="Wei Cai"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-ROSETTA" target="https://datarightplus.github.io/datarightplus-specs/main/datarightplus-rosetta.html"> <front><title>DataRight+ Rosetta Stone</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="CDS" target="https://consumerdatastandardsaustralia.github.io/standards"><front><title>Consumer Data Standards (CDS)</title><author><organization>Data Standards Body (Treasury)</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-INFOSEC-SHARING-V1" target="https://datarightplus.github.io/datarightplus-specs/main/datarightplus-infosec-sharing-v1.html"> <front><title>CDR: Sharing Arrangement V1</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="OIDC-Core" target="http://openid.net/specs/openid-connect-core-1_0.html"> <front> <title>OpenID Connect Core 1.0 incorporating errata set 1</title> <author initials="N." surname="Sakimura" fullname="Nat Sakimura"></author></front></reference>

<reference anchor="TDIF" target="https://www.digitalidentity.gov.au"><front><title>Trusted Digital Identity Framework (
TDIF)</title><author><organization>Commonwealth of
Australia (Digital Transformation Agency)</organization></author></front> </reference>






