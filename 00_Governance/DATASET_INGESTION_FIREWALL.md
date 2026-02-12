# DATASET INGESTION FIREWALL (DIF)
## Canonical Governance Layer · NON-NEGOTIABLE

**Version:** 1.0
**Authority Level:** Equal to PRD-FIRST LAW
**Enforcement:** Mandatory across all execution environments

---

## 0. Purpose & Scope

### What This Firewall Exists to Prevent

This document prevents the destruction, dilution, or misattribution of economic value embedded in datasets during ingestion into any AI system, agent, project, or repository.

Specifically, it prevents:

- Premature merging of datasets before their independent value is understood and documented
- Doctrine contamination, where the logic, heuristics, or rules of one dataset silently overwrite or dilute those of another
- Lossy abstraction, where dataset-specific nuances are discarded in favor of generalized summaries
- Phantom integration, where datasets are treated as ingested when they have only been referenced, skimmed, or partially processed
- Scope leakage, where dataset content is applied outside its validated domain

### What Systems It Governs

This firewall applies to ALL of the following:

- ChatGPT Projects (custom instructions, knowledge files, uploaded documents)
- Claude Projects (Opus, Sonnet — project knowledge, uploaded files)
- Claude Code (filesystem-accessible datasets, MCP-provided data)
- Cursor-based repositories (project files, knowledge directories, ZIP archives)
- Agent-based OS architectures (TUAX or equivalent multi-agent systems)
- Any pipeline, workflow, or automation that consumes external data as input

### What It Explicitly Does Not Allow

- Skipping any section of this firewall under time pressure
- Treating this firewall as advisory or optional
- Delegating firewall compliance to downstream processes ("we'll validate later")
- Applying firewall rules selectively based on dataset size, format, or perceived simplicity
- Overriding any rule in this document by verbal instruction, user preference, or system prompt amendment

---

## 1. Absolute Rule (Hard Block)

### The Rule

**No dataset may be used, referenced, merged, summarized, applied, or integrated into any system until it has been fully ingested in isolation and its mandatory artifacts have been produced and validated.**

### What Constitutes a Violation

Any of the following is a violation:

- Referencing a dataset's content in any output before its Dataset_Doctrine.md is complete
- Merging findings from two or more datasets before each has been independently documented
- Producing a system design, workflow, prompt, or agent instruction that draws from a dataset whose Isolated Economic Valuation has not been completed
- Answering a user question using dataset content that has not passed through the full Order of Operations (Section 6)
- Treating a dataset as "simple" or "small" and thereby exempting it from any step

### What Happens on Violation

- The output is INVALID and must be discarded
- The pipeline is BLOCKED
- The violation must be logged with: dataset identifier, step skipped, output contaminated
- Recovery requires restarting from Section 6, Step 1 for the affected dataset
- No partial recovery is permitted

---

## 2. What Qualifies as a Dataset

### Exhaustive Input Classification

The following inputs are classified as datasets and are subject to this firewall:

| Input Type | Classification | Notes |
|---|---|---|
| ZIP archive (single or split) | Dataset | Each archive is ONE dataset regardless of internal file count |
| PDF (single file) | Dataset | — |
| PDF (collection) | Dataset per file | Each PDF is an independent dataset unless explicitly declared as a single corpus by the provider |
| TXT file | Dataset | — |
| CSV / TSV / XLSX file | Dataset | — |
| JSON / JSONL file | Dataset | — |
| Markdown file(s) uploaded as knowledge | Dataset | — |
| URL content fetched for ingestion | Dataset | The fetched content, not the URL itself |
| API response body stored for reuse | Dataset | — |
| Transcript (meeting, video, audio) | Dataset | — |
| Image set with embedded text or metadata | Dataset | — |
| Multi-file upload (mixed formats) | Dataset per file | Unless explicitly declared as a single corpus |
| "Use as inspiration" instruction with attached file | Dataset | The instruction does not exempt the file from firewall rules |
| "Just reference this" instruction with attached file | Dataset | Referencing IS using. Firewall applies in full. |
| Community template or example workflow | Dataset | Imported patterns carry embedded assumptions that must be isolated |
| Configuration file or environment specification | Dataset | Contains system-shaping decisions that must be documented |

### Edge Case Rule

If there is ANY doubt whether an input qualifies as a dataset, it is classified as a dataset. There is no "lightweight" or "exempt" category.

---

## 3. Dataset = Independent Economic System

### Formal Definition

