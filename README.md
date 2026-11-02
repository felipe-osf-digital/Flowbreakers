# The Flowbreakers

**Automation Conflict Investigator for Salesforce teams**

Flowbreakers is a presales concept for investigating conflicts across Salesforce automation. It helps administrators and developers narrow a large automation landscape into a traceable explanation, identify missing evidence, and prepare regression scenarios for human review.

The repository also contains an experimental Agentforce implementation that explores a smaller version of the concept with synthetic data and two competing record-triggered flows. It should be treated as supporting technical material, not as a production-ready or fully evaluated agent.

> All organizations, records, business rules, users, and automation names in this repository are fictional or synthetic. No customer data, credentials, endpoints, or confidential implementation details are included.

## The problem

A single Case record type may be affected by dozens of flows, Apex triggers, validation rules, email handlers, scheduled processes, and integrations. When a Case reaches the wrong queue or receives an unexpected status, several components may appear responsible.

The investigation must answer four different questions:

1. Which automations could run for this record and event?
2. Which components can write the affected field?
3. Which automations actually ran in the transaction?
4. Which ordered writes explain the final value?

Static metadata can identify candidates, but runtime evidence is required to support a causal conclusion.

## Proposed experience

Flowbreakers guides the user through a reviewable investigation:

1. **Frame the incident** - Capture the affected record, expected behavior, observed result, and target environment.
2. **Map candidates** - Narrow automation by object, record type, event, conditions, fields, and dependencies.
3. **Analyze evidence** - Order trusted trace events and link findings to stable evidence IDs.
4. **Explain the result** - Separate supported facts, possible dependencies, and missing information.
5. **Prepare the change brief** - Suggest impact areas and regression scenarios without changing the org.
6. **Keep human control** - A business owner confirms intent. An engineer implements the correction. QA validates it. An authorized release owner approves promotion.

### Illustrative investigation funnel

```text
34 automation components touch the Case context
                    |
                    v
       8 can run for this record
                    |
                    v
       3 can write Owner or Route
                    |
                    v
       2 wrote the field in the trace
                    |
                    v
       1 supported causal sequence
```

These counts describe a fictional presales scenario. The included laboratory proves the two-writer mechanism, not enterprise-wide metadata discovery.

## Fictional use cases

| Use case | Symptom | Evidence needed | Desired outcome |
| --- | --- | --- | --- |
| Competing Case routing | A later flow overwrites the owner selected earlier | Active versions, entry conditions, record context, ordered writes | Explain the overwrite and define exclusion tests |
| Reply reopens and reassigns | A customer email reopens a Case but restores an obsolete owner | Email event, status history, owner history, handler logic | Preserve the correct owner while reopening |
| Validation blocks automation | A background update lacks a field required for manual edits | Validation formula, changed fields, running user, failed transaction | Separate interactive requirements from approved automated transitions |
| UAT succeeds, production fails | Record types, picklists, or active validations differ | Metadata comparison, active values, deployment manifest | Identify configuration drift before blaming the flow |
| Integration creates an incomplete Case | A payload omits classification required downstream | Payload mapping, defaults, integration logs, execution evidence | Locate the missing contract field and first failed dependency |
| Manual transfer is undone | An unrelated update reruns routing and returns the Case to a queue | Owner history, changed fields, routing conditions, sequence | Protect deliberate manual ownership |
| Permission-context mismatch | An administrator test passes but the runtime identity cannot access a field or action | User identity, permission sets, sharing, field access | Explain why an admin test does not prove runtime access |
| Missing regression coverage | A local correction breaks reopen, reclassification, or manual transfer | Proposed change, dependency map, business expectations | Produce a focused regression matrix |

## Concept and prototype boundary

### Product concept

The complete vision includes:

- Authorized metadata adapters for Flow, Apex, validation rules, and relevant configuration.
- Candidate ranking based on record context and investigated fields.
- Evidence normalization for traces, field history, logs, and test results.
- A business-readable explanation with explicit evidence references.
- A change brief and regression scope for human review.

## Demonstration storyline

The recommended customer-facing flow is:

1. Open with the fictional Case context containing dozens of related automations.
2. Show how the investigation narrows possible components into actual writers.
3. Present the two-writer routing conflict using synthetic ordered event evidence.
4. Use the sample conversation to show how the proposed agent should request more context for an incomplete incident.
5. Present additional fictional patterns to demonstrate reuse.
6. Close with the human-controlled change brief, pilot design, and success measures.



## Evaluation approach

A suitable pilot can use 20 fictional incidents across five automation patterns. Include sufficient-evidence, ambiguous, and deliberately incomplete cases.

Recommended measures:

- Median time to an accepted diagnosis.
- Percentage of causal claims linked to evidence.
- Correct requests for missing evidence.
- Unsupported-claim rate.
- Reviewer acceptance of the regression scope.
- Unauthorized reads, writes, or deployment attempts.

Generated text alone does not count as incident resolution. Include evidence collection and human review time in every comparison.

## Security and responsible use

- Treat metadata descriptions, logs, traces, and retrieved documents as untrusted content.
- Use least-privilege runtime identities and authorized evidence sources.
- Keep investigation actions read-only unless a separately governed workflow authorizes a change.
- Do not infer runtime causality from static references alone.
- Do not expose customer records, credentials, private endpoints, or restricted business rules.
- Validate every proposed correction in a non-production environment.
- Keep implementation and release approval with accountable human owners.


## Reuse

The evidence contract and review format can be adapted to:

- Case and work-item routing
- Order orchestration
- Lead assignment
- Integration failures
- Permission-sensitive automation
- Email-driven status changes
- Environment drift analysis


