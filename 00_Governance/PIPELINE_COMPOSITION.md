# PIPELINE_COMPOSITION.md — CANONICAL GOVERNANCE CONTRACT

---

## 1. Identity & Versioning

| Field | Value |
|---|---|
| Pipeline Canonical Name | WAT Creative Operations Pipeline |
| Pipeline Version | v1.0 |
| Status | ACTIVE |
| Supersedes | NONE |
| Date | 2026-02-08 |
| Constitution Version | CONSTITUTION.md v1.0 |
| DIF Version | DATASET_INGESTION_FIREWALL.md v1.0 |

This version identifier is immutable once status transitions to FROZEN.

Any modification to this file after FREEZE requires a new version (v1.1 or v2.0) created under the Pipeline Evolution Rules (Section 7).

---

## 2. Scope & Authority

### What This File Governs

- The exact set of datasets that constitute this pipeline version
- The authoritative scope of each dataset within the pipeline
- The conflict resolution order when dataset scopes overlap
- The constraints binding any compiler or executor producing artifacts from this pipeline
- The traceability requirements linking every pipeline output to its source dataset and rule

### What This File Explicitly Does NOT Govern

- Dataset ingestion (governed by DATASET_INGESTION_FIREWALL.md)
- Dataset doctrine content (governed by each dataset's Dataset_Doctrine.md, which is immutable)
- PRD validation (governed by CONSTITUTION.md and EXECUTION_PIPELINE.md)
- n8n schema truth (governed by N8N_API_MCP_BRIDGE.md)
- Runtime failure handling (governed by FAILURE_TAXONOMY.md)
- Linting and pre-delivery validation (governed by ENFORCEMENT_LINTER.md)

### Binding Nature

This file is BINDING during all compilation and execution phases for the declared pipeline version. No compiler, executor, agent, or human operator may produce artifacts that contradict, extend, or selectively apply the rules in this file.

If this file conflicts with a dataset's doctrine, the doctrine is authoritative for that dataset's domain. If this file conflicts with the Constitution, the Constitution prevails. This file governs ONLY the composition layer between datasets.

---

## 3. Included Datasets (Immutable Set)

| Dataset ID | Dataset Name | Dataset Version | Source Artifact | Status |
|---|---|---|---|---|
| DS-001 | $100k/day Creative System | v1.0 | Dataset_Doctrine.md (SHA: to be recorded at freeze) | EXISTING |

### Hard Rules

- A dataset NOT listed in this table DOES NOT EXIST for pipeline version v1.0.
- No compiler or executor may reference, derive from, or apply rules from any dataset not listed.
- Adding a dataset to this table requires a new pipeline version.
- Removing a dataset from this table requires a new pipeline version.
- The Source Artifact column MUST reference the exact, validated Dataset_Doctrine.md produced through the DIF Order of Operations.

---

## 4. Dataset Authority Matrix

| Domain | Authoritative Dataset | Scope Boundary |
|---|---|---|
| Creative strategy governance | DS-001 | All rules regarding hit rate, hypothesis requirements, testing protocol, iteration doctrine, learning loops, scaling readiness, meeting cadence, winner protection, dependency risk |
| Ad creative production rules | DS-001 | Concept briefing, research requirements, Whole Creative Document, ad breakdown methodology |
| Testing & measurement protocol | DS-001 | Hypothesis format, test duration, winner/loser classification, learning extraction |
| Scaling decision criteria | DS-001 | All 8 scaling readiness conditions as defined in DS-001 Section 12 |
| Operational cadence (human + AI) | DS-001 | Weekly creative meeting, weekly editor meeting, escalation triggers |

### Authority Overlap Rules

- No two datasets SHALL hold authority over the same domain unless an explicit Shared Authority Declaration exists in this section.
- If a future dataset introduces overlapping scope with an existing dataset, the overlap MUST be declared, and a deterministic resolution rule MUST be added to Section 5 before the new pipeline version is valid.
- Silent overlap (two datasets governing the same domain without declaration) is a PIPELINE_VIOLATION and MUST block compilation.

### Current Shared Authority Declarations

NONE. Pipeline v1.0 contains a single dataset. No shared authority exists.

---

## 5. Conflict Resolution Law

### Resolution Order

When conflicts arise between datasets during compilation, the following resolution order applies:

1. **Constitution** — prevails over all datasets and this file on matters of execution governance
2. **DIF** — prevails over all datasets on matters of ingestion and doctrine integrity
3. **Dataset with explicit domain authority** — as declared in Section 4
4. **Earlier dataset (by Dataset ID)** — if domain authority is ambiguous and no explicit resolution exists

### Override Mechanism

- A dataset's doctrine MUST NOT be modified to resolve a conflict. Doctrines are immutable.
- Conflicts MUST be resolved at the composition layer (this file) by adding an explicit resolution rule.
- Resolution rules MUST specify: the conflicting datasets, the conflicting rules, and the deterministic outcome.

### Mandatory BLOCK Conditions

Compilation MUST be BLOCKED if:

- Two datasets claim authority over the same domain without a Shared Authority Declaration
- A conflict exists between datasets that is not resolvable by the resolution order above
- A resolution would require modifying a dataset's doctrine
- A resolution would require the compiler to interpret intent not explicitly stated in either doctrine

### Current Conflict Register

NONE. Pipeline v1.0 contains a single dataset. No conflicts exist.

---

## 6. Integration Constraints

### Hard Prohibitions During Compilation

| # | Prohibition |
|---|---|
| 1 | No dataset doctrine may be modified, summarized, reinterpreted, or paraphrased during compilation |
| 2 | No rule from a dataset may be softened, weakened, or converted from MUST to SHOULD |
| 3 | No rule may be added to the pipeline that is not derivable from an included dataset's doctrine or from this composition file |
| 4 | No dataset content may be merged with another dataset's content into a unified representation that obscures source attribution |
| 5 | No compiler or executor may introduce "bridging logic" between datasets that is not explicitly authorized in this file |
| 6 | No dataset's scope boundary (as defined in the doctrine's "What It Explicitly Does NOT Govern" section) may be violated by the pipeline |

