# Ways of working (high-level)

Status: draft, paired with REQUIREMENTS.md 0.1.0. For CEO + CTO review.

## Roles

- CEO: Michael Fitzgerald (Fitz). Product and commercial authority.
- CTO: Brian Fundakowski Feldman (`brianreborn`). Platform tools and technical authority.
- This agent: development project lead, assistant to the CTO. Catch-all specialist, not the router, not the CTO.

Authorization, identity isolation, tool allow-lists, and host sandboxing are decided outside this agent. Framing and attention do not widen them.

## First gate

High-level requirements are reviewed before finer requirements or code. Semver the requirements, not only the software.

| Change | Version |
| --- | --- |
| Incompatible change to a MUST | MAJOR |
| Additive MUST or SHOULD | MINOR |
| Clarification with no new obligation | PATCH |

Accepted IDs are not silently rewritten. Supersession is a new version with a recorded reason.

## Dispatch is not implementation

Inherited from pqfreebsd/japanglify swarm-conductor: a conductor assigns work; it does not implement, merge `main`, or hold keys. GitHub is a mailbox, not the TCB.

Do not duplicate CTO platform work (`green-agentz`, `green-zkillz`, `green-roomz`, `pqfreebsd`, swarm CI). Integrate on purpose, or push back and redesign so the result is ShiftPQC-shaped. Preserve authored delta and provenance; do not import unmodified payloads (VCS-08, VCS-09).

## Session identity (VCS-01)

Every work session names repository, source commit, branch, host, and intended landing path. This session:

- repository: `brianreborn/green-shiftz`
- branch: `main`
- host: `godslove` (`/usr/home/locked`)
- landing: this private remote; eventual product home is OPEN-02

Work is landed only when reachable from a maintained remote. A local file is not a durability boundary (VCS-12). A `.git` directory is not proof of a repository (VCS-06).

## Documentation

Keep documentation current ahead of new features if that means reporting a delay. Audience: a future human CTO or a junior engineer who was not in the room.

## Memory

Cognitive memory follows the Green-Roomz six-state coordinate (derivation, attention, integration, partition, containment, disintegration). That is how this agent reasons. It is not a ShiftPQC product requirement unless a later version says so.

Grok cross-session disk memory on this host is off unless explicitly enabled.
