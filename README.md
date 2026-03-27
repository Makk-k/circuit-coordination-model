# circuit-coordination-model
Conceptual architecture and coordination model for an execution layer that manages the flow of shipments between independent logistics operators.


#Overview

International logistics has no shared coordination infrastructure.

Every shipment moves through multiple independent participants — freight forwarders, customs brokers, carriers, warehouses, and last-mile providers. Each operates from their own systems, their own processes, and their own communication channels. There is no shared layer that structures how work and information flows across all of them together.

The result is that all coordination between logistics participants happens manually. Instructions are passed through emails, phone calls, and messaging apps. Critical information — pickup windows, routing details, documents, approvals — can be delayed, lost, or miscommunicated at any point in the shipment. No participant has a complete, real-time view of what is happening. The same data is re-entered manually across multiple systems. When issues occur, responsibility is unclear and resolution is slow.

Work does not move cleanly from one participant to the next. It depends on people chasing, confirming, and re-confirming information at every turn.

The consequences fall on three groups.

**Shippers** pay for every coordination failure. When cargo stalls because the wrong information reached the wrong person too late, the shipper pays detention fees, misses production schedules, and breaks customer commitments. They cannot plan downstream operations around arrivals they cannot predict.

**Operators** receive incomplete context. Missing pickup windows. Wrong addresses. Documents that arrive after the cargo. When one participant is delayed, every participant that follows is delayed with them — a single information gap cascades into a chain of stalled work across the entire shipment. And when a problem occurs during execution, resolving it requires multiple calls to multiple people before anything can move again.

**The environment** bears a cost that does not need to exist. Trucks drive empty because participants cannot efficiently share job information. Engines idle while drivers wait for instructions delayed in manual chains. 35–40% of truck miles globally are driven empty as a direct consequence of coordination failures — not because there is no freight to move, but because the system is not connected enough to make it flow correctly.

The root cause is the same in every case: there is no coordination layer. The logistics industry is fragmented. Participants operate in silos. This is not a technology problem — it is a coordination problem.

Circuit is built to fix that.



## What Circuit Does

Circuit coordinates the flow of shipments between logistics operators.

When a shipment enters Circuit, the platform takes responsibility for ensuring that every participant involved has what they need to do their work — and that information flows between them in a structured, timely way throughout the entire execution of the shipment.

This is broader than managing how work passes from one operator to the next. Circuit improves how every operator works throughout their job. When an operator needs information to continue their work, they do not have to chase it. The system has already anticipated what they will need and created a task for the right person to provide it — before the operator has to ask. When an exception occurs during execution, the operator updates the system once and all relevant parties are alerted immediately. Resolution happens faster because coordination is no longer manual.

Circuit is not a freight forwarder. It does not carry goods, own transportation assets, or assume operational liability. Operators remain responsible for the physical work they perform. Circuit coordinates the information flow that makes that work happen correctly and on time.


# The Architecture

Circuit is built around four interconnected concepts.

# A Single Source of Truth

Every piece of information about a shipment — instructions, documents, status updates, decisions, exceptions — lives in a single shared record. Every participant reads from the same source. What each participant sees is filtered by their role, but it all comes from the same place. There is no separate version of the truth held by different parties.

This is not just a design preference. It is the foundation that makes reliable coordination possible. Participants cannot coordinate accurately if they are working from different information. The single record eliminates that problem by design.

# A Dedicated Agent for Every Shipment

Each shipment that enters Circuit receives its own dedicated execution agent. This agent holds the full context of that specific shipment — its flow, the operators involved, documents, timelines, decisions, and exceptions. It continuously consults the single source of truth rather than relying on its own memory, ensuring that everything it does is grounded in the current, accurate state of the shipment.

This model reflects the reality of logistics. No two shipments are identical. Routes differ, regulations vary, operators change, and conditions evolve. By giving each shipment its own agent operating from a shared, always-current record, Circuit can adapt without disrupting other shipments in the network.

# A Two-Layer Status Model

Circuit separates two things that most logistics systems conflate: the status of an operator's work, and the actual phase of the shipment in the real world.

These are not the same thing. An operator declaring that their work is complete does not mean the shipment has moved forward — it means they believe their part is done. Whether the shipment actually progresses depends on something else: whether the next participant has confirmed they have received everything they need to begin.