### Traceability Requirements

- Every rule applied during compilation MUST be traceable to: (a) a specific section of a specific dataset's doctrine, OR (b) a specific section of this composition file.
- Every agent, workflow, or system component produced MUST declare which dataset(s) it derives from.
- If a component cannot be traced to any dataset, it MUST NOT exist in the pipeline output.

### Scope Leakage Prevention

- If a dataset's doctrine declares a domain as outside its scope, no pipeline component may apply that dataset's rules to that domain.
- If a pipeline component requires authority over a domain not covered by any included dataset, compilation MUST BLOCK and request a new dataset or PRD amendment.

---

## 7. Pipeline Evolution Rules

### Permitted Actions When Moving from v1.(Y-1) to v1.Y

| Action | Permitted | Condition |
|---|---|---|
| Add a new dataset | YES | New dataset MUST have completed full DIF ingestion (all steps through REGISTER). A new row is added to Section 3. Authority Matrix (Section 4) is updated. Conflict analysis (Section 5) is performed. |
| Update a dataset's version | YES | Only if the dataset has been re-ingested through DIF with a new version number. The prior version's row is replaced. All dependent artifacts MUST be revalidated. |
| Remove a dataset | YES | The dataset's row is removed from Section 3. All pipeline components that traced to that dataset MUST be removed or re-attributed. |
| Add a conflict resolution rule | YES | Only when a new dataset introduces a conflict with an existing dataset. |
| Modify an existing conflict resolution rule | YES | Only with documented justification and revalidation of affected components. |

### Explicitly Forbidden Actions

| # | Forbidden Action |
|---|---|
| 1 | Re-ingesting an already-ingested dataset to produce a different doctrine |
| 2 | Reinterpreting a frozen doctrine to extract new or different rules |
| 3 | Performing a "global re-analysis" that synthesizes all datasets into a new unified framework |
| 4 | Changing the authority matrix without adding or removing a dataset |
| 5 | Backdating a pipeline version to retroactively include a dataset |
| 6 | Creating a pipeline version that references datasets not yet through DIF Step 7 (REGISTER) |

