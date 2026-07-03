# Operations Plan Document Specification

This specification defines the standard structure, required content, and depth
expectations for operations plan documents. It is the authoritative reference
for documenting how a production system is operated, monitored, and maintained.

The Architect produces the initial operations plan alongside the architecture
design document. The Operations Lead adapts it to the live production
environment after the first release and maintains it ongoing.

Every operations plan produced under this process must follow this
specification.

---

## Document Scope Levels

Every operations plan declares one of two scope levels:

| Level | Purpose | Location | When to use |
| --- | --- | --- | --- |
| `full` | Complete operations plan for a production system. | `architecture/` or approved product documentation. | First production release, major operational redesign, first-time operations documentation. |
| `scoped` | Operations update for a specific release or incident. | GitHub Issue comment, pull request, or linked document. | Release with operational impact, post-incident operational changes. |

A `scoped` document may reference a `full` document instead of repeating stable
content. A `scoped` document must never contradict a `full` document without
explicitly recording the override and its rationale.

---

## 1. Document Header

Every operations plan begins with a header containing these fields:

| Field | Required | Description |
| --- | --- | --- |
| Product | Always | Product or application name. |
| Scope level | Always | `full` or `scoped`. |
| Date | Always | Date of last substantive update. |
| Author | Always | Person or agent that produced or last updated the document. |
| Status | Always | `draft`, `review`, `accepted`, `superseded`. |
| Canonical path | Always | File path or URL where the authoritative version lives. |
| Architecture document | Always | Canonical path of the architecture design document this plan implements. |
| Related issues | When applicable | GitHub Issue numbers this document covers. |
| Supersedes | When applicable | Document this replaces. |
| Superseded by | When applicable | Document that replaces this one. |

Example:

```
Product:                Checkout Service
Scope level:            full
Date:                   2026-07-03
Author:                 Architect agent (initial), Operations Lead agent (adapted)
Status:                 accepted
Canonical path:         architecture/checkout-service-ops.md
Architecture document:  architecture/checkout-service.md
```

### Document Lifecycle

The operations plan follows the same lifecycle rules as the architecture design
document:

- One canonical location per document, recorded in the header.
- Agents and humans read from the canonical path. Copies are not authoritative.
- Superseded documents are updated with status and pointer, not deleted.
- Incremental updates increment the date. Do not create a new document for
  incremental updates.
- Agents verify status is `accepted` before treating content as settled.

### Authorship Phases

The operations plan has two authorship phases:

| Phase | Author | Trigger | Status on completion |
| --- | --- | --- | --- |
| Initial | Architect | Architecture design document accepted. | `draft` or `review`. |
| Adaptation | Operations Lead | First production release promoted. | `accepted` after adaptation. |

The Architect produces the initial operations plan based on the architecture
design document. Sections that require production data (actual baselines,
dashboard URLs, tested restore times) are marked as placeholders. The Operations
Lead replaces placeholders with actual values after observing the live system.

After adaptation, the Operations Lead owns the document. The Architect is
consulted when architecture changes affect operational behavior.

---

## 2. Service Overview

Ground the operations plan in the system it covers. Do not duplicate the
architecture design document. Reference it.

Record:

- **System purpose**: one sentence stating what the system does.
- **Primary users and consumers**: who depends on the system.
- **Architecture document reference**: canonical path of the architecture design
  document.
- **System dependencies**: external services, platform services, and
  infrastructure the system depends on. For each, record the dependency name,
  what it provides, and what happens when it is unavailable.
- **Business impact of downtime**: what happens to users and the business when
  the system is partially or fully unavailable. State in concrete terms, not
  abstractions.

---

## 3. Service Level Definitions

Service levels make reliability concrete and measurable. They connect
operational work to business outcomes.

### 3.1 Service Level Indicators (SLIs)

An SLI is a quantitative measure of service quality. For each SLI:

| Field | Description |
| --- | --- |
| Name | Descriptive name (e.g., "Checkout latency"). |
| Category | Availability, latency, throughput, correctness, or freshness. |
| Definition | How the indicator is calculated (e.g., "proportion of HTTP requests returning 2xx within 500ms, measured at the load balancer"). |
| Measurement point | Where in the system the measurement is taken. |
| Data source | The monitoring system or log that provides the raw data. |

Choose SLI categories based on what matters to users:

- **Availability**: could the user complete their request?
- **Latency**: how long did the user wait? Use percentiles (p50, p95, p99).
- **Throughput**: can the system handle the expected load?
- **Correctness**: did the system return the right answer?
- **Freshness**: is the data the user sees current?

Every system needs at least availability and latency SLIs. Add others when they
matter to the specific product.

### 3.2 Service Level Objectives (SLOs)

An SLO is a target value for an SLI. For each SLO:

| Field | Description |
| --- | --- |
| SLI | Reference to the SLI. |
| Target | Target value (e.g., "99.9% of requests succeed", "p95 latency < 200ms"). |
| Compliance window | Rolling 28-day or calendar month. |
| Error budget | 100% minus the target, stated in concrete terms (e.g., "99.9% availability = 43 minutes of downtime per 30 days"). |

Set SLOs based on user expectations and business requirements, not on what
sounds impressive. An SLO that is never breached is probably too loose. An SLO
that is constantly breached is not useful.

The Architect sets initial SLO targets based on the architecture design
document's capability requirements (section 7.1). The Operations Lead adjusts
targets after observing actual production behavior.

### 3.3 Error Budget Policy

Define what happens as the error budget is consumed:

| Budget remaining | Status | Actions |
| --- | --- | --- |
| >50% | Normal | Features ship normally. |
| 20-50% | Caution | Risky changes require review. Operations Lead monitors closely. |
| 1-20% | Restricted | Only reliability and security fixes proceed. Feature work pauses. |
| 0% | Frozen | All effort goes to reliability until budget recovers. |

Adapt thresholds to the team's risk tolerance. The error budget policy is a
guideline for prioritization decisions, not an automated system. The Product
Owner and Operations Lead use it to make informed trade-offs between features
and reliability.

---

## 4. Monitoring and Observability

### 4.1 Metrics Framework

State which metrics framework applies to this system. Choose based on system
type:

- **Four Golden Signals** (latency, traffic, errors, saturation): general
  purpose, recommended for most user-facing systems.
- **RED** (rate, errors, duration): focused on request-driven services.
- **USE** (utilization, saturation, errors): focused on infrastructure
  resources.

A system may use more than one framework. RED for the application layer and USE
for the infrastructure layer is a common combination.

### 4.2 Metrics Inventory

For each metric collected:

| Field | Description |
| --- | --- |
| Metric name | Name as it appears in the monitoring system. |
| Category | Which framework signal it belongs to (e.g., "latency", "saturation"). |
| Description | What it measures in plain language. |
| Collection method | How it is collected (instrumentation, agent, log parsing, platform metric). |
| Collection interval | How often it is sampled. |
| Retention period | How long the data is kept. |
| Dashboard | Which dashboard displays it. |

Do not list metrics for the sake of listing them. Every metric in the inventory
must connect to an SLI, an alert, a capacity concern, or a troubleshooting
need. If removing a metric would not reduce the team's ability to understand or
operate the system, remove it.

### 4.3 Dashboards

For each dashboard:

| Field | Description |
| --- | --- |
| Name | Dashboard name. |
| Location | URL or path. |
| Purpose | What operational question it answers. |
| Key panels | What metrics and visualizations it contains. |
| Review cadence | How often it should be reviewed (e.g., daily, weekly). |

At minimum, every system needs:

- A **service health dashboard** showing SLI status, error rates, and latency
  percentiles.
- A **capacity dashboard** showing resource utilization and headroom.

### 4.4 Logging

Record the logging strategy:

- **Log aggregation**: where logs are collected and how they are accessed.
- **Log format**: structured JSON, required fields (timestamp, level,
  correlation ID, component).
- **Log levels**: what is logged at each level (error, warn, info, debug).
- **Log retention**: how long logs are kept at each storage tier.
- **Sensitive data exclusion**: how PII, secrets, and restricted data are
  prevented from appearing in logs. Reference the data classification from the
  architecture design document (section 4.4).

### 4.5 Health Checks

For each health check endpoint or probe:

| Field | Description |
| --- | --- |
| Endpoint | URL, command, or probe identifier. |
| Type | Liveness (is the process running), readiness (can it serve traffic), or deep (are dependencies healthy). |
| Frequency | How often the check runs. |
| Expected response | What a healthy response looks like. |
| Timeout | Maximum response time before marking unhealthy. |
| Dependencies checked | Which downstream services are verified (deep checks only). |

---

## 5. Alerting

### 5.1 Alert Definitions

For each alert:

| Field | Description |
| --- | --- |
| Alert name | Descriptive name. |
| Condition | The threshold or pattern that triggers the alert. Be specific: "error rate > 1% for 5 minutes", not "high error rate". |
| Severity | Critical, warning, or informational. |
| Notification channel | How the alert is delivered (page, Slack, email). |
| Responder | Who responds to this alert. |
| Runbook | Link to the response procedure. |
| Expected response time | How quickly someone should acknowledge. |

Every alert must have a runbook. An alert without a response procedure produces
noise, not action.

Avoid alert fatigue. Every alert should be actionable. If an alert fires
regularly and the response is "ignore it," remove the alert or fix the
underlying condition.

### 5.2 Escalation Matrix

| Severity | First responder | Escalation trigger | Escalation target | Update cadence |
| --- | --- | --- | --- | --- |
| Critical | Operations Lead | No acknowledgment in 15 min, or no mitigation in 1 hour | Operator | Every 30 minutes |
| Warning | Operations Lead | No resolution in 4 hours | Technical Lead | Every 2 hours |
| Informational | Operations Lead reviews on next check | Trend worsening over 24 hours | Operations Lead investigates | Daily review |

Adapt response times and escalation targets to the team's size and
availability. A small team with no on-call rotation has different escalation
paths than a team with 24/7 coverage.

---

## 6. Incident Management

### 6.1 Severity Classification

| Severity | Definition | Response time | Resolution target |
| --- | --- | --- | --- |
| Critical | System unavailable or data at risk. Users cannot complete core tasks. | 15 minutes | 4 hours |
| Major | Significant functionality degraded. Users affected but workarounds may exist. | 1 hour | 24 hours |
| Minor | Partial degradation, workaround exists, limited user impact. | 4 hours | 3 business days |
| Low | Cosmetic issue, minor performance degradation, no user-visible impact. | 1 business day | Next maintenance window |

Adapt definitions and targets to the specific system. State what "unavailable"
means for this product. State what "core tasks" are.

### 6.2 Incident Response Procedure

Document the standard response flow:

1. **Detection**: how the incident is discovered (alert, user report, monitoring
   review).
2. **Classification**: assign severity based on the definitions above.
3. **Communication**: notify stakeholders per the escalation matrix.
4. **Investigation**: identify the cause using logs, metrics, traces, and
   runbooks.
5. **Mitigation**: stop the immediate impact (rollback, failover, scale,
   disable feature).
6. **Resolution**: fix the root cause or apply a durable workaround.
7. **Recovery**: verify the system is healthy. Verify no data loss or
   corruption.
8. **Post-incident review**: conduct review for critical and major incidents.

### 6.3 Post-Incident Review

Required for every critical and major incident. Complete within 48 hours of
resolution.

Template:

| Field | Content |
| --- | --- |
| Incident summary | 2-3 sentences: what happened, impact, duration. |
| Timeline | Chronological events from detection to resolution with timestamps. |
| Impact | Users affected, functionality disrupted, data implications, SLO impact. |
| Root cause | Why it happened. Use iterative "why" questioning to get past symptoms. |
| What went well | What worked during detection and response. |
| What went poorly | What was slow, missing, or broken in the response. |
| Action items | Specific fixes with owner, deadline, and definition of done. |

Post-incident reviews are blameless. The question is "what about our system
allowed this to happen," not "who caused this." Focus on systems, processes, and
tooling.

Action items from post-incident reviews become GitHub Issues. They enter the
maintenance lifecycle through the Technical Lead.

---

## 7. Operational Procedures

### 7.1 Runbook Format

For each operational procedure, produce a runbook:

| Field | Description |
| --- | --- |
| Name | Descriptive name. |
| Purpose | What task this runbook accomplishes. |
| Trigger | When to use this runbook (alert name, symptom, scheduled task). |
| Prerequisites | Access, tools, and conditions required before starting. |
| Steps | Numbered steps with exact commands and expected output per step. |
| Verification | How to confirm the procedure succeeded. |
| Rollback | How to undo if something goes wrong. |

Runbooks must be specific enough that someone unfamiliar with the system can
follow them. "Check the database" is not a runbook step. "Run
`SELECT count(*) FROM orders WHERE status = 'stuck'` and verify the count is 0"
is.

### 7.2 Required Runbooks

Every operations plan must include runbooks for:

