# Wildwest Founder Brief

## Working Thesis

AI agents are becoming operators.

They no longer only chat, summarize, or draft text. They read files, run commands, modify code, inspect infrastructure, call tools, access services, and make decisions inside real operational environments.

Today, most agent systems treat that authority casually. Agents often run as the human user, inside the human user's account, with blurry boundaries around credentials, filesystem access, process ownership, and accountability.

Wildwest starts from a different premise:

> If agents are operators, they should have operator identities.

Wildwest is a control layer for AI agents operating on real systems. It gives agents explicit identities, scoped authority, durable instructions, observable execution, and revocable access.

The first concrete primitive is Linux-native: an agent can be created and managed as a real system user. That gives the agent its own Unix identity, home directory, permissions, environment, process ownership, logs, and lifecycle.

The larger company vision is broader:

> Wildwest makes AI agents governable enough to operate in serious, regulated, and security-sensitive environments.

## The Problem

AI platforms are racing to sell agents, but regulated enterprises do not only buy capability. They buy controlled capability.

A bank, hospital, insurer, defense contractor, industrial operator, public company, or government agency cannot safely rely on agents that act through shared accounts, inherited user permissions, vague service identities, or incomplete logs.

They need to answer basic operational questions:

- Which agent acted?
- On whose authority?
- Under which instructions?
- Under which policy?
- With which credentials?
- Against which resource?
- What did the agent observe?
- What did it decide?
- Which tools did it call?
- What changed?
- Was human approval required?
- Can the session be reconstructed?
- Can the agent be disabled or revoked?
- Can an auditor verify the control?

This is not only a chatbot problem. It is an operational governance problem.

Normal SaaS access control asks:

> Can this user access this feature?

Agentic governance asks:

> Can this delegated non-human operator pursue this goal, using these tools, over this data, for this user, in this environment, within this risk budget, with this level of autonomy, and produce an audit trail good enough for review later?

That is a harder control surface.

## The Market Gap

Major AI and cloud platforms already speak the language of enterprise security: SOC 2, ISO 27001, HIPAA readiness, IAM, logging, data protection, tenant isolation, and admin controls.

Those controls matter, but they do not automatically prove that every autonomous agent action has:

- a distinct non-human identity
- a scoped execution boundary
- explicit delegated authority
- policy evaluation before action
- tamper-resistant logs
- replayable session evidence
- controlled credential access
- lifecycle and revocation
- compliance-ready audit records

Platform compliance is not the same thing as agent governance.

Wildwest can become the missing trust layer between AI capability and enterprise operations.

## Product Vision

Wildwest should not be framed as just another AI platform.

The stronger category is:

> Agent Control Infrastructure

Other possible category phrases:

- Agent IAM
- Agent operations platform
- Governance and execution controls for AI operators
- Identity, policy, and audit for autonomous agents
- The operating authority boundary for AI agents

The strongest positioning:

> Wildwest gives AI agents their own controlled operating identities, so organizations can delegate work without handing agents uncontrolled human authority.

Or:

> Wildwest is the trust layer for AI agents operating inside real organizations.

## Product Layers

Wildwest can evolve in layers.

### 1. Local Agent Users

Manage agents as real Linux users on a local machine or server.

Core capabilities:

- create an agent user
- attach instructions and metadata
- grant scoped filesystem access
- run commands or agent runtimes as that user
- inspect agent state
- disable or delete the agent

This is the first wedge because it is concrete, inspectable, and rooted in proven operating-system primitives.

### 2. Policy and Capability Control

Define what each agent can read, write, execute, access over the network, or escalate to.

This may eventually involve:

- Unix users and groups
- sudo or doas policy
- file ACLs
- namespaces
- cgroups and quotas
- AppArmor, SELinux, or seccomp
- network policy
- secrets brokering
- approval workflows

The product should be careful with security language. A Linux user boundary is useful, but it is not a complete sandbox by itself.

Honest claim:

> Wildwest uses Linux account isolation and least-privilege controls to reduce accidental authority sharing between humans and agents.

Claim to avoid until stronger controls exist:

> Wildwest securely sandboxes AI agents.

### 3. Execution Control

Wildwest should manage how agent work runs.

It does not need to build the agent model or agent loop first. A more durable product manages the authority around agents regardless of which agent runtime is used.

Wildwest could run:

- Codex
- Claude Code
- OpenHands
- custom internal agents
- scripts
- infrastructure bots
- vertical AI operators

