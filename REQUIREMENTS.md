# ShiftPQC high-level requirements

| | |
| --- | --- |
| Version | **0.1.0** |
| Status | Draft for CEO + CTO review. Not an implementation plan. |
| Date | 2026-09-02 |
| Staging | `brianreborn/green-shiftz` (private, temporary) |
| Product | ShiftPQC™ |

This document is the first review gate. Finer requirements come only after acceptance or an explicit revision.

## Semver

| Bump | When |
| --- | --- |
| MAJOR | Incompatible change to a MUST |
| MINOR | Additive MUST or SHOULD |
| PATCH | Clarification; no new obligation |

Each MUST/SHOULD has a stable ID. Do not reuse IDs. Supersede in a new version.

## Inherited sources (not first-hand)

Origin kept. Lower weight than verification against running code or a signed decision.

| Origin | What it is |
| --- | --- |
| `/usr/home/locked/mvp-prompt.txt2` | CEO charter (2026-08-28). Canonical over `mvp-prompt.txt`. |
| [shiftpqc/mvp](https://github.com/shiftpqc/mvp) | Declared functional prototype. **README only** (commit `169826f`, 2026-07-15). Layout and APIs described; no service code in the repo. |
| [shiftpqc.com](https://shiftpqc.com/) | Public positioning: BYOC, air-gapped logic, CBOM, blast radius, hybrid FIPS 203. |
| [green-agentz](https://github.com/brianreborn/green-agentz) lessons + VCS-01…18 | Named repo/host/landing; clean checkout ≠ fleet consistency; preserve authored delta, not third-party payload. |
| [pqfreebsd swarm-conductor](https://github.com/brianreborn/pqfreebsd/blob/main/docs/swarm-conductor.md) | Dispatch ≠ implement. GitHub is mailbox, not TCB. |
| Green-Roomz / Agentz memory loop | Six-state cognitive coordinate. Not a product feature unless OPEN-04 says integrate. |

## Open decisions

Resolve before finer requirements. Do not paper over them in code.

| ID | Decision | Notes |
| --- | --- | --- |
| OPEN-01 | CISO dashboard runtime | Charter: Fresh + Deno. Prototype README: Angular + Node 20. Pick one; do not ship both. |
| OPEN-02 | Eventual landing repository | This tree is temporary. `shiftpqc/mvp` is public README-only and this GitHub identity currently has pull, not push. |
| OPEN-03 | SOC 2 / enterprise assurance scope | Charter is unsure. Do not claim SOC 2. Capture control *themes* (access, audit, change, vendor) as SHOULD until a named framework is chosen. |
| OPEN-04 | CTO platform integration | Integrate green-zkillz, swarm CI, pqfreebsd, green-roomz only where they fit a BYOC PQC product. Push back rather than import a second product. |
| OPEN-05 | Metrics vs no-exfil | Charter wants investor/client metrics *and* no automatic customer-data exposure. Telemetry must be customer-local or aggregated by the customer, unless an exceptional human sysadmin process is invoked. |
| OPEN-06 | “100% test coverage” | Treat as a debt risk, not a MUST. Coverage of security, migration, and rollback paths is MUST; blanket 100% is not. |
| OPEN-07 | Copyright and license of this tree and of the product | Unset. NOTICE.md is not a grant. |
| OPEN-08 | Graph store | Prototype README names Neo4j for blast radius. Not a MUST until CBR data model is specified. |

## Non-goals (this version)

- Implementing services, dashboard, or Docker stack in this repository.
- Treating `shiftpqc/mvp` README diagrams as existing code.
- Job requisitions (sysadmin lead, demo engineer, part-time security/QA). Those are org work, not product MUSTs.
- Replacing customer HSMs, CAs, or data-plane crypto libraries in-process.
- Automatic access to customer key material or payloads.

---

## Governance

- **REQ-GOV-01 [Debt and scope] MUST.** Do not accept decisions that create avoidable technical debt. Scope creep is debt. New work needs a requirement ID and a semver bump.
- **REQ-GOV-02 [Review gate] MUST.** High-level requirements are reviewed by CEO and CTO before finer requirements or implementation.
- **REQ-GOV-03 [Readable] MUST.** Requirements and later code must be parseable by a future human CTO or a junior engineer who was not in the originating session.
- **REQ-GOV-04 [Prototype] MUST.** [shiftpqc/mvp](https://github.com/shiftpqc/mvp) is the functional prototype for *what the product does*. Conflicts with this document are OPEN items, not silent overrides.
- **REQ-GOV-05 [No duplicate platform] MUST.** Do not reimplement green-agentz, green-zkillz, green-roomz, pqfreebsd, or swarm CI inside ShiftPQC. Integrate, pin, or reject with a recorded reason (OPEN-04).

## Product jobs

The product does three things. Nothing else is in 0.1.0 scope.

- **REQ-JOB-01 [CBOM] MUST.** Discover and inventory cryptographic assets. Emit CycloneDX CBOM (prototype: v1.7).
- **REQ-JOB-02 [CBR] MUST.** Map cryptographic blast radius across infrastructure dependencies: what breaks if a given credential, certificate, or algorithm is rotated or replaced.
- **REQ-JOB-03 [Shift] MUST.** Orchestrate migration from legacy public-key crypto (RSA, ECC) toward NIST PQC, in hybrid mode, targeting FIPS 203 (ML-KEM), FIPS 204 (ML-DSA), and FIPS 205 (SLH-DSA).

Phase order is CBOM → CBR → Shift. Later phases MUST consume earlier artifacts; they MUST NOT require a second discovery pass as a substitute for CBOM.

## Users

- **REQ-USR-01 [CISO] MUST.** A chief information security officer uses a high-level dashboard to see posture and make high-level decisions in real time. Runtime is OPEN-01.
- **REQ-USR-02 [SRE] MUST.** A site reliability engineer integrates the product into an existing cloud via containerized, read-oriented APIs (Go + Rust). The product MUST NOT require the SRE to put ShiftPQC on the data plane.
- **REQ-USR-03 [Consultancy] SHOULD.** Packaging supports white-label use by consultancies selling to CISOs (public site claim). Customization per CISO is a demo/config concern, not a second product.

## Stack

Charter stack. OPEN-01 is the only allowed exception, and only for the dashboard runtime.

- **REQ-STK-01 [Rust] MUST.** Lowest infrastructure / crypto engine layer is Rust.
- **REQ-STK-02 [Go] MUST.** Microservices layer is Go.
- **REQ-STK-03 [TypeScript] MUST.** Full-stack web application layer is TypeScript.
- **REQ-STK-04 [No extra languages] MUST.** Do not add a fourth application language to the product. Host glue (Make, Docker, CI YAML, shell) is allowed.

Prototype layout (inherited, not yet normative file tree):

```
api/                 OpenAPI contract
dashboard/           CISO UI (OPEN-01)
engine/              Rust cryptographic engine
services/gateway     API gateway and auth boundary
services/cbom        CBOM discovery
services/cbr         Blast radius
services/orchestrator  Migration automation
deploy/docker        Local and BYOC compose
marketing/           Static landing (out of 0.1.0 product MUST)
```

## Deployment and reliability

- **REQ-DEP-01 [BYOC] MUST.** Deployed into the customer’s cloud or network. Bring-your-own-cloud. The product is “off-line” relative to ShiftPQC operators: not a ShiftPQC-hosted SaaS control plane over customer crypto.
- **REQ-DEP-02 [Container] MUST.** Delivered as a dockerized stack that can be dropped into an existing environment.
- **REQ-DEP-03 [Clouds] SHOULD.** First-class discovery adapters for AWS, Azure, and GCP. Prototype currently specifies AWS read-only ACM / ELBv2 / API Gateway calls plus a `mock` provider. Azure/GCP are SHOULD until an adapter is specified.
- **REQ-REL-01 [Zero downtime] MUST.** The product MUST NOT be the cause of a site-unreliability event, during deployment or afterward.
- **REQ-REL-02 [Rollback] MUST.** Migration actions that change customer crypto posture MUST be hybrid and reversible. Prototype public site: hybrid mode beside legacy RSA; automated PR injection is a candidate mechanism, not a MUST until specified.
- **REQ-REL-03 [Least privilege] MUST.** Cloud integration uses least-privilege, read-oriented IAM (prototype example: `acm:DescribeCertificate` / list+describe). No private key material.

## Data, privacy, APIs

- **REQ-DAT-01 [No automatic egress] MUST.** Customer data is never exposed to ShiftPQC operators by an automatic process. Exceptional access is a human sysadmin action, not a pipeline.
- **REQ-DAT-02 [No data plane] MUST.** APIs used to integrate with the customer environment do not mutate the customer data plane. Discovery and mapping are read-oriented.
- **REQ-DAT-03 [Local metrics] MUST.** Metrics the CEO wants for investors, clients, and internal stakeholders (OPEN-05) are produced *in the customer deployment* or from data the customer has authorized. They are not a side channel out of BYOC.
- **REQ-API-01 [Contract] SHOULD.** One OpenAPI contract shared across services (prototype). Gateway is the auth boundary.
- **REQ-API-02 [CBOM scan] SHOULD.** Prototype shape, not yet frozen: `POST /api/v1/cbom/scans`, poll by `scanId`, export CycloneDX. Classification of legacy vs PQC is an engine concern (`/analyze` in the README).

## Dashboard and demo

- **REQ-UI-01 [At a glance] MUST.** CISO dashboard shows posture, blast radius, and migration state without requiring an SRE workflow.
- **REQ-UI-02 [High-level control] MUST.** CISO can make high-level decisions in real time. Finer mechanics stay in APIs/engine.
- **REQ-DEM-01 [Demo dataset] MUST.** A polished demo path with dummy data, aimed at CISOs and consultancies, customizable per CISO. Dummy data is not customer data.
- **REQ-DEM-02 [Wow without lying] MUST.** Demo may be impressive. It MUST NOT present prototype-README features as running code, or BYOC-local metrics as ShiftPQC-hosted telemetry.

## Quality

- **REQ-QA-01 [Tests] MUST.** Unit, integration, and end-to-end tests for CBOM, CBR, shift/hybrid/rollback, and the no-egress / no-data-plane properties.
- **REQ-QA-02 [Coverage] SHOULD.** High coverage on REQ-QA-01 paths. Not a 100% line-count MUST (OPEN-06).
- **REQ-QA-03 [Lean runtime] SHOULD.** Prefer fewer moving parts in the customer cloud. Runtime cost is a first-class constraint, not a post-hoc cleanup.

## Documentation (later; not this gate)

After 0.1.0 acceptance, living docs for:

- public and private APIs (developers)
- administration (sysadmin)
- marketing summaries that do not contradict the APIs

Documentation currency outranks new feature work when they conflict. Report the delay.

## Pushback (for review, not already decided)

1. The public prototype is a README. Building “the layout” as if the services exist is fake progress (VCS-06).
2. Fresh+Deno vs Angular is a fork in the first UI commit. Pick OPEN-01.
3. Investor metrics plus air-gap is a product contradiction until OPEN-05 is a design, not a wish.
4. Importing green-zkillz or swarm as the product would duplicate the CTO’s platform and miss ShiftPQC (REQ-GOV-05).
5. “Maximum viable” without REQ-GOV-01 becomes an unbounded rewrite. This 0.1.0 is the bound.