- System restart or recovery after a crash.
- Database backup verification.
- Database restore from backup.
- Log retrieval for a specific time range and component.
- Scaling actions when manual scaling is needed.
- Dependency failure response for each critical dependency.

Add runbooks for system-specific operational tasks as needed.

### 7.3 Deployment Operations

Document how deployments affect operations. This is the Operations Lead's view
of deployment, not the Release role's promotion procedure.

- **Pre-deployment checks**: what the Operations Lead verifies before a
  deployment proceeds (system health, capacity headroom, recent incident
  status).
- **Monitoring during deployment**: what metrics to watch during and immediately
  after deployment, and what thresholds trigger a rollback.
- **Post-deployment verification**: how to confirm the deployment is healthy
  (health checks, smoke tests, SLI comparison to pre-deployment baseline).
- **Rollback criteria**: when to roll back versus fix forward. State specific
  thresholds (e.g., "error rate doubles within 15 minutes of deployment").

---

## 8. Capacity and Performance

### 8.1 Current Baseline

Document the current system capacity. The Architect estimates initial values
from architecture design and load testing. The Operations Lead replaces
estimates with measured production values.

| Resource | Current utilization | Capacity limit | Headroom |
| --- | --- | --- | --- |
| Compute (CPU) | average %, peak % | cores or units | % remaining at peak |
| Memory | average usage | total available | % remaining |
| Storage | used | total | % remaining |
| Database connections | average | pool maximum | % remaining |
| Request throughput | average req/s, peak req/s | tested maximum req/s | % remaining at peak |

Add rows for resources specific to the system (queue depth, cache hit rate,
external API rate limits, etc.).

### 8.2 Growth Projections

State expected growth over the next 6 and 12 months. Base projections on
observed trends, business plans, or product roadmap.

Identify which resource will hit its capacity limit first and when. This is the
system's scaling bottleneck.

### 8.3 Scaling Triggers

For each scalable resource:

| Resource | Warning threshold | Action threshold | Scaling action | Responsible |
| --- | --- | --- | --- | --- |
| Compute | 70% sustained | 80% sustained | Add capacity (specify how) | Operations Lead |
| Storage | 70% used | 80% used | Expand (specify how) | Operations Lead |

Adapt thresholds to the specific system. Stateful resources (databases, queues)
need more headroom than stateless compute.

### 8.4 Performance Tracking

Reference the architecture design document's performance requirements (section
7.1). Record actual measured performance against those requirements:

| Metric | Architecture target | Current measured value | Status |
| --- | --- | --- | --- |
| p95 checkout latency | < 200ms | measured value | met / not met |
| Availability | 99.9% | measured value | met / not met |

Update this table at least monthly.

---

## 9. Backup and Recovery

### 9.1 Backup Schedule

For each data store:

| Data store | Backup method | Frequency | Retention | Storage location | Encryption |
| --- | --- | --- | --- | --- | --- |
| | | | | | |

Reference the data classification from the architecture design document
(section 4.4). Restricted and confidential data must be encrypted in backups.

### 9.2 Recovery Objectives

| Data store | RTO | RPO | Last restore test | Restore test result |
| --- | --- | --- | --- | --- |
| | | | | |

- **RTO** (Recovery Time Objective): maximum acceptable time from failure to
  restored service.
- **RPO** (Recovery Point Objective): maximum acceptable data loss measured in
  time (e.g., "1 hour of data" means the most recent 1 hour of writes may be
  lost).

The Architect sets initial RTO and RPO targets from the architecture design
document. The Operations Lead validates them through restore testing.

### 9.3 Restore Procedures