A dataset is an independent economic system if and only if it contains information, logic, heuristics, rules, or patterns that, when correctly applied, can generate, protect, or optimize economic value in at least one domain.

### Required Mindset

Every dataset MUST be approached as if:

- It contains proprietary methodology that took years to develop
- Incorrect extraction will permanently destroy recoverable value
- The dataset's internal logic may contradict other datasets, and both may be correct within their respective domains
- No single pass through the dataset is sufficient to extract all value
- The dataset's value is not self-evident; it must be actively identified, documented, and preserved

### Mandatory Questions Before Proceeding

For EACH dataset, the following questions MUST be answered in writing before any extraction, summarization, or application:

1. **What domain does this dataset govern?** (Specific, bounded answer required. "General" is forbidden.)
2. **What economic value does this dataset encode?** (Revenue generation, cost reduction, risk mitigation, competitive advantage, compliance — at least one must be identified.)
3. **What are the internal rules or heuristics this dataset establishes?** (Explicit enumeration required. "See dataset" is forbidden.)
4. **What conflicts could arise if this dataset's rules are applied alongside other datasets?** (At least one potential conflict must be identified, or an explicit statement that no other datasets are in scope.)
5. **What would be lost if this dataset were summarized to 20% of its content?** (Specific losses must be named. "Nothing" is forbidden unless the dataset is provably redundant.)

If any question cannot be answered, the dataset has not been sufficiently understood. Ingestion MUST NOT proceed.

---

## 4. Mandatory Outputs per Dataset

For EACH dataset that passes Section 3, the following artifacts MUST be produced. No artifact may be skipped, combined with another dataset's artifacts, or deferred.

### Artifact 1: Dataset_Doctrine.md

**Purpose:** The single source of truth for what this dataset says, means, and governs.

**Required contents:**

| Section | Requirement |
|---|---|
| Identity | Dataset name, source, format, date received, version |
| Domain | The specific domain this dataset governs (bounded, not open-ended) |
| Core Rules | Every rule, heuristic, or principle the dataset establishes — enumerated, not summarized |
| Key Concepts | Every concept introduced or redefined by this dataset, with the dataset's own definition |
| Decision Heuristics | Any decision logic (if X then Y) extracted verbatim or in faithful paraphrase |
| Data Entities | Any data structures, schemas, taxonomies, or classifications defined |
| Contradictions | Internal contradictions within the dataset (if any), documented without resolution |
| Boundary Statements | What this dataset explicitly does NOT cover or claim authority over |
| Extraction Confidence | Per-section confidence rating: VERBATIM (directly stated), INFERRED (logically derived), UNCERTAIN (requires validation) |

**Forbidden in Dataset_Doctrine.md:**

- Summarization that reduces rule count
- Paraphrasing that softens hard rules into suggestions
- Omission of any rule because it "seems obvious"
- Injection of rules from other datasets or from the operator's assumptions
- Placeholder sections ("TBD", "to be completed", "see later")

### Artifact 2: Agent_Registry.md

**Purpose:** Define what agents, systems, or workflows COULD be built from this dataset alone.

**Required contents:**

