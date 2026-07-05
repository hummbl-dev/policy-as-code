# Prior Art and Adjacent Ecosystem References

This document collects public prior art and adjacent ecosystem references relevant to `policy-as-code`.

> **Note: These are references, NOT adopted canon.**
>
> Nothing in this document is adopted as operating canon for this repository.
> These entries are collected to orient contributors, clarify the surrounding
> ecosystem, and make it easier to compare approaches. They are public,
> third-party projects and concepts. Adopting any of them as canon would
> require an explicit, audited decision recorded in this repository.
>
> See [`docs/principles.md`](./principles.md) ("Separate examples from adopted
> operating canon") and [`docs/definition.md`](./definition.md) for the
> boundary that applies here.

References are grouped into:

1. [Public prior art](#public-prior-art) — policy engines and policy languages.
2. [Adjacent ecosystem](#adjacent-ecosystem) — cloud-native policy and compliance surfaces.
3. [Vocabulary references](#vocabulary-references) — terms used across the domain.
4. [Key concepts](#key-concepts) — recurring mechanics worth naming explicitly.

## Public prior art

### Open Policy Agent (OPA)

- **What it does:** A general-purpose policy engine that evaluates structured
  data against policies written in its own language, Rego. Commonly embedded in
  services, used as a sidecar, or run as a standalone decision service.
- **Relevance to this repo:** A widely deployed reference for how a
  decoupled policy engine and policy language can work. Useful as a comparison
  point for evaluation semantics, input shape, and the boundary between policy
  authoring and policy enforcement.
- **Docs:** https://www.openpolicyagent.org/

### Rego

- **What it does:** The query/policy language used by OPA. Declarative,
  rule-based, and designed for expressing allow/deny and data-shape checks over
  JSON-like input.
- **Relevance to this repo:** A concrete example of a policy DSL with explicit
  rule syntax, variable binding, and a deny/allow idiom. Useful when discussing
  what a policy language needs to express and how readable it must be for
  review.
- **Docs:** https://www.openpolicyagent.org/docs/latest/policy-language/

### Cedar (AWS)

- **What it does:** A policy language and evaluation engine developed by AWS,
  used in services such as Amazon Verified Permissions. Designed around
  principals, actions, resources, and conditions.
- **Relevance to this repo:** A modern, formally specified policy language with
  a clear authorization model (principal/action/resource). Useful as a
  reference for schema-driven policy authoring and for separating policy from
  application code.
- **Docs:** https://www.cedarpolicy.com/

### HashiCorp Sentinel

- **What it does:** A policy-as-code framework integrated into HashiCorp
  tools (Terraform Cloud/Enterprise, among others). Policies are written in
  Sentinel's own language and evaluated against Terraform plans and other
  input.
- **Relevance to this repo:** A reference for policy-as-code applied to
  infrastructure change review — i.e., enforcing policy before a change is
  applied, with an emphasis on human-readable rules and enforcement levels
  (advisory, soft-mandatory, hard-mandatory).
- **Docs:** https://developer.hashicorp.com/sentinel

### Conftest

- **What it does:** A tool that evaluates structured configuration files
  (YAML, JSON, HCL, etc.) against Rego policies, built on top of OPA. Often
  used in CI pipelines to check Kubernetes manifests, Terraform plans, and
  similar artifacts.
- **Relevance to this repo:** A reference for how policy-as-code can be wired
  into CI and pre-merge review without requiring a running cluster or service.
  Useful when discussing where evaluation happens in a change lifecycle.
- **Docs:** https://www.conftest.dev/

### Kyverno

- **What it does:** A Kubernetes-native policy engine that uses Kubernetes
  resources themselves as the policy format (no separate DSL). Policies can
  validate, mutate, generate, and verify resources.
- **Relevance to this repo:** A reference for a "policy as data" approach that
  avoids a custom language in favor of the host system's native resource model.
  Useful when discussing the trade-off between a dedicated policy language and
  reusing an existing schema.
- **Docs:** https://kyverno.io/

### Gatekeeper (OPA Gatekeeper)

- **What it does:** An admission controller for Kubernetes that enforces OPA
  Rego policies at admission time, with support for audit, constraint
  templates, and constraint resources.
- **Relevance to this repo:** A reference for admission-control-style
  enforcement plus an audit mode that reports pre-existing violations without
  blocking them. Useful when discussing the deny/allow vs. audit distinction.
- **Docs:** https://open-policy-agent.github.io/gatekeeper/

## Adjacent ecosystem

### AWS Service Control Policies (SCPs)

- **What it does:** Organization-level guardrails in AWS Organizations that
  constrain the maximum permissions available to accounts within an
  organization. SCPs are not grants; they set the boundary that IAM policies
  operate within.
- **Relevance to this repo:** A reference for boundary-style policy at the
  organizational layer, distinct from per-resource enforcement. Useful when
  discussing where policy is enforced (org boundary vs. resource vs. request).
- **Docs:** https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html

### Azure Policy

- **What it does:** A service for defining, assigning, and evaluating policies
  over Azure resources, including built-in and custom definitions, compliance
  state tracking, and remediation.
- **Relevance to this repo:** A reference for a platform-managed policy
  lifecycle: definition, assignment, evaluation, compliance view, and
  remediation. Useful when discussing audit/compliance posture vs. hard
  enforcement.
- **Docs:** https://learn.microsoft.com/en-us/azure/governance/policy/

### GCP Organization Policy

- **What it does:** Google Cloud's organization-level policy service that
  enforces constraints on resource configuration across the organization,
  folders, and projects.
- **Relevance to this repo:** A reference for constraint-based organizational
  policy expressed as a fixed set of constraint types rather than a free-form
  language. Useful when discussing the trade-off between expressiveness and a
  small, reviewable constraint catalog.
- **Docs:** https://cloud.google.com/resource-manager/docs/organization-policy/overview

### CIS Benchmarks

- **What it does:** Prescriptive, consensus-based configuration benchmarks
  published by the Center for Internet Security, covering operating systems,
  cloud providers, databases, and other technologies.
- **Relevance to this repo:** A reference for how compliance guidance is
  expressed as concrete, checkable configuration recommendations with
  scoring/levels. Useful when discussing how prose compliance guidance maps to
  machine-checkable policy.
- **Docs:** https://www.cisecurity.org/cis-benchmarks

## Vocabulary references

These terms recur across the projects above and across this repository's own
docs. They are listed here for shared vocabulary, not as adopted definitions.

- **policy-as-code** — Treating policies as versioned, reviewable, testable
  source material rather than prose-only runbooks or click-ops configuration.
- **policy enforcement** — Acting on policy evaluation results (block, warn,
  mutate, remediate) at a defined point in a lifecycle.
- **admission control** — Evaluating and enforcing policy at the point a change
  is admitted (classically, a Kubernetes resource create/update/delete), as
  opposed to after-the-fact audit.
- **compliance automation** — Mechanically evaluating configuration or behavior
  against a compliance standard and producing evidence of state.

## Key concepts

These are recurring mechanics worth naming when comparing approaches. They are
descriptive labels, not design decisions for this repo.

- **policy engines** — Components that evaluate input against authored policy
  and return a decision. OPA, Cedar's evaluator, Sentinel, Kyverno, and
  Gatekeeper are all examples, with different embedding models.
- **evaluation** — The act of producing a decision (or set of decisions) from
  input plus policy. Where evaluation happens (CI, admission, runtime, audit)
  is a first-class design choice.
- **deny / allow** — The two most common decision outcomes. Many systems also
  support warn, mutate, or "compliant/non-compliant" for audit-style
  evaluation.
- **exceptions** — Mechanisms for recording an approved deviation from a
  policy, typically with scope, expiry, owner, and rationale. Relevant to both
  enforcement and audit.
- **audit** — Evaluating existing state against policy and reporting
  violations without necessarily blocking change. Distinct from admission
  control, and often used to establish a compliance baseline.

## Status

Reference collection. Not adopted canon. Additions and corrections are
welcome; adoption of any approach described here as part of this repository's
operating canon would require an explicit, audited decision.
