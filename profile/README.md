Được. Vì hiện tại dự án **chưa bắt đầu implementation**, README phải thể hiện đúng trạng thái **Pre-alpha / Foundation Setup**, tuyệt đối không viết như các chức năng đã hoàn thành.

README hiện tại trong ảnh cần thay vì vẫn còn các nội dung sai thời điểm:

* “There is no official thesis topic yet”.
* “Brainstorming phase”.
* “Our Graduation Thesis Workspace”.
* Quy tắc đặt tên repository cũ.

Trong khi đề tài HomeSpace đã được xác định và chấp thuận chính thức. 

## Vị trí README của Organization

Trong repository `.github`, tạo cấu trúc:

```text
.github/
└── profile/
    ├── README.md
    └── assets/
        └── homespace-architecture.png
```

Đưa sơ đồ kiến trúc của bạn vào:

```text
profile/assets/homespace-architecture.png
```

Sau đó thay toàn bộ `profile/README.md` bằng nội dung dưới đây.

---

## README đề xuất

````md
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
- Mock data and testing strategies

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
````

A customer does not need separate landlord and tenant accounts.

---

## Data Ownership

Each microservice owns its business data and database schema.

Examples:

```text
User Service
└── user profiles and account metadata

Property Service
└── property listings and property-related data

Booking Service
└── viewing schedules and rental requests

Contract Service
└── rental contracts and contract parties

Payment Service
└── payment transactions

Chat Service
└── conversations and messages
```

Services must not access another service's database directly.

Cross-service identifiers such as `userId`, `propertyId`, and `contractId`
are logical references. They are not cross-database foreign keys.

---

## Mock Data Policy

Synthetic data may be used during development, demonstrations, integration
testing, and automated testing.

Mock data must follow these rules:

1. Do not use real citizen identification information.
2. Do not use real payment card or bank account information.
3. Do not commit real API keys, access tokens or private keys.
4. Use clearly synthetic names, emails, phone numbers and addresses.
5. Seed data must be repeatable and safe to recreate.
6. Development data must be separated from staging and production data.
7. Mock payment flows must use provider sandbox environments.
8. Blockchain testing must use a test network such as Sepolia.
9. Uploaded identity documents must be synthetic samples.
10. Database reset scripts must not run against production environments.

Example development accounts:

```text
customer.minh@homespace.local
customer.tuan@homespace.local
moderator@homespace.local
admin@homespace.local
```

Example mock entities:

```text
User ID:       00000000-0000-0000-0000-000000000101
Property ID:   00000000-0000-0000-0000-000000000201
Request ID:    00000000-0000-0000-0000-000000000301
Contract ID:   00000000-0000-0000-0000-000000000401
```

Mock datasets should be maintained through seed scripts instead of manual
database modifications.

---

## Engineering Principles

HomeSpace follows these engineering principles:

* Domain-oriented service boundaries
* Database ownership per service
* API-first development
* Secure-by-default authorization
* Event-driven integration where appropriate
* Idempotent event processing
* Automated testing
* Reproducible development environments
* Centralized observability
* No secrets committed to source control
* Explicit architecture decisions
* Backward-compatible API evolution

The use of a technology must be justified by a concrete system requirement.
Technologies must not be introduced only to increase architectural
complexity.

---

## Repository Naming Convention

All repositories use lowercase kebab-case.

### Deployable backend services

```text
homespace-<business-capability>-service
```

Examples:

```text
homespace-user-service
homespace-property-service
homespace-booking-service
homespace-contract-service
homespace-notification-service
```

### Applications and platform components

```text
homespace-customer-web
homespace-mobile-app
homespace-admin-portal
homespace-api-gateway
homespace-smart-contracts
homespace-infrastructure
homespace-observability
```

Repository names must describe a business capability or a clear platform
responsibility.

Names such as the following should not be used:

```text
base-service
core-service
common-service
backend-service
spring-service
```

---

## Repository Minimum Requirements

A repository is considered initialized only when it contains:

* A clear README
* Defined responsibility and scope
* Ownership information
* Project structure
* Local development instructions
* Environment variable example
* Build configuration
* Test configuration
* Dockerfile where applicable
* Continuous integration workflow
* License and security information where applicable

An empty repository is not considered an implemented microservice.

---

## Branching and Pull Requests