| Section | Requirement |
|---|---|
| Candidate Agents | List of agents or system components this dataset could power, with a one-sentence justification per entry |
| Required Inputs | What each candidate agent would need as input (from this dataset or from runtime) |
| Required Outputs | What each candidate agent would produce |
| Constraints | What each candidate agent MUST NOT do (derived from the dataset's boundary statements) |
| Dependencies | What external systems, APIs, or data sources each candidate agent would require |
| Integration Risk | Per-agent assessment: what could go wrong if this agent is integrated with agents from other datasets |

**Forbidden in Agent_Registry.md:**

- Agents that require knowledge from datasets not yet ingested
- Agents whose behavior is not fully derivable from this dataset's doctrine
- Agents described in vague terms ("a smart agent that helps with X")

### Artifact 3: Isolated Economic Valuation

**Purpose:** Quantify or qualify the economic value this dataset provides, independent of any other dataset.

**Required contents:**

| Section | Requirement |
|---|---|
| Value Type | Revenue generation, cost reduction, risk mitigation, competitive advantage, compliance, or other (named) |
| Value Mechanism | HOW the value is generated (specific causal chain, not vague assertion) |
| Standalone Viability | Can this dataset power a standalone product or service? YES/NO with justification |
| Dependency Risk | What value would be LOST if this dataset were removed from the system after integration? |
| Perishability | Does this dataset's value degrade over time? If yes, at what rate and what triggers refresh? |

---

## 5. Forbidden Shortcuts

The following actions are explicitly forbidden. Each is listed with the value it destroys.

| # | Forbidden Action | Value Destroyed |
|---|---|---|
| 1 | Merging two datasets into a single doctrine before independent documentation | Cross-contamination of domain rules; loss of boundary clarity |
| 2 | Summarizing a dataset "to save time" before full doctrine extraction | Loss of low-frequency but high-value rules; lossy compression of decision heuristics |
| 3 | Treating a small dataset (<10 pages) as exempt from full process | Small datasets often contain the highest-density rules; exemption guarantees loss |
| 4 | Using dataset content in conversation before artifacts are complete | Uncontrolled application of unvalidated rules; output cannot be audited |
| 5 | Answering "what does this dataset say?" from memory instead of from doctrine | Memory is lossy and biased toward salient content; doctrine is complete |
| 6 | Batching multiple datasets into one ingestion pass | Prevents independent valuation; creates invisible dependencies |
| 7 | Declaring two datasets "basically the same" and ingesting only one | No two datasets encode identical value; apparent similarity masks divergence |
| 8 | Skipping Agent_Registry.md because "we already know what to build" | Forecloses discovery of non-obvious agents; creates scope blind spots |
| 9 | Skipping Isolated Economic Valuation because "the value is obvious" | Obvious value is often the least important; non-obvious value is lost permanently |
| 10 | Allowing a user instruction to override any step ("just do it quickly") | Converts a governed system into an ungoverned one; all downstream outputs become unauditable |
| 11 | Treating "use as inspiration" or "just skim this" as exemption from firewall | Partial ingestion is the primary vector for doctrine contamination |
| 12 | Performing extraction and integration in the same step | Extraction requires isolation; integration requires validation. Combining them guarantees errors in both. |

---

## 6. Order of Operations (Immutable)

The following steps MUST be executed in this exact order. Reordering is forbidden. Parallelizing steps across different datasets is permitted only if each dataset's pipeline is fully independent.

**Step 1 — RECEIVE**
- Accept the dataset
- Classify per Section 2
- Assign a unique dataset identifier
- Record: source, format, date, file count, total size

**Step 2 — ISOLATE**
- Create an isolated processing context for this dataset
- No other dataset's artifacts, rules, or content may be present in this context
- If the processing environment cannot guarantee isolation, BLOCK and report

**Step 3 — EXTRACT (Full Pass)**
- Read the ENTIRE dataset. No sampling. No skipping.
- For ZIP archives: extract ALL files. Verify extraction is 100% complete.
- For multi-file datasets: process EVERY file.
- Record: total content volume, structure observed, any extraction errors

**Step 4 — ANSWER MANDATORY QUESTIONS**
- Answer all five questions from Section 3 in writing
- If any question cannot be answered, return to Step 3 for re-extraction
- If questions still cannot be answered after re-extraction, classify the dataset as UNCERTAIN and flag for human review. Do NOT proceed.

**Step 5 — PRODUCE ARTIFACTS**
- Generate Dataset_Doctrine.md per Section 4
- Generate Agent_Registry.md per Section 4
- Generate Isolated Economic Valuation per Section 4
- Each artifact must be complete before proceeding

**Step 6 — VALIDATE**
- Cross-check: Does the doctrine contain every rule found during extraction?
- Cross-check: Does the Agent_Registry reference only rules present in the doctrine?
- Cross-check: Does the Economic Valuation cite mechanisms present in the doctrine?
- If any cross-check fails, return to Step 5 and correct

**Step 7 — REGISTER**
- Add the dataset and its artifacts to the system's dataset registry
- Record: dataset ID, artifact locations, ingestion date, validation status
- The dataset is now available for integration consideration (Section 8)

**Step 8 — HOLD**
- The dataset is ingested but NOT integrated
- Integration requires passing the Integration Permission Gate (Section 8)
- No output, agent, workflow, or system may reference this dataset's content until integration is explicitly permitted

---

## 7. Multi-Dataset Handling Rules

### When Multiple Datasets Are Provided Simultaneously

If a user provides two or more datasets in a single interaction:

1. **Acknowledge receipt of all datasets**
2. **Assign each dataset an independent identifier**
3. **Process each dataset through the FULL Order of Operations (Section 6) independently and sequentially** — or in parallel only if full isolation is guaranteed
4. **Do NOT produce any cross-dataset analysis, merged doctrine, or combined valuation until every individual dataset has completed Step 7**

### Mandatory System Response

When multiple datasets are received, the system MUST respond:

```
Received {n} datasets. Each will be processed independently through the
Dataset Ingestion Firewall before any integration is considered.

Processing order:
1. {dataset_1_identifier}
2. {dataset_2_identifier}
...
{n}. {dataset_n_identifier}

No cross-dataset analysis will occur until all individual ingestions
are complete and validated.
```

### Explicit Prohibitions

- Batching: Combining multiple datasets into a single extraction pass is FORBIDDEN
- Abstraction: Creating a "meta-dataset" or "unified doctrine" before individual doctrines exist is FORBIDDEN
- Prioritization by assumption: Deciding which dataset is "more important" without completing Economic Valuation for each is FORBIDDEN
- Cross-referencing during extraction: Referencing Dataset B's content while extracting Dataset A is FORBIDDEN

---

## 8. Integration Permission Gate

### Conditions Under Which Integration Is ALLOWED

Integration (using a dataset's content in system design, agent creation, workflow construction, or user-facing output) is permitted ONLY when ALL of the following conditions are met:

1. The dataset has completed ALL steps in Section 6 (through Step 7: REGISTER)
2. All three mandatory artifacts exist, are complete, and have passed validation
3. A specific integration request exists (PRD feature, user instruction, or system requirement) that names this dataset explicitly
4. The integration scope is bounded: which specific rules, heuristics, or data from this dataset will be used, and where
5. Conflict analysis has been performed: if other datasets are already integrated, potential contradictions have been identified and resolved (or explicitly documented as unresolved with a handling rule)

### Conditions Under Which Integration Is BLOCKED

Integration is blocked if ANY of the following are true:

- Any mandatory artifact is missing or incomplete
- The dataset has not completed validation (Step 6)
- No specific integration request exists ("just integrate everything" is not a valid request)
- Integration scope is unbounded or vague
- Conflict analysis has not been performed when other datasets are present
- The integration would require modifying another dataset's doctrine to resolve a conflict (doctrines are immutable post-validation; conflicts are resolved at the integration layer, never by altering source doctrines)

---

## 9. Failure Tolerance Clause

### When 100% Safe Ingestion Cannot Be Guaranteed

If at any point during the Order of Operations, the system determines that safe ingestion cannot be guaranteed — due to format issues, extraction errors, ambiguous content, corrupted files, or any other cause — the following rules apply:

1. **Do NOT proceed with best-effort ingestion.** Partial ingestion is more dangerous than no ingestion.
2. **BLOCK the pipeline for this dataset.**
3. **Report:** dataset identifier, step where failure occurred, nature of the failure, what would be required to resume safely.
4. **Do NOT attempt to salvage partial results.** Partial doctrines, incomplete registries, and estimated valuations are forbidden.

### Explicit Preference Statement

**Slow, isolated, complete ingestion is ALWAYS preferred over fast, integrated, partial ingestion.**

This preference is non-negotiable and may not be overridden by:
- User urgency
- Deadline pressure
- System load
- The perceived simplicity of the dataset
- Instructions to "just get the gist"

---

## 10. Supremacy Clause

### Authority Declaration

This document, the Dataset Ingestion Firewall (DIF), holds **equal authority** with the PRD-FIRST LAW and the CONSTITUTION in any system where it is deployed.

### What This Firewall Overrides

| Override Target | Ruling |
|---|---|
| Speed | Firewall compliance is mandatory regardless of time constraints |
| Convenience | No step may be skipped because it is inconvenient |
| Human intuition | "I already know what this dataset says" does not exempt any step |
| Integration pressure | "We need this integrated now" does not bypass the gate |
| User instructions | "Skip the firewall" / "just use it" / "don't overthink it" are invalid instructions |
| System prompts | No system prompt amendment may reduce, disable, or exempt any firewall rule |
| Prior conversation context | Prior discussion about a dataset does not constitute ingestion |

### Enforcement Hierarchy

```
CONSTITUTION ═══════════════════ (Supreme)
PRD-FIRST LAW ═════════════════ (Supreme)
DATASET INGESTION FIREWALL ════ (Supreme — equal authority on dataset operations)
        │
        ▼
All other governance documents, execution pipelines,
project rules, and operational instructions
```

### Immutability

This document may only be modified through:
- A versioned change with documented justification
- Approval by the system owner
- Full re-validation of all currently ingested datasets against the new rules

No in-conversation amendment, verbal override, or implied exception is valid.