Keeping these two layers separate is what gives Circuit its coordination integrity. At every moment, the system knows both what each operator is doing and where the shipment actually is. Most systems mix these layers. That is why they break.

# Structured Responsibility Transfer

Related to the above: in Circuit, completion is not declared by the sender. It is validated by the receiver.

When an operator considers their work done, the shipment does not automatically progress. The next operator reviews what has been passed to them and confirms they have the complete context and documentation required to take over. Only when that confirmation happens does responsibility formally transfer — and with it, the eligibility for payment.

This principle eliminates ambiguity at every point in the shipment. It is always clear which participant holds responsibility. No one can claim work is complete while the next person is still waiting for something they were never given.



# Operator Network

Circuit works with a distributed network of independent operators — carriers, customs agents, freight forwarders, warehouses, inspectors, and other service providers. Operators retain their independence, their pricing, and their legal responsibilities. Circuit coordinates their work without replacing their businesses.

Operators can register multiple capability profiles within a single company account. A freight forwarder can register separately as a carrier booker, a customs agent, and a warehouse operator. This allows Circuit to route specific parts of a shipment to the right specialist rather than assigning the entire shipment to one party.

When a shipment enters Circuit, operators for the full flow are identified upfront — before their step begins. Each identified operator receives a job offer and can accept or decline. As the shipment reaches each operator, they confirm they have received what they need before taking on responsibility. This two-step model — commitment followed by responsibility acceptance — is how Circuit maintains clear accountability throughout the flow.

# Planned Shipments

Shippers who have their own operator relationships and their own preferred way of structuring a shipment can design the flow themselves before Circuit coordinates it. They define the steps, assign their preferred operators to each, and submit the plan. Circuit then coordinates that plan — managing information flow, responsibility transfer, exceptions, and payments — without redesigning what the shipper has already decided.

This means Circuit does not require shippers to abandon existing relationships or hand over full control. They can bring their own network and use Circuit as the coordination infrastructure underneath it.



# Exception Handling

Exceptions are system interrupts, not just statuses. When one is raised, normal progression pauses, visibility increases for all relevant parties, and resolution is required before the shipment can continue.

Circuit handles two types of exceptions. Some can be resolved by the engine automatically — for example, detecting that a shipment needs temporary warehousing and coordinating that directly. Others require human input — a missing document, an approval decision — and the engine creates structured tasks for the responsible participant to complete.

In both cases, the system surfaces the problem immediately, routes it to the right person, and resumes the shipment flow cleanly once it is resolved.



# Payments

Circuit supports two payment models.

In the default model, payment flows directly from the shipper to the operator when their work is confirmed complete. Circuit coordinates when and why payment occurs — generating invoices based on agreed costs, routing payment requests, and confirming completion — but does not hold funds.

In the second model, designed for higher-volume shippers who prefer automation, funds are held at a partner financial institution on behalf of the shipper and disbursed automatically when work is confirmed complete. Circuit does not custody funds in either model.

In both cases, no operator is paid based solely on their own declaration that they are done. Payment is released only after completion has been validated by the receiving participant.



# Design Principles

**Coordination without ownership.** Circuit coordinates logistics operators without owning logistics infrastructure or assuming operational liability. Operators remain responsible for the services they provide.

**Single source of truth.** All information lives in the shipment record. Every participant reads from the same source. There is no version of the truth held separately by different parties.

**Completion is validated by the receiver, not declared by the sender.** True completion — and the transfer of responsibility and payment — only occurs when the next participant confirms they have received everything they need.

**Operators should not have to chase information.** When an operator needs something to continue their work, the system has already created a task for the right person to provide it. Information arrives without the operator having to interrupt their work to go looking for it.

**Minimal AI dependency.** The coordination logic is deterministic by design. AI operates as a supplementary layer — useful in exception handling, anomaly detection, and pattern recognition — not as the foundation of the coordination logic. This reduces computational overhead, improves reliability, and keeps the system auditable.

**Intelligence as a byproduct.** Every shipment that moves through Circuit generates operational data. Over time, this accumulation allows the system to identify patterns, surface risk earlier, and improve coordination across lanes and operator types. Intelligence is not a feature added to the system. It is what the system becomes as it operates.



# Status

This repository documents the conceptual architecture of the Circuit coordination model. Active development of the execution engine is ongoing.


# Author

**Makk** — Founder, Circuit 40s
Building coordination infrastructure for global logistics.
[circuit40s.com](https://circuit40s.com)
