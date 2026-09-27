# LoanJuan Lending Management System

LoanJuan is a full-stack lending management platform designed to support the complete lifecycle of lending operations — from borrower management and loan application processing to repayment scheduling, payment collection, monitoring, and reporting.

The system combines a web-based administrative platform, a mobile application for field operations, a REST API, and automated background processing to provide a centralized solution for managing lending activities.

> **Portfolio Project:** This repository contains project documentation, screenshots, architecture diagrams, and selected technical information. The production source code and sensitive configuration are not publicly available.

---

## Overview

LoanJuan was developed to centralize and automate lending operations that involve borrowers, loan officers, collection officers, loan applications, repayments, payment processing, and administrative reporting.

The application supports role-based access and assignment workflows while maintaining relationships between companies, agents, borrowers, loans, payments, and repayment schedules.

The platform consists of several integrated components:

- Administrative web application
- Flutter mobile application
- ASP.NET Core REST API
- PostgreSQL database
- Automated loan scheduling and background processing
- Role and menu-based access control
- Reporting and monitoring features

---

## Technology Stack

### Backend

- C#
- ASP.NET Core
- .NET
- Entity Framework Core
- RESTful Web API
- JWT Authentication
- PostgreSQL

### Mobile Application

- Flutter
- Dart
- REST API integration
- Local SQLite storage
- Offline/online data synchronization

### Web Application

- ASP.NET Core
- Web-based administrative interface
- Responsive administrative screens
- REST API integration

### Database

- PostgreSQL
- Relational database design
- Stored functions
- Constraints and indexes
- Transactional processing
- Audit and historical records

### DevOps

- Git
- GitHub
- CI/CD pipelines
- Automated testing
- Environment-specific configuration
- UAT and Production deployment workflows

---

## System Architecture

LoanJuan follows a multi-layer application architecture.

```text
┌───────────────────────────────┐
│        Admin Web App          │
└───────────────┬───────────────┘
                │
                │ HTTPS / REST
                │
┌───────────────▼───────────────┐
│       ASP.NET Core API        │
│                               │
│ Authentication                │
│ Business Services             │
│ Authorization                 │
│ Loan Processing               │
│ Payment Processing            │
│ Reporting                     │
└───────────────┬───────────────┘
                │
                │ Entity Framework Core
                │
┌───────────────▼───────────────┐
│          PostgreSQL           │
│                               │
│ Borrowers                     │
│ Agents                        │
│ Loan Applications             │
│ Repayment Schedules           │
│ Payments                      │
│ Charges                       │
│ Audit / Historical Data       │
└───────────────────────────────┘


┌───────────────────────────────┐
│     Flutter Mobile App        │
│                               │
│ Field Operations              │
│ Borrower Management           │
│ Collection Activities         │
│ Local SQLite Storage          │
└───────────────┬───────────────┘
                │
                └──────── REST API ───────► ASP.NET Core API
```

---

## Core Features

### Borrower Management

LoanJuan maintains centralized borrower information and supports assigning borrowers to agents responsible for lending and collection activities.

Key capabilities include:

- Borrower registration
- Borrower information management
- Borrower status tracking
- Agent assignment
- Multiple borrower assignments
- Loan history
- Repayment monitoring

Borrower-to-agent relationships are managed through an assignment model, allowing the system to support flexible operational responsibilities.

---

### Agent, Position and Access Management

Agents represent users involved in lending and collection operations.

Each agent belongs to a company and is associated with a position that determines their organizational role.

The system supports:

- Agent profiles
- Position assignment
- Company membership
- Borrower assignments
- Approval privileges
- Administrative privileges
- Role-based functionality

<p align="center">
  <img width="300"
       alt="LoanJuan Agent, Position and Access Management"
       src="https://github.com/user-attachments/assets/7b90ac00-d593-4dc2-8a3c-6f25acb0f79a" />
</p>


---

### Loan Application Management

The loan application module manages the lifecycle of loans associated with borrowers and agents.

Loan information includes:

