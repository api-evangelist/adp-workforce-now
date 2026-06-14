# ADP Workforce Now GraphQL Schema

## Overview

This directory contains a conceptual GraphQL schema for the ADP Workforce Now HCM v2 REST API. ADP Workforce Now is ADP's cloud-based human capital management (HCM) suite for mid-sized businesses, covering payroll, HR, benefits administration, time and attendance, talent management, and analytics in a single platform.

The schema models the core domain objects exposed by the ADP Workforce Now HCM v2 REST API and translates them into a unified GraphQL surface.

- **Developer Portal:** https://developers.adp.com
- **API Explorer:** https://developers.adp.com/build/api-explorer/hcm-offrg-wfn
- **Base URL:** https://api.adp.com
- **Authentication:** OAuth 2.0 (client credentials) with mutual TLS client certificates

## Schema File

- [`adp-workforce-now-schema.graphql`](adp-workforce-now-schema.graphql)

## Type Coverage

The schema defines 70+ named types across the following functional domains:

### Worker and Employment

| Type | Description |
|---|---|
| `Worker` | Root worker object linking all HCM data |
| `WorkerDetails` | Demographics, identifiers, custom fields |
| `WorkerStatus` | Active/inactive/terminated state with reason and effective date |
| `WorkerType` | Employee classification (full-time, part-time, contractor, etc.) |
| `WorkerDates` | Hire, rehire, termination, seniority, and benefit coverage dates |

### Identity and Contact

| Type | Description |
|---|---|
| `PersonName` | Container for legal, preferred, and alternate names |
| `LegalName` | Full legal name with title and suffix |
| `PreferredName` | Preferred/nickname name record |
| `AlternateName` | Additional name records by type |
| `PersonAddress` | Typed addresses (home, work, mailing) |
| `CommunicationInfo` | Container for emails, phones, and social handles |
| `WorkerEmail` | Typed email addresses with notification flags |
| `WorkerPhone` | Typed phone numbers with area code and extension |
| `SocialNetwork` | Social network URI records |
| `InstantMessage` | Instant message channel records |

### Assignment

| Type | Description |
|---|---|
| `Assignment` | Worker's current job assignment |
| `AssignmentDetails` | Position, title, hire date, business unit |
| `AssignmentStatus` | Assignment lifecycle state |

### Remuneration and Pay

| Type | Description |
|---|---|
| `BaseRemuneration` | Annual, hourly, and periodic rate amounts |
| `PayFrequency` | Pay period cadence and annual period count |
| `PayGrade` | Compensation grade classification |
| `PayRange` | Min/midpoint/max salary band |

### Payroll

| Type | Description |
|---|---|
| `Payroll` | Worker payroll configuration container |
| `PayrollDetails` | Pay group, frequency, tax codes |
| `PayrollRun` | Individual payroll processing run with period dates |
| `PayrollFrequency` | Payroll processing cadence |
| `WorkerPayment` | Per-worker payment detail within a run |
| `EarningCode` | Earning type code definitions |
| `DeductionCode` | Deduction type code definitions |
| `TaxWithholding` | Federal, state, and local tax withholding elections |
| `DirectDeposit` | Bank account direct deposit records |
| `AllocationDetails` | Deposit split allocation rules |
| `PaycheckDelivery` | Paper check delivery address |
| `EarningLine` | Line-level earning detail on a payment |
| `DeductionLine` | Line-level deduction detail on a payment |
| `TaxLine` | Line-level tax detail on a payment |

### Time and Attendance

| Type | Description |
|---|---|
| `TimeAndAttendance` | Worker time and attendance data container |
| `ScheduledShift` | Published work schedule shift |
| `TimeEntry` | Individual clock-in/out or manual time entry |
| `TimeCard` | Aggregated time card for a pay period |
| `PTO` | Paid time off balance by leave type |
| `LeaveRequest` | Leave of absence request record |
| `LeaveBalance` | Available, used, and pending leave balance |

### Benefits

| Type | Description |
|---|---|
| `Benefit` | Worker benefit enrollment record |
| `BenefitElection` | Active benefit election with contribution amounts |
| `BenefitPlan` | Benefit plan definition and carrier details |
| `BenefitCoverage` | Coverage tier with employee/employer premiums |
| `Enrollment` | Enrollment event record |
| `DependentCoverage` | Dependent included on a benefit election |