Recommended branches:

```text
main
develop
feature/<issue-number>-<short-description>
fix/<issue-number>-<short-description>
chore/<issue-number>-<short-description>
```

Example:

```text
feature/HS-102-create-property
fix/HS-148-prevent-self-rental-request
chore/HS-201-add-docker-healthcheck
```

Changes should be merged through pull requests.

Each pull request should include:

* Purpose of the change
* Related issue
* Technical approach
* Testing evidence
* API or database changes
* Screenshots for UI changes
* Known limitations

Direct pushes to protected branches should not be allowed.

---

## Definition of Done

A feature is considered complete when:

* Acceptance criteria are satisfied
* Code has been reviewed
* Automated tests pass
* Input validation is implemented
* Authorization is enforced by the backend
* Database migrations are included
* API documentation is updated
* Logs and errors are meaningful
* The feature runs in the documented environment
* No manual database modification is required for demonstration

---

## Initial Delivery Roadmap

### Phase 1 — Foundation

* Finalize service boundaries
* Establish repository conventions
* Configure development environments
* Define API and event standards
* Configure authentication and authorization
* Prepare deterministic mock datasets

### Phase 2 — Core Rental Workflow

* User profile management
* Property listing and moderation
* Property search
* Viewing schedules
* Rental requests
* Electronic contract lifecycle

### Phase 3 — Intelligent and Verifiable Contracts

* AI-assisted property discovery
* Contract document generation
* Contract hashing
* Smart contract integration
* Contract integrity verification

### Phase 4 — Platform Quality

* Integration tests
* Security testing
* Monitoring and tracing
* Performance evaluation
* Deployment automation
* Technical documentation

---

## Core Team

| Member          | Primary focus                                                                |
| --------------- | ---------------------------------------------------------------------------- |
| Nguyễn Văn Minh | Backend services, security, contract workflow, blockchain and infrastructure |
| Đào Quốc Tuấn   | Web, mobile, AI integration, user experience and application testing         |

All team members are expected to understand the complete business workflow
and platform architecture.

---

## Important Notice

HomeSpace is currently under active development.

The target architecture and planned capabilities may evolve based on
technical validation, implementation constraints, security reviews, and
product priorities.

This repository and its related services are not currently intended for
production use.

````

---

## Những gì được phép ghi ở thời điểm hiện tại

Bạn được phép dùng các cụm:

```text
Target Architecture
Planned Capability
Under Development
Pre-alpha
Foundation Setup
Initial Scaffold
Experimental
Mock Integration
Sandbox Environment
````

Không được dùng khi chưa có bằng chứng:

```text
Production Ready
Fully Implemented
Completed
Highly Scalable
Secure
High Performance
Fault Tolerant
AI model works accurately
Payment integration completed
Legally valid digital signature integrated
```

## Quy định dữ liệu giả tôi chốt

Bạn **nên tạo dữ liệu giả ngay từ giai đoạn thiết lập**, nhưng phải tạo có hệ thống:

```text
homespace-infrastructure/
└── seed/
    ├── users.json
    ├── properties.json
    ├── rental-requests.json
    ├── contracts.json
    └── README.md
```

Hoặc tốt hơn, mỗi service tự quản lý seed data của mình:

```text
homespace-property-service/
└── src/main/resources/db/dev-seed/

homespace-booking-service/
└── src/main/resources/db/dev-seed/

homespace-contract-service/
└── src/main/resources/db/dev-seed/
```

Nguyên tắc quan trọng:

> Infrastructure khởi tạo môi trường; service sở hữu dữ liệu và script seed thuộc domain của service đó.

Ngoài ra, bạn cần đổi tên các repository cũ trong ảnh trước khi công khai Organization:

```text
user-service
→ homespace-user-service

notification-service
→ homespace-notification-service

audit-service
→ homespace-audit-service

storage-service
→ homespace-media-service

gateway-server
→ homespace-api-gateway

infrastructure
→ homespace-infrastructure

observability
→ homespace-observability
```

`base-service` phải được xác định lại trách nhiệm hoặc đổi thành:

```text
homespace-spring-boot-starter
```

README trên thể hiện đúng hình ảnh doanh nghiệp nhưng vẫn trung thực rằng dự án đang ở giai đoạn thiết lập, chưa implementation.