- Borrower
- Assigned agent
- Loan amount
- Loan term
- Loan status
- Charges
- Repayment schedule
- Payment history

Loan applications provide the central relationship connecting borrowers, agents, repayment schedules, charges, and payments.

---

### Loan Approval Workflow

LoanJuan supports controlled loan review and approval workflows.

Approval operations are restricted to authorized users, allowing the application to separate loan processing responsibilities from approval authority.

The system supports:

- Loan review
- Loan approval
- Loan rejection
- Review comments
- Rejection reasons
- Bulk approval operations
- Approver-based authorization

---

### Repayment Scheduling

Repayment schedules are generated for approved loans and track the financial obligations associated with each repayment period.

Schedule information includes:

- Due date
- Principal due
- Interest amount
- Amount paid
- Payment status

The system uses scheduled processing to maintain loan repayment information and support ongoing loan monitoring.

---

### Payment Processing

LoanJuan records borrower payments and applies them against loan obligations.

The payment architecture includes:

- Loan payments
- Payment allocations
- Payment applications
- Repayment schedule updates
- Payment tracking
- Remaining balances

Payments are associated with loan applications, allowing payment ownership and borrower information to be derived from the loan relationship.

---

### Payment Correction and Audit History

LoanJuan includes a controlled payment-voiding workflow for correcting payment transactions while preserving an audit trail.

When a payment is voided:

1. The original payment is identified.
2. Relevant loan information is retrieved.
3. Authorization and validation are performed.
4. A historical copy of the payment is stored.
5. Related payment calculations are reversed.
6. The original payment record is removed.
7. Loan schedules and balances are recalculated.

Voided payments are preserved separately as historical audit records, allowing payment corrections without losing transaction history.

---

### Loan Charges

Charges can be configured and associated with loan applications.

The charge architecture separates reusable charge definitions from charges actually applied to individual loans.

This provides flexibility for managing different lending fees while retaining the exact charges associated with each loan.

---

### Collections and Loan Monitoring

LoanJuan provides operational tools for monitoring repayment activity and identifying loans requiring attention.

Capabilities include:

- Repayment monitoring
- Overdue loan tracking
- Missed-due monitoring
- Collection activities
- Borrower assignments
- Loan status tracking

These features help lending and collection personnel identify accounts requiring follow-up.

---

## Reporting

LoanJuan provides administrative and operational reports for monitoring lending activities.

Examples include:

- Overdue Loans
- Repayment Overview
- Cash Release
- Loan Client List
- Missed Dues
- Payment List

Reporting data is derived from the lending, borrower, repayment, and payment modules.

---

## Mobile Application

LoanJuan includes a Flutter mobile application designed for field personnel.

The mobile application communicates with the central LoanJuan API while also supporting local data storage using SQLite.

Key mobile capabilities include:

- Secure authentication
- Dashboard
- Borrower access
- Loan information
- Collection-related activities
- Role-aware functionality
- Offline/local data support
- API synchronization

Environment indicators distinguish application builds connected to different deployment environments.

---

## Authentication and Authorization

LoanJuan uses JWT-based authentication.

Authentication claims provide application context such as:

- Company
- Agent
- Position
- Role

Authorization rules are enforced by the backend API rather than relying solely on user-interface restrictions.

The system also supports:

- Role-based access
- Menu-based access
- Approval permissions
- Administrative permissions
- Forced password reset
- Protected API endpoints

---

## Access Control

Application navigation can be controlled through configurable menu relationships.

The access-control model includes concepts such as:

```text
User
 │
 ├── UserMenu
 │
 ▼
Menu

Position
 │
 ├── PositionMenu
 │
 ▼
Menu
```

This allows application functionality and navigation options to be associated with users and organizational positions.

---

## Entity Relationship Diagram

The LoanJuan database is structured around the complete lending lifecycle, including companies, agents, borrowers, loan applications, repayment schedules, payments, charges, allocations, and payment applications.

The ERD below illustrates the primary entities and relationships used by the lending platform.

