# AI Plant Assistant – Problem Statement & Solution Description

Nov 14, 2026 · Loïc Ngueko

## Context

A global, multi-site process manufacturer (specialty chemicals, batch and continuous production) wants a conversational AI assistant for its plants. The assistant should sit on top of its manufacturing data and connect the shop floor, MES, ERP, logistics, quality and plant knowledge to the people who make decisions and place orders.

The company already runs a central data platform with a natural-language query layer. It has shown management a live demo of that layer on real plant data. The next step is to turn the vision into a product that can be rolled out to many production sites through the tools operators and control rooms already use.

## Problem statement

**Plant decisions are slow and fragmented because the data and knowledge needed to make them sit in separate systems and in people's heads.**

- **Customer commitments are hard to confirm quickly.** Answering "can we deliver in 3 weeks?" means checking stock, line capacity, staffing and material availability by hand, across systems and sometimes across plants.
- **Planning and the shop floor do not see the same status.** Production plans, inventory, inbound deliveries and actual plant performance are reconciled late, plant by plant.
- **Actual data is not used for decisions.** Lead times, cycle times and capacities are often planned values, not what the plant really achieves.
- **Knowledge is lost or hard to find.** Solutions to past problems live in shift logs, reports and the experience of retiring staff, and cannot be searched when a similar complaint or deviation comes up.
- **Deviations and escalations depend on individuals.** When a batch parameter goes out of spec, finding the right approver and documenting the correction is manual.
- **Optimization is reactive.** Recipe and process improvements are found after the fact, and the cost of alternatives (plant time, consumption, staffing) is not evaluated consistently.

The result is slower response to customer demand, local optimization instead of end-to-end flow, and decisions that are hard to trace.

## Target users

| User | Typical question or task | Decision they own |
| --- | --- | --- |
| Sales / customer service | Can we confirm this delivery date? | Customer commitment |
| Production planner | Do all plants have material for next week's plan? | Plan release, transfers between plants |
| Shift lead / operator | I'm one operator short: who can help, and what must they know? | Shift staffing, executing work steps |
| Process / recipe engineer | Is the optimized recipe within valid limits? | Recipe change release |
| Quality | Who must approve this out-of-spec correction? | Batch correction, release |
| Plant / site management | Does the cheaper alternative still hold with plant time, consumption and staffing? | Cost and route decisions |
| Logistics / warehouse | Stock at the filling line is running low | Material call-off, internal transport |

## Key use cases

Six capability areas, each with one scenario the solution must support end to end.

| # | Capability area | Scenario | What the assistant does | Capabilities needed |
| --- | --- | --- | --- | --- |
| 1 | Empower people, enable collaboration | A customer asks for delivery within 3 weeks | Checks stock and line capacity, finds a staffing gap in week 3, proposes an operator from another plant, prepares their work instructions and QC steps, routes open questions to the responsible role | Available-to-promise across ERP/MES, capacity and shift planning, skills matrix, generated work instructions |
| 2 | Create value beyond the plant | Packaging stock at a filling line is running low | Checks warehouse stock for the order, creates a logistics order, confirms when material is loaded on the automated transport | Inventory monitoring, order creation in ERP/WMS, intralogistics integration |
| 2 | (same) | Recipe improvement before production | Simulates the process on production data with a product twin and proposes an optimized recipe for review | Process/product digital twin, historical process data |
| 2 | (same) | Material readiness for next week | Checks stock and inbound deliveries against the production plan, plant by plant | Shared data products across sites, plan vs. inventory logic |
| 3 | Make knowledge accessible | A customer complaint needs root-cause input | Finds a former employee's shift log describing a similar case solved by an approved recipe change, with sources | Search over unstructured documents (shift logs, reports, SOPs) with citations |
| 4 | Prepare decisions, structure escalations | A batch parameter is out of spec | Shows the documented escalation path and required approvers, records the reviewed and approved correction | Spec monitoring, role-based approval workflow, audit trail |
| 5 | Optimize proactively, automate selectively | An adjusted recipe is proposed | Checks it against the valid recipe and plant limits; after human approval, sends the released version to the plant automatically | Recipe management integration, limit checks, controlled download to automation |
| 6 | Reduce cost, improve value creation | A cheaper production alternative is suggested | Evaluates plant time, consumption and staffing, with quality, safety and delivery as fixed constraints | Scenario and cost simulation |

## Solution concept and architecture

The solution is a plant AI assistant on top of one governed data foundation. It answers questions, prepares actions and routes decisions to the right person. Nothing reaches the plant without approval.

*Target architecture: 5 layers + governance.*

The process historian and MES supply the contextualized shop floor data that every use case above depends on. The governance rules apply to every layer, not only to the AI.

## Guiding principles and constraints

- **AI prepares, people decide.** The assistant proposes, checks and documents. Release decisions stay with the responsible role.
- **Automate selectively.** Actions only run automatically after approval and within defined limits, such as plant limits and the valid recipe.
- **Trust through real data.** Every answer is based on actual plant data and cites its sources. No answers from unverified data.
- **Embedded, not another tool.** The assistant appears in the tools users already work in (control room, MES, process data displays), so one feature can reach many sites at once.
- **Customer demand first.** The goal is end-to-end flow from order to delivery, not local cost tweaks.
- **Fixed constraints.** Quality, safety and delivery commitments are never traded off by an optimization.
- **Traceability.** Escalations, approvals and recipe changes are logged and auditable to regulated-industry standards.
- **One data foundation.** Planning, shop floor and management use the same governed data products across all sites.

## Success criteria

Targets are still open. The table lists the measures to agree on during discovery.

| Area | Measure | Baseline / target |
| --- | --- | --- |
| Customer response | Time to confirm a delivery date | To be defined |
| Delivery | On-time delivery rate | To be defined |
| Planning | Share of plan decisions based on actual (not planned) lead and cycle times | To be defined |
| Knowledge | Time to find relevant precedent for a complaint or deviation | To be defined |
| Quality | Time from out-of-spec event to approved, documented correction | To be defined |
| Process | Recipe changes simulated before production | To be defined |
| Adoption | Sites and active users using the assistant | To be defined |
| Trust | Share of answers with verifiable sources; user-rated accuracy | To be defined |

## Open questions and assumptions

- [ ] Which use case is the first pilot, and at which site?
- [ ] What is the target platform architecture? The customer's reference architecture document is still to be received.
- [ ] Which systems are in scope for the pilot: process historian, MES, ERP, WMS, LIMS, recipe management, document stores?
- [ ] Which actions may the assistant trigger (orders, recipe download), and which stay read-only?
- [ ] Which approval and audit rules apply (e.g. GMP-relevant processes)?
- [ ] Where must the assistant be embedded first: control room displays, MES, mobile, chat?
- [ ] How are users, roles and data access rights mapped across sites?
- [ ] What language(s) must the assistant support?

**Assumptions:** a central data platform with a natural-language query layer already exists; plant process data is available in a historian and can be contextualized; the customer's teams provide domain experts for each use case.

## Next steps

- Get the customer's reference architecture and map where process data, MES and ERP connect.
- Pick one or two pilot use cases with clear value and low automation risk (suggested: knowledge search over shift logs, and material readiness check).
- Run a data inventory for the pilot: sources, owners, quality, access.
- Define the guardrails: read-only vs. action, approval roles, audit logging.
- Build a thin prototype embedded in an existing operator tool, on real data from one site.
- Test with real users, measure against the success criteria, then plan the rollout to further sites.