Reference the restore runbook for each data store (section 7). State the
expected restore duration and any data implications (e.g., "transactions
committed after the last backup are lost").

### 9.4 Disaster Recovery

When the system requires disaster recovery beyond single-component restore:

- **DR strategy**: active-passive, active-active, cold standby, or manual
  rebuild. Reference the architecture design document if the strategy is
  defined there.
- **Failover procedure**: reference the runbook.
- **Failback procedure**: reference the runbook.
- **DR test schedule**: how often DR is tested. Record the last test date and
  result.

Small systems that can be rebuilt from code and backups within the RTO may
not need a formal DR strategy. Record that decision explicitly rather than
leaving the section blank.

---

## 10. Security Operations

### 10.1 Security Monitoring

For each monitoring area:

| Area | What to watch for | Tool or method | Alert threshold |
| --- | --- | --- | --- |
| Authentication | Brute force, credential stuffing, unusual login patterns | | |
| Authorization | Privilege escalation, unauthorized access attempts | | |
| Traffic | DDoS indicators, scanning, anomalous volume or patterns | | |
| Dependencies | CVE notifications, security advisories for libraries and platform services | | |

Cover the areas relevant to the system. Reference the threat model and attack
surfaces from the architecture design document (section 7.1, Security
capability).

### 10.2 Vulnerability Management

- **Detection**: how vulnerabilities are discovered (dependency scanning,
  container scanning, security advisories, penetration testing).
- **Patching cadence**: target time from vulnerability disclosure to patch
  applied, by severity. Example: critical within 48 hours, high within 1 week,
  medium within 1 month.
- **Responsibility**: who patches each component type (application
  dependencies, platform services, infrastructure).

### 10.3 Access Review

- **Production access**: who has access to production systems and data.
- **Review cadence**: how often access is reviewed (at least quarterly).
- **Revocation**: how stale or unnecessary access is removed.

---

## 11. Operational Readiness

### 11.1 Operational Readiness Checklist

The Operations Lead reviews this checklist before subsequent production releases
(after the Operations Lead is active). The checklist is a review, not a
blocking gate. The Operations Lead flags unresolved concerns to the Technical
Lead and Release role. Critical concerns are escalated to the operator.

- Monitoring covers new or changed components.
- Alerts exist for new failure modes introduced by the release.
- Runbooks are updated for new operational procedures.
- Capacity impact of the release is assessed.
- Backup procedures cover new data stores.
- Security monitoring covers new attack surfaces.
- Operations plan is updated to reflect the release.
- Rollback procedure is documented and understood.
- Performance impact is assessed (no SLO regression expected).

Add checklist items specific to the system as needed.

---

## 12. Scaling Rules

### Full Document Requirements

A `full` operations plan must include all sections:

| Section | Minimum depth |
| --- | --- |
| 1. Header | All fields including architecture document reference. |
| 2. Service Overview | System purpose, dependencies, business impact of downtime. |
| 3. Service Levels | At least one SLI per relevant category. SLO for each SLI. Error budget policy. |
| 4. Monitoring | Metrics framework declared. Metrics inventory. At least service health and capacity dashboards. Logging strategy. Health checks. |
| 5. Alerting | Alert definition for every monitored threshold. Escalation matrix. |
| 6. Incident Management | Severity classification adapted to the system. Response procedure. Post-incident review template. |
| 7. Procedures | Runbook for every required procedure. Deployment operations documented. |
| 8. Capacity | Current baseline (measured or estimated). Growth projections. Scaling triggers. Performance tracking against architecture targets. |
| 9. Backup and Recovery | Backup schedule for every data store. Recovery objectives. Restore procedures referenced. |
| 10. Security Operations | Security monitoring for relevant areas. Vulnerability management process. Access review cadence. |
| 11. Operational Readiness | Checklist populated for the system. |

### Scoped Document Requirements

A `scoped` operations update must include:

| Section | Requirement |
| --- | --- |
| 1. Header | Product, scope level, date, canonical path of parent `full` document, related issues or release. |
| Changed sections only | Document only what changes. Reference the `full` document for unchanged content. |

A `scoped` update that changes content in the `full` document must trigger an
update to the `full` document. The `scoped` update records the change; the
`full` document absorbs it.

### When to Produce Each Level

Produce a `full` document when:

- The product has no existing operations plan.
- The system undergoes major operational redesign.
- The Architect produces an initial operations plan for a new product.

Produce a `scoped` update when:

- A release changes operational behavior (new components, changed metrics, new
  failure modes).
- A post-incident review identifies operational changes.
- Capacity or performance baselines shift significantly.

Update an existing `full` document when:

- A `scoped` update introduces changes that affect the overall operations plan.
- Components, dependencies, or infrastructure change.
- Quarterly review identifies stale content.

---

## Mermaid Diagram Standards

Operations plan documents may include diagrams when they aid understanding.
Follow the same Mermaid conventions as the architecture design document
specification. Common diagram types:

- **Escalation flow** (`flowchart`): who is notified at each severity level.
- **Incident response flow** (`flowchart`): the steps from detection to
  resolution.
- **Dependency map** (`graph`): what the system depends on and failure impact.

Diagrams are recommended but not required for every section. Use them when the
relationships or flows are complex enough that prose alone is unclear.
