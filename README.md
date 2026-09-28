# LoanJuan Lending Management System

LoanJuan is a full-stack lending management platform designed to support the complete lifecycle of lending operations, from borrower management and loan application processing to repayment scheduling, payment collection, monitoring, and reporting.

The system combines a Flutter mobile application, a REST API, a centralized database, and automated background processing to provide a complete solution for managing lending activities.

> **Portfolio Project:** This repository contains project documentation, screenshots, architecture diagrams, and selected technical information. The production source code and sensitive configuration are not publicly available.

---

## Overview

LoanJuan was developed to centralize and automate lending operations that involve borrowers, loan officers, collection officers, loan applications, repayments, payment processing, and administrative reporting.

The application supports role-based access and assignment workflows while maintaining relationships between companies, agents, borrowers, loans, payments, and repayment schedules.

The platform consists of several integrated components:

- Flutter mobile application
- ASP.NET Core REST API
- PostgreSQL database
- Automated loan scheduling and background processing
- Role and menu-based access control
- Reporting and monitoring features

## Table of Contents

- [Overview](#overview)
- [Process Flow Description](#process-flow-description)
- [Technology Stack](#technology-stack)
  - [Backend](#backend)
  - [Database](#database)
  - [DevOps](#devops)
- [System Architecture](#system-architecture)
- [Core Features](#core-features)
  - [Borrower Management](#borrower-management)
  - [Agent, Position and Access Management](#agent-position-and-access-management)
  - [Loan Application Management](#loan-application-management)
  - [Loan Approval Workflow](#loan-approval-workflow)
  - [Repayment Scheduling](#repayment-scheduling)
  - [Payment Recording](#payment-recording)
  - [Payment Correction and Audit History](#payment-correction-and-audit-history)
  - [Reporting](#reporting)
- [Mobile Application](#mobile-application)
- [Authentication and Authorization](#authentication-and-authorization)
- [Access Control](#access-control)
- [Entity Relationship Diagram](#entity-relationship-diagram)
  - [Transactional Consistency](#transactional-consistency)
  - [Auditability](#auditability)
- [Testing](#testing)
- [Deployment Environments](#deployment-environments)
- [CI/CD](#cicd)
- [Engineering Practices](#engineering-practices)
- [Author](#author)

## Process Flow Description
The LoanJuan Lending Management System manages the complete loan lifecycle from borrower registration to loan completion. The process covers four main stages: Loan Creation, Loan Application Review, Loan Processing, and Automated Scheduling.
A borrower is registered or selected, and a loan application is created. The application is then reviewed and either approved or rejected. Approved loans proceed to loan release and repayment schedule generation. The system then manages scheduled payment collection and updates the outstanding loan balance. This cycle continues until the loan is fully paid, at which point the loan is marked as completed.

<img width="1628" height="880" alt="LoanJuan Lending Management Process Flow (1)" src="https://github.com/user-attachments/assets/257be711-3c8e-4914-ab1f-d16275815553" />


---

## Technology Stack

### Backend

- C#
- ASP.NET Core
- .NET
- Entity Framework Core
- RESTful Web API
- JWT Authentication

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
│    Flutter Mobile App         │
│                               │
│ Borrower Management           │
│ Loan Applications             │
│ Loan Approval                 │
│ Payment Collection            │
│ Reporting                     │
│ Local SQLite Storage          │
└───────────────┬───────────────┘
                │
                │ HTTPS / REST API
                ▼
┌───────────────────────────────┐
│       ASP.NET Core API        │
│                               │
│ Authentication / JWT          │
│ Authorization                 │
│ Borrower Management           │
│ Loan Processing               │
│ Payment Processing            │
│ Reporting                     │
└───────────────┬───────────────┘
                │
                │ Entity Framework Core
                ▼
┌───────────────────────────────┐
│          PostgreSQL           │
│                               │
│ Borrowers                     │
│ Agents / Users                │
│ Loan Applications             │
│ Repayment Schedules           │
│ Payments                      │
│ Charges                       │
│ Audit / Historical Data       │
└───────────────┬───────────────┘
                │
                │ Read / Update
                │
                ▼
┌───────────────────────────────┐
│ Automated Background Process  │
│                               │
│ Scheduled Loan Processing     │
│ Repayment Processing          │
│ Overdue / Penalty Processing  │
│ Scheduler Logging             │
└───────────────┬───────────────┘
                │
                │ Update / Log Results
                │
                └──────────────► PostgreSQL

```

---

## Core Features

### Borrower Management

LoanJuan provides centralized borrower management for maintaining borrower records and assigning collection responsibilities to authorized personnel.
Key capabilities include:
- Borrower registration and profile management
- Borrower search and status tracking
- Collection officer assignment
- Loan and repayment monitoring

<p align="center">
<img width="300" 
      alt="Borrower Management" src="https://github.com/user-attachments/assets/7512a3a5-b3c5-4180-8aea-97b1932635a0" />
</p>      

<p align="center">
  <em>Borrower management interface showing borrower records, collection officer assignments, search, and status information.</em>
</p>

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
<p align="center">
  <em>Mobile interface for managing agents, positions, roles, and access.</em>
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

<p align="center">
<img width="300" 
      alt="Loan Applications" 
      src="https://github.com/user-attachments/assets/bd387b4f-fd25-4d77-8485-2a46146687a2" />
</p>  
<p align="center">
  <em>Loan applications provide the central relationship connecting borrowers, agents, repayment schedules, charges, and payments.</em>
</p>


---

### Loan Approval Workflow

LoanJuan provides a controlled loan approval workflow for authorized personnel to review and approve loan applications.

Before approval is completed, the system presents a confirmation step showing the borrower and loan amount and reminds the approver to verify important loan terms before proceeding.

The workflow supports:

- Review of loan applications
- Loan status tracking
- Authorized loan approval
- Verification of loan amount and borrower
- Confirmation of loan terms before approval
- Explicit approval confirmation
- Protection against accidental approval

<p align="center">
<img width="300" 
      alt="Loan Approval" 
      src="https://github.com/user-attachments/assets/4fc3a832-7204-4201-b435-c6773b495d74" />
</p>
<p align="center">
  <em>Loan approval confirmation requiring verification of key loan details before final approval.</em>
</p>

---

### Repayment Scheduling

LoanJuan provides detailed repayment tracking for approved loans, giving collection personnel a clear view of the borrower's loan terms, payment progress, and outstanding balance.

The repayment details include:

- Principal amount
- Interest rate
- Loan term and payment frequency
- Loan start and end dates
- Total collectible amount
- Payments collected
- Unpaid balance
- Advanced payments
- Scheduled repayment dates
- Amount due and amount paid
- Running loan balance

The loan statement provides a chronological view of the loan release and subsequent repayments, allowing users to track how each payment affects the remaining balance.

<p align="center">
<img width="300" 
      alt="Repayment Schedule" src="https://github.com/user-attachments/assets/91b504bf-df37-46ce-8c25-c019dda6601a" />
</p>
<p align="center">
  <em>Loan repayment details showing loan terms, collection summary, scheduled repayments, payments received, and running balance.</em>
</p>

---

### Payment Recording

The LoanJuan Collector mobile application allows collection personnel to record payments received directly from borrowers.

Before a payment is submitted, the collector can review the borrower's outstanding balance, enter the amount collected, and confirm the transaction to help prevent accidental or incorrect payment entries.

The payment recording workflow includes:

- Borrower identification
- Outstanding balance display
- Payment amount entry
- Payment confirmation
- Verification of the borrower and collected amount before submission
- Submission to the LoanJuan backend for payment processing

<p align="center">
<img width="300" 
      alt="Record Payment" src="https://github.com/user-attachments/assets/238d2f73-b416-4736-8d5e-1e743cbaeab9" />
</p>
<p align="center">
  <em>Collector application confirming a borrower payment before the transaction is submitted for processing.</em>
</p>

---

### Payment Correction and Audit History

LoanJuan provides a controlled payment correction workflow for voiding previously recorded payments.

Payment voiding is protected by OTP verification to help prevent unauthorized or accidental corrections. Before the void is completed, a one-time code is sent to the configured authorization recipient and must be entered by the user performing the correction.

The payment correction workflow includes:

- Selection of the payment to be voided
- OTP-based confirmation
- Payment void authorization and validation
- Reversal of affected payment calculations
- Repayment schedule recalculation
- Outstanding balance recalculation
- Audit information for payment corrections

<p align="center">
<img width="300" 
      alt="Payment Correction and Audit History" src="https://github.com/user-attachments/assets/806e9673-daae-488d-9dc5-2ff6e1247081" />
</p>
<p align="center">
  <em>OTP-protected payment correction workflow requiring verification before a recorded payment can be voided.</em>
</p>

---

### Reporting

LoanJuan provides administrative and operational reports that give lending personnel visibility into loan releases, repayments, collections, and borrower activity.

Available reports include:

- Overdue Loans
- Repayment Overview
- Cash Released Report
- Loan Client List
- Missed Dues
- Payment List

Reports support filtering and structured presentation of lending data, with selected reports available in printable and shareable formats.

<p align="center">
<img width="300" 
      alt="Cash Release Reports" src="https://github.com/user-attachments/assets/a8da1fad-f2b5-4a66-a13f-0405c9418374" />
</p>
<p align="center">
  <em>Cash Released Report showing released loans grouped by loan officer, including borrower, collection officer, loan term, principal, interest, payable amount, and daily repayment.</em>
</p>

---

## Mobile Application

LoanJuan includes a Flutter mobile application used by field personnel for borrower, loan, and collection activities.

The application integrates with the central LoanJuan API and uses SQLite for local application data.

Separate UAT and production configurations allow mobile builds to connect to the appropriate deployment environment.

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

<p align="center">
    <img height="533" alt="LoanJuanLending" src="https://github.com/user-attachments/assets/874c4cc5-8072-4494-b244-fbdf980b9b83" />
</p>

The LoanJuan API uses an automated Azure DevOps pipeline to build, test,
package, and deploy application changes from the development repository
to the UAT environment.

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