<img width="2747" height="1754" alt="LoanJuan Lending Entity Relationship Diagram (4)" src="https://github.com/user-attachments/assets/196e79f0-ef08-45f8-b5fd-b3447999a61f" />




The model minimizes unnecessary duplicated ownership information by deriving relationships through authoritative entities where appropriate.

Historical and audit records may intentionally retain contextual information independently from active transactional records.

---

## Database Design

The database was designed with attention to:

- Referential relationships
- Appropriate indexes
- Monetary precision
- Controlled string lengths
- Nullable versus required relationships
- Historical audit data
- Transaction consistency
- Reporting performance

Financial transaction values use standardized precision appropriate for monetary data.

Database functions support reporting and specialized data retrieval where appropriate.

---

## Background Processing

LoanJuan includes automated processing for loan-related activities.

The scheduling architecture supports:

- Loan scheduler processing
- Repayment schedule maintenance
- Loan status updates
- Delinquency processing
- Scheduled job execution
- Scheduler run logging

Background processing is separated from interactive application operations so scheduled financial processing can run independently.

---

## Data Integrity

Several design principles are used throughout LoanJuan:

### Single Source of Truth

Relationships are derived from their authoritative source where possible.

For example:

```text
Company
   ↓
Agent
   ↓
BorrowerAssignment
   ↓
Borrower
```

Agent positions are maintained on the Agent rather than duplicated in borrower assignments.

### Transactional Consistency

Financial operations that affect multiple records are processed as coordinated transactions to reduce the risk of partial updates.

### Auditability

Historical information is retained where financial operations require traceability, including payment correction and voiding operations.

---

## Testing

The project includes automated testing across backend and mobile components.

Testing covers areas such as:

- Business logic
- API behavior
- Authorization
- Loan processing
- Payment processing
- Database contracts
- Data type and field-length standards
- Mobile services
- Mobile screens
- Environment configuration

Regression testing is performed when changes affect shared lending or financial workflows.

---

## Deployment Environments

LoanJuan uses separate environments for application development and release validation.

```text
Development
     ↓
    UAT
     ↓
Production
```

Database changes are validated in UAT before production promotion.

Deployment processes include:

- Pre-deployment validation
- Application deployment
- Database changes
- Post-deployment verification
- Automated testing
- Environment-specific configuration

---

## CI/CD

The project uses automated build and deployment processes to improve release consistency.

The pipeline supports activities such as:

- Source validation
- Automated tests
- Application builds
- Environment configuration
- Mobile APK generation
- UAT deployment
- Production deployment preparation

This reduces reliance on manual deployment steps and improves repeatability between environments.

---

## Engineering Practices

LoanJuan was developed with emphasis on:

- Layered application architecture
- Separation of concerns
- RESTful API design
- Relational data modeling
- Role-based authorization
- Transactional financial processing
- Auditability
- Automated testing
- CI/CD
- Environment separation
- Schema validation
- Maintainability

---

## Project Documentation

Additional technical material will be added to this repository as the portfolio documentation evolves.

Planned documentation includes:

- Entity Relationship Diagram (ERD)
- System Architecture Diagram
- Loan Processing Workflow
- Payment Processing Workflow
- Screenshots
- Mobile Application Screens
- Database Design Overview

---

## Screenshots

Application screenshots will be available in the `screenshots` directory.

> All portfolio screenshots use demonstration data. Production customer and borrower information is not included.

---

## Repository Notice

This repository is intended to demonstrate the architecture, functionality, and engineering practices used in the LoanJuan Lending Management System.

The complete production source code, database credentials, API secrets, deployment credentials, customer information, and other confidential configuration are intentionally excluded.

---

## Author

**Rey Concellado**  
Senior Programmer Analyst / Software Developer

Primary areas demonstrated by this project:

- C# / ASP.NET Core
- Flutter / Dart
- Entity Framework Core
- PostgreSQL
- REST API Development
- Relational Database Design
- Mobile Application Development
- Authentication and Authorization
- Financial Transaction Processing
- Automated Testing
- CI/CD