The value is in the boundary layer: identity, permissions, execution context, audit, lifecycle, policy, and delegation.

### 4. Evidence and Audit

Wildwest should produce records that security teams and auditors can understand.

Potential evidence:

- who created the agent
- why it was created
- what instructions it received
- what policy applied
- when it ran
- what process executed
- what commands or tool calls occurred
- what files changed
- what approvals were requested or granted
- what credentials were accessed
- when access was revoked

This becomes especially important for ISO, SOC 2, HIPAA, financial services, government, and internal risk teams.

### 5. Enterprise Control Plane

After the local primitive is proven, Wildwest can become a central control plane for teams and fleets.

Possible enterprise capabilities:

- agent identity inventory
- templates for standard agent roles
- team RBAC
- SSO integration
- policy-as-code
- approval workflows
- SIEM export
- retention controls
- fleet management
- compliance evidence reports
- integration with cloud IAM and secret managers

### 6. Platform Trust Layer

The largest version of Wildwest is infrastructure that other AI platforms use to deliver trusted agent execution.

In this model, Wildwest is not the end-user AI app. It is the control substrate that AI platforms, enterprise AI teams, and vertical software vendors use when their agents need to operate in serious customer environments.

Wildwest could become:

- Okta for non-human AI identities
- Vault for delegated agent secrets
- Kubernetes-like orchestration for agent execution boundaries
- Datadog or Splunk-like evidence for agent actions
- Vanta or Drata-like evidence collection for AI operations
- sudo, auditd, AppArmor, and policy controls wrapped in a usable product

## Strategic Insight

The durable value is not in one specific model, vendor, or agent loop.

Models will change. Agent frameworks will change. Tool protocols will change. AI platforms will come and go.

But serious organizations will continue to need:

- identity
- access control
- policy
- execution boundaries
- audit
- lifecycle management
- delegation
- revocation
- compliance evidence

Wildwest should own that layer.

## Initial Technical Wedge

The first credible version can be small, but it should embody the larger thesis.

A useful prototype could include:

- `wildwest create <name>`
- `wildwest instruct <name> <text-or-file>`
- `wildwest grant <name> --path <path> --mode <read|write|rw>`
- `wildwest run <name> -- <command>`
- `wildwest inspect <name>`
- `wildwest disable <name>`

This first version proves:

- agent identities can be real system users
- authority can be separated from the human account
- instructions can be attached to an operator identity
- permissions can be granted deliberately
- execution can happen as the agent user
- the agent can be inspected and revoked

The prototype does not need to solve every enterprise control problem immediately. It needs to prove that agent delegation can be modeled as managed operating authority rather than ambient human authority.

## Non-Goals for the First Version

Wildwest should avoid trying to solve too much at once.

Early non-goals:

- building a new foundation model
- building a full agent framework
- replacing existing agent CLIs
- claiming perfect sandboxing
- solving multi-cloud IAM immediately
- building a broad no-code AI platform
- supporting every operating system
- handling every compliance framework in detail

The first version should make the core primitive real and credible.

## Founder-Level Framing

Short version:

> Wildwest is the control layer for AI agents operating on real systems.

Longer version:

> Wildwest gives AI agents explicit operating identities, scoped permissions, auditable execution, and revocable authority, so organizations can safely delegate real work to agents without handing them uncontrolled human access.

Regulated-industry version:

> Wildwest makes agentic AI admissible in regulated operations by providing the identity, access, policy, execution, logging, and evidence controls enterprises need before autonomous agents can touch real systems.

Developer wedge version:

> Wildwest lets developers create AI operators as real Linux users, each with its own permissions, instructions, execution context, and lifecycle.

## Open Questions

Important questions to resolve next:

- Is Wildwest first a developer tool, an enterprise control plane, or infrastructure for AI platforms?
- Should the first customer be individual developers, infra/security teams, regulated enterprises, or AI platform companies?
- How much of the agent runtime should Wildwest own versus integrate with?
- What is the minimum evidence trail that makes the product meaningfully different from a wrapper around `useradd` and `sudo`?
- Which security controls are required before using the word "sandbox"?
- What compliance mappings matter first: SOC 2, ISO 27001, ISO/IEC 42001, HIPAA, PCI, financial-services controls, or internal audit requirements?
- Should Wildwest be open source, commercial source, or closed source with an open local agent?

