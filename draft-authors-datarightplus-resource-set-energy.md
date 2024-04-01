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

This is the resource set profile outlining the energy sector related endpoints. In addition to outlining Initiator and Provider provisions it also specifies requirements for the Energy Authority (electricity assets and usage) and Energy Plan Website (retail electricity plan information).

.# Notational Conventions

The keywords "**REQUIRED**", "**SHALL**", "**SHALL NOT**", "**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**",  "**MAY**", and "**OPTIONAL**" in this document are to be interpreted as described in [@!RFC2119].

{mainmatter}

# Scope

The scope of this document is intended to be limited to the resource server endpoints related to energy, their associated authorisation contexts, the services provided by the Energy Authority and Energy Plan Website.

# Terminology

This specification utilises the various terms outlined within [@!DATARIGHTPLUS-ROSETTA].

# Providers

Providers which manage energy sector information are expected to deliver a number of resource server end points.

## Authorisation Server

In addition to other provisions incorporated within the relevant ecosystem set, the Provider authorisation server **SHALL**:

1. Support the [@!RFC6749] `scope` parameter with possible values outlined within [Authorisation Scopes](#name-authorisation-scopes);

### Authorisation Scopes

The Provider authorisation server **SHALL** utilise the following Data Set Language when seeking Consumer authorisation from a User for specific `scope` values:

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

#### Overlapping Scope Optimisation

Alternative Data Cluster Language **SHALL** be used when pairs of `scope` value are used as follows:

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

## Resource Server

The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `energy:accounts.basic:read` scope value:

| Resource Server Endpoint                                       | Valid `x-v` |
|----------------------------------------------------------------|-------------|
| `GET /energy/accounts`                                         | `2`         |

The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `energy:accounts.detail:read` scope value:

| Resource Server Endpoint                                       | Valid `x-v` |
|----------------------------------------------------------------|-------------|
| `GET /energy/accounts/{accountId}`                             | `2`, `3`    |

The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `energy:accounts.concessions:read` scope value:

| Resource Server Endpoint                                       | Valid `x-v` |
|----------------------------------------------------------------|-------------|
| `GET /energy/accounts/{accountId}/concessions`                 | `1`         |

The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `energy:accounts.paymentschedule:read` scope value:

| Resource Server Endpoint                                       | Valid `x-v` |
|----------------------------------------------------------------|-------------|
| `GET /energy/accounts/{accountId}/payment-schedule`            | `1`         |

The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `energy:billing:read` scope value:

| Resource Server Endpoint                                       | Valid `x-v` |
|----------------------------------------------------------------|-------------|
| `GET /energy/accounts/balances`                                | `1`         |
| `GET /energy/accounts/{accountId}/balance`                     | `1`         |
| `POST /banking/accounts/balances`                              | `1`         |
| `GET /energy/accounts/{accountId}/invoices`                    | `1`         |
| `GET /energy/accounts/invoices`                                | `1`         |
| `POST /energy/accounts/invoices`                               | `1`         |
| `GET /energy/accounts/{accountId}/billing`                     | `1`         |
| `GET /energy/accounts/billing`                                 | `1`         |
| `POST /energy/accounts/billing`                                | `1`         |


The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `energy:electricity.usage:read` scope value:

| Resource Server Endpoint                                       | Valid `x-v` |
|----------------------------------------------------------------|-------------|
| `GET /energy/electricity/servicepoints/{servicePointId}/usage` | `1`         |
| `GET /energy/electricity/servicepoints/usage`                  | `1`         |
| `POST /energy/electricity/servicepoints/usage`                 | `1`         |

The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `energy:electricity.servicepoints.basic:read` scope value:

| Resource Server Endpoint                                       | Valid `x-v` |
|----------------------------------------------------------------|-------------|
| `GET /energy/electricity/servicepoints`                        | `1`         |


The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `energy:electricity.servicepoints.detail:read` scope value:

| Resource Server Endpoint                                       | Valid `x-v` |
|----------------------------------------------------------------|-------------|
| `GET /energy/electricity/servicepoints/{servicePointId}`       | `1`         |

The Provider **SHALL** make available, as described further in [@!DATARIGHTPLUS-REDOCLY-ID1] endpoints, the following endpoints where the token is granted the `energy:electricity.der.basic:read` scope value:

| Resource Server Endpoint                                       | Valid `x-v` |
|----------------------------------------------------------------|-------------|
| `GET /energy/electricity/servicepoints/der`                    | `1`         |
| `POST /energy/electricity/servicepoints/der`                   | `1`         |

In addition, the Provider **MAY** deliver the following unauthenticated and generally available endpoints, in accordance with [@!DATARIGHTPLUS-REDOCLY-ID1]:

| Resource Server Endpoint     | `x-v` |
|------------------------------|-------|
| `GET /energy/plans`          | `1`   |
| `GET /energy/plans/{planId}` | `1`   |

### Electricity Authority Resource Bridge

In order to deliver information requested by the Initiator the Provider **SHALL** provide a back-to-back relay of resource server requests between the Provider and Electricity Authority as follows:

| Provider Resource Server Endpoint                              | Electricity Authority Resource Server Endpoint                           |
|----------------------------------------------------------------|--------------------------------------------------------------------------|
| `GET /energy/electricity/servicepoints/{servicePointId}/usage` | `GET /secondary/energy/electricity/servicepoints/{servicePointId}/usage` |
| `GET /energy/electricity/servicepoints/usage`                  | `GET /secondary/energy/electricity/servicepoints/usage`                  |
| `POST /energy/electricity/servicepoints/usage`                 | `POST /secondary/energy/electricity/servicepoints/usage`                 |
| `GET /energy/electricity/servicepoints`                        | `GET /secondary/energy/electricity/servicepoints`                        |
| `GET /energy/electricity/servicepoints/{servicePointId}`       | `GET /secondary/energy/electricity/servicepoints/{servicePointId}`       |
| `GET /energy/electricity/servicepoints/der`                    | `GET /secondary/energy/electricity/servicepoints/der`                    |
| `POST /energy/electricity/servicepoints/der`                   | `POST /secondary/energy/electricity/servicepoints/der`                   |

_Note:_ Refer to the Provider [Resource Server](#resource-server) and Electricity Authority [Resource Server](#resource-server-1) sections for the appropriate API mappings.

# Initiators

Initiators **SHALL** describe the requested `scope` values using the same Data Set Language as Providers, as outlined in [Authorisation Scopes](#name-authorisation-scopes).

# Electricity Authority

The Electricity Authority **SHALL** deliver the electricity asset and usage information to the Provider.

## Authorisation Server

The Electricity Authority **SHALL** authorise Providers using existing information security protocols. The specific details of this are outside the scope of this document.

## Resource Server

The Electricity Authority **SHALL** make the following restricted endpoints available to Providers, using existing authentication and authorisation channels, and in accordance with [@!DATARIGHTPLUS-REDOCLY-ID1]:

| Resource Server Endpoint                                                     | Valid `x-v` |
|------------------------------------------------------------------------------|-------------|
| `GET /secondary/energy/electricity/servicepoints/{nationalMeteringId}/usage` | `1`         |
| `POST /secondary/energy/electricity/servicepoints/usage`                     | `1`         |
| `POST /secondary/energy/electricity/servicepoints`                           | `1`         |
| `GET /secondary/energy/electricity/servicepoints/{nationalMeteringId}`       | `1`         |
| `GET /secondary/energy/electricity/servicepoints/{nationalMeteringId}/der`   | `1`         |
| `POST /secondary/energy/electricity/servicepoints`                           | `1`         |

# Electricity Plan Website

The Electricity Plan Website **SHALL** deliver the following unauthenticated and generally available endpoints, in accordance with [@!DATARIGHTPLUS-REDOCLY-ID1]:

| Resource Server Endpoint     | Valid `x-v` |
|------------------------------|-------------|
| `GET /energy/plans`          | `1`         |
| `GET /energy/plans/{planId}` | `1`         |

# Acknowledgement

The following people contributed to this document:

- Stuart Low (Biza.io) - Editor

We acknowledge the contribution to the [@!CDS] of the following individuals:

- James Bligh (Data Standards Body) - Lead Architect for the Consumer Data Right
- Mark Verstege (Data Standards Body) - Lead Architect, Banking & Information Security for the Consumer Data Right
- Ivan Hosgood (formerly Data Standards Body & ACCC) - Solutions Architect

{backmatter}

<reference anchor="DATARIGHTPLUS-REDOCLY-ID1" target="https://datarightplus.github.io/datarightplus-redocly/?v=ID1"> <front><title>DataRight+: Redocly (ID1)</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author><author initials="B." surname="Kolera" fullname="Ben Kolera"><organization>Biza.io</organization></author>
<author initials="W." surname="Cai" fullname="Wei Cai"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="DATARIGHTPLUS-ROSETTA" target="https://datarightplus.github.io/datarightplus-specs/main/datarightplus-rosetta.html"> <front><title>DataRight+ Rosetta Stone</title><author initials="S." surname="Low" fullname="Stuart Low"><organization>Biza.io</organization></author></front> </reference>

<reference anchor="CDS" target="https://consumerdatastandardsaustralia.github.io/standards"><front><title>Consumer Data Standards (CDS)</title><author><organization>Data Standards Body (Treasury)</organization></author></front> </reference>

<reference anchor="RFC6749" target="https://datatracker.ietf.org/doc/html/rfc6749"> <front> <title>The OAuth 2.0 Authorization Framework</title><author fullname="D. Hardt"> <organization>Microsoft</organization> </author><date month="Oct" year="2012"/></front> </reference>