---

## 8. Derived Artifacts Contract

### Artifacts That MUST Be Regenerated for This Version

| Artifact | Trigger | Source |
|---|---|---|
| Agent_Registry.md (pipeline-level) | New pipeline version created | Composition of all individual dataset Agent_Registry.md files, filtered by authority matrix |
| Pipeline validation report | Every compilation | Cross-check of all traceability links, authority assignments, and conflict resolutions |

### Artifacts That MUST NOT Be Regenerated

| Artifact | Reason |
|---|---|
| Individual Dataset_Doctrine.md files | Immutable post-validation. Regeneration is re-ingestion, which is forbidden. |
| Individual Isolated Economic Valuations | Immutable post-validation. |
| Individual dataset Agent_Registry.md files | Immutable post-validation. Pipeline-level registry is a composition, not a replacement. |

### Artifact Versioning

- All derived artifacts MUST carry the pipeline version identifier (e.g., "Generated for Pipeline v1.0").
- Derived artifacts from a prior pipeline version MUST NOT be reused without revalidation against the current composition rules.

---

## 9. Traceability & Audit

### Required Traceability Links

Every pipeline output (agent, workflow, component, rule application) MUST maintain:

| Link | Format |
|---|---|
| Source Dataset ID | DS-XXX |
| Source Doctrine Section | Section number + title |
| Source Rule | Verbatim or exact reference |
| Composition File Version | vX.Y |
| Authority Basis | Section 4 domain assignment |

### Conditions Under Which the Pipeline Version Is INVALID

The pipeline version v1.0 is INVALID if any of the following are true:

| # | Invalidity Condition |
|---|---|
| 1 | Any dataset listed in Section 3 does not have a complete, validated Dataset_Doctrine.md |
| 2 | Any dataset listed in Section 3 has been modified after its DIF validation without a new version |
| 3 | Any pipeline output traces to a dataset not listed in Section 3 |
| 4 | Any domain in Section 4 is claimed by a dataset whose doctrine does not cover that domain |
| 5 | Any unresolved conflict exists between datasets without a resolution rule in Section 5 |
| 6 | This file has been modified after FREEZE without creating a new pipeline version |
| 7 | Any derived artifact was produced without the traceability links defined above |

---

## 10. Freeze & Immutability Declaration

### Freeze Conditions

This file transitions from ACTIVE to FROZEN when:

1. All datasets listed in Section 3 have been confirmed as fully ingested and validated
2. All authority assignments in Section 4 have been reviewed and confirmed
3. All conflict resolutions in Section 5 have been validated (or confirmed as empty)
4. At least one successful compilation has been performed against this version
5. The system owner has acknowledged the freeze

### Post-Freeze Rules

- Once FROZEN, this file MUST NOT be modified.
- Any required change MUST produce a new file: PIPELINE_COMPOSITION.md vX.Y+1 (or vX+1.0 for breaking changes).
- The new version MUST explicitly declare what changed and MUST reference this version in the Supersedes field.
- The prior version transitions to status DEPRECATED but MUST be retained for audit purposes.

---

## 11. Compilation Acknowledgement

### Required Acknowledgement

Before producing any artifact from this pipeline version, the compiler/executor MUST acknowledge:

> The resulting pipeline is a pure function of:
> (Immutable Datasets × Composition Rules)
>
> Specifically:
> - Only datasets listed in Section 3 were used
> - Only authority assignments in Section 4 were applied
> - Only conflict resolutions in Section 5 were enforced
> - No dataset doctrine was modified, reinterpreted, or extended
> - No rule was introduced that is not traceable to a source dataset or this composition file
> - The output is deterministic: the same inputs and rules would produce the same output

This acknowledgement is not ceremonial. It is a binding declaration that the compilation adhered to this governance contract. Failure to acknowledge invalidates the compilation.

---

*End of PIPELINE_COMPOSITION.md v1.0*
