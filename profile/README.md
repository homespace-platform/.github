<div align="center">

# HomeSpace Platform

**An intelligent cross-platform housing rental and digital contract platform.**

HomeSpace connects tenants and property providers through Web and Mobile
applications while supporting AI-assisted property discovery, electronic
rental contracts, and blockchain-based contract integrity verification.

![Project Status](https://img.shields.io/badge/status-pre--alpha-orange)
![Architecture](https://img.shields.io/badge/architecture-microservices-blue)
![Web](https://img.shields.io/badge/client-web-informational)
![Mobile](https://img.shields.io/badge/client-cross--platform-informational)

</div>

---

## Project Status

HomeSpace is currently in the **foundation and system design phase**.

The team is establishing:

- Product scope and business requirements
- Domain boundaries and microservice responsibilities
- Repository conventions
- Development environments
- Infrastructure configurations
- Authentication and authorization models
- API and event contracts

> The repositories may currently contain only initial scaffolding,
> configuration files, documentation, or experimental implementations.
> Features described in this organization represent the target architecture
> and are not necessarily completed.

| Area | Current status |
|---|---|
| Product requirements | In progress |
| System architecture | In progress |
| Repository initialization | In progress |
| Local infrastructure | Planned |
| Backend implementation | Not started |
| Web implementation | Not started |
| Mobile implementation | Not started |
| AI integration | Planned |
| Blockchain integration | Planned |
| Production deployment | Not available |

---

## Product Vision

HomeSpace aims to provide a unified rental platform where users can:

- Discover and compare rental properties
- Publish and manage property listings
- Schedule property viewings
- Submit and manage rental requests
- Communicate with other transaction participants
- Create and manage electronic rental contracts
- Verify contract integrity through blockchain records
- Receive AI-assisted property recommendations
- Make rental-related payments through supported payment providers

A single customer account may participate in different business contexts.

For example, the same user may:

- Publish a property as a landlord
- Rent another property as a tenant
- Act as the lessor in one contract
- Act as the lessee in another contract

`LANDLORD`, `TENANT`, `LESSOR`, and `LESSEE` are contextual business roles,
not permanent account types.

---

## Platform Applications

| Application | Repository | Description |
|---|---|---|
| Customer Web | `homespace-customer-web` | Web application for tenants and property providers |
| Mobile Application | `homespace-mobile-app` | Cross-platform mobile application |
| Admin Portal | `homespace-admin-portal` | Internal administration and moderation portal |

The Web and Mobile applications share the same business platform through
the HomeSpace API Gateway.

---

## Target Architecture

![HomeSpace Microservices Architecture](./assets/homespace-architecture.png)

HomeSpace follows a domain-oriented microservices architecture.

Client applications communicate with backend services through the API
Gateway. Services use synchronous REST APIs for request-response operations
and asynchronous domain events for workflows that do not require an
immediate response.

### Backend services

| Service | Repository | Primary responsibility |
|---|---|---|
| API Gateway | `homespace-api-gateway` | Routing, security enforcement, rate limiting and resilience |
| User Service | `homespace-user-service` | User profiles, account status, authorization metadata and eKYC status |
| Property Service | `homespace-property-service` | Property listings, categories, facilities, media references and search |
| Booking Service | `homespace-booking-service` | Viewing schedules, rental requests and booking lifecycle |
| Chat Service | `homespace-chat-service` | Real-time conversations, messages and WebSocket communication |
| Contract Service | `homespace-contract-service` | Electronic contract lifecycle, signatures and contract verification |
| Payment Service | `homespace-payment-service` | Deposits, rental payments, transactions and payment-provider integration |
| Notification Service | `homespace-notification-service` | In-app, email and push notifications |
| AI Service | `homespace-ai-service` | AI assistant, semantic retrieval, image analysis and price estimation |
| Audit Service | `homespace-audit-service` | Audit trails and records of important system activities |

### Platform repositories

| Repository | Responsibility |
|---|---|
| `homespace-smart-contracts` | Solidity smart contracts, deployment scripts and contract tests |
| `homespace-infrastructure` | Docker Compose, databases, Kafka, Redis, Keycloak and supporting services |
| `homespace-observability` | Prometheus, Grafana, dashboards and alert configurations |
| `homespace-engineering-docs` | Architecture, ADRs, API conventions, event definitions and runbooks |
| `homespace-spring-boot-starter` | Shared technical configuration for Java services |
| `.github` | Organization profile, templates and shared GitHub configuration |

---

## Identity and Authorization Model

HomeSpace separates system-level authorization from transaction-level
business roles.

### System roles

Examples of system-level roles include:

- `CUSTOMER`
- `ADMIN`
- `MODERATOR`
- `SUPPORT`

System roles control access to platform-level capabilities.

### Contextual roles

Business roles are inferred from the related resource:

```text
Property landlord:
property.landlordId == currentUserId

Rental request tenant:
rentalRequest.tenantId == currentUserId

Rental request landlord:
rentalRequest.landlordId == currentUserId

Contract lessor:
contract.lessorId == currentUserId

Contract lessee:
contract.lesseeId == currentUserId