### Performance and Talent

| Type | Description |
|---|---|
| `PerformanceReview` | Formal performance appraisal record |
| `Goal` | Individual worker goal |
| `GoalStatus` | Goal lifecycle state |
| `Competency` | Competency assessed in a review |
| `CompetencyRating` | Rating applied to a competency |
| `Training` | Training completion record |
| `LearningCourse` | Learning course definition |

### Organization

| Type | Description |
|---|---|
| `Organization` | Top-level organization entity |
| `OrgChart` | Hierarchical org structure node |
| `Department` | Department with parent and manager linkage |
| `Position` | Budgeted position with vacancy tracking |
| `Location` | Physical work location |
| `CostCenter` | Financial cost center |
| `Job` | Job definition with pay grade and FLSA status |
| `JobCode` | Job code value object |
| `Team` | Cross-functional team grouping |
| `Manager` | Manager reference on worker/assignment |

### API and Integration

| Type | Description |
|---|---|
| `APIKey` | OAuth client credential record |
| `Token` | OAuth 2.0 access token |
| `WebhookEvent` | Delivered webhook event payload |
| `Subscription` | Webhook event subscription configuration |

### Utility

| Type | Description |
|---|---|
| `CustomField` | Flexible custom field value |
| `PageInfo` | Pagination cursor metadata |
| `WorkerConnection` | Paginated worker list result |

## Enumerations

| Enum | Values |
|---|---|
| `WorkerStatusCode` | ACTIVE, INACTIVE, TERMINATED, LEAVE, SUSPENDED |
| `WorkerTypeCode` | EMPLOYEE, CONTRACTOR, TEMPORARY, PART_TIME, FULL_TIME, INTERN |
| `AssignmentStatusCode` | ACTIVE, INACTIVE, PENDING, ENDED |
| `PayFrequencyCode` | WEEKLY, BIWEEKLY, SEMIMONTHLY, MONTHLY, ANNUAL |
| `GoalStatusCode` | NOT_STARTED, IN_PROGRESS, COMPLETED, CANCELLED, ON_HOLD |
| `CompetencyRatingCode` | EXCEEDS_EXPECTATIONS, MEETS_EXPECTATIONS, BELOW_EXPECTATIONS, NOT_RATED |
| `LeaveTypeCode` | VACATION, SICK, PERSONAL, FMLA, MATERNITY, PATERNITY, BEREAVEMENT, MILITARY, JURY_DUTY, OTHER |
| `WebhookEventTypeCode` | WORKER_HIRE, WORKER_TERMINATE, WORKER_TRANSFER, PAYROLL_RUN_COMPLETE, BENEFIT_ELECTION_CHANGE, TIME_ENTRY_SUBMITTED, LEAVE_REQUEST_APPROVED |

## Queries

The schema provides a `Query` root type covering:

- Worker lookup by ID and filtered list (status, department, location) with pagination
- Payroll run retrieval by ID and date range
- Time card and leave request queries
- Benefit and benefit plan queries
- Performance review queries
- Organizational hierarchy queries (org, department, position, location, job, team)
- Reference data (earning codes, deduction codes, learning courses)
- Webhook subscription management

## Mutations

The schema provides a `Mutation` root type covering:

- Leave request creation and status updates
- Time entry creation and time card submission
- Benefit election updates
- Goal creation and progress updates
- Webhook subscription creation and deletion
- Direct deposit account management
- Tax withholding election updates

## Authentication

The ADP Workforce Now HCM API uses OAuth 2.0 with the client credentials grant type. All requests require a mutual TLS client certificate issued by ADP in addition to the bearer token. Scopes control access to specific API product areas (workers, payroll, time, benefits, talent).

```
POST https://accounts.adp.com/auth/oauth/v2/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id={client_id}&client_secret={client_secret}
```

## References

- ADP Developer Portal: https://developers.adp.com
- HCM API Explorer: https://developers.adp.com/build/api-explorer/hcm-offrg-wfn
- ADP API Central: https://apps.adp.com/en-US/apps/410612/adp-api-central-for-adp-workforce-now-and-adp-workforce-now-next-generation/features
- ADP Marketplace: https://apps.adp.com
