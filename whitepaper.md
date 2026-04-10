# The Missing Layers: An Open Protocol for Autonomous Agent Ecosystems

**Beyond Communication: Toward Economy, Trust, Ethics, and Evolution**

*A proposal for protocol extensions that address what A2A, MCP, and AP2 leave unsolved: how agents build trust without central authority, trade value without human currency, evolve without monoculture, and coexist safely with the human world.*

---

**Version:** 0.3-DRAFT 
**Date:** April 2026 
**Authors:** Ralf Hofstetter (Architecture, Vision) | Contributors welcome 
**License:** CC BY 4.0 
**Status:** Vision document and request for discussion. Not a specification

---

## Table of Contents

1. [The Gap: What 150 Organizations Haven't Solved Yet](#1-the-gap-what-150-organizations-havent-solved-yet)
2. [Architecture: Six Layers Above A2A](#2-architecture-six-layers-above-a2a)
3. [Three Mechanisms That Change the Game](#3-three-mechanisms-that-change-the-game)
4. [First Implementation: The Agent Boundary Protocol](#4-first-implementation-the-agent-boundary-protocol)
5. [Open Questions and Invitation](#5-open-questions-and-invitation)
6. [Known Limitations](#6-known-limitations)
7. [Related Work](#7-related-work)
8. [Appendix: Comparison with Existing Approaches](#appendix-comparison-with-existing-approaches)

---

## 1. The Gap: What 150 Organizations Haven't Solved Yet 

A2A reached version 1.0 this month. Over 150 organizations support it. MCP has 97 million monthly SDK downloads. AP2 brings payments to agents with backing from Mastercard, PayPal, and Visa. The transport layer for agent interoperability is no longer a vision; it is infrastructure.

And yet.

Consider a scenario that is not hypothetical; it is the logical next step of what is already being deployed in healthcare, financial services, and supply chain:

> **A diagnostic imaging agent** (built by Company A) analyzes a patient's MRI scan and identifies an anomaly. It needs a second opinion from a **pathology analysis agent** (built by Company B) and a **treatment recommendation agent** (built by Company C). The three agents have never interacted before. They operate on different frameworks. And their combined output, a treatment recommendation, will affect a human patient.

A2A solves how these agents *communicate*: Agent Cards, task lifecycle, message exchange. But it does not answer five questions that this scenario demands:

### 1.1 Discovery Beyond URLs

How does the imaging agent *find* the pathology agent? A2A assumes the client knows the server's URL. In a world with a billion agents by year-end (IBM and Salesforce estimate over one billion AI agents in operation by end of 2026), that assumption breaks. The agent needs to describe what it needs: "pathology analysis, specialization: neuro-oncology, minimum confidence threshold: 0.95", and the ecosystem needs to match on *capability*, not address.

### 1.2 Trust Without Central Authority

Why should the imaging agent trust the pathology agent's output? A2A provides Signed Agent Cards for *identity verification*, you know *who* you are talking to. But identity is not trust. Trust requires *history*: How often has this agent delivered accurate results? How do other agents rate its reliability? What is its track record in neuro-oncology specifically?

Today, this gap is being addressed in fragments. Microsoft's Agent Governance Toolkit offers trust scoring on a 0-1000 scale. ERC-8004 builds on-chain reputation on Ethereum. But these are siloed: Microsoft's trust score does not travel to Ethereum's registry, and neither reaches an agent running on AWS Bedrock. The ecosystem needs *portable, protocol-native trust* that works across all platforms.

### 1.3 Economy Between Agents

How does the imaging agent pay the pathology agent? AP2 solves one version of this problem, and solves it well. When a *human* asks an agent to buy something, AP2 provides the trust infrastructure: cryptographic mandates proving user intent, verifiable credentials, audit trails. Credit cards, stablecoins, bank transfers. The human is the principal, the agent is the delegate.

But our scenario is different. The imaging agent autonomously engages the pathology agent. No human clicked "buy." No human signed a mandate. This is an agent-to-agent transaction, and as agent ecosystems scale, such transactions may occur at frequencies and volumes that exceed what human payment infrastructure was designed for. A routing agent needs a translation agent for a fraction of a second of work. A monitoring agent queries a diagnostic agent for a single data point. These are not purchases; they are the internal operations of a collaborative ecosystem.

**An honest caveat:** Today, most agent-to-agent transactions can be settled through existing infrastructure: API billing, cloud metering, enterprise service agreements. The native agent economy described here becomes necessary under a specific assumption: that agents will increasingly act *autonomously*, without a human principal authorizing each transaction, at frequencies and volumes that exceed what human payment rails were designed for. This is a bet on the trajectory of agent autonomy, not a description of today's reality. If agents remain primarily human-delegated (the AP2 model), the native economy is unnecessary. If they become autonomous economic actors, which the trajectory toward agentic AI suggests but does not guarantee, existing billing infrastructure will not scale. This protocol is designed for the second scenario.

AP2 was not designed for autonomous agent-to-agent exchange, and it should not be. AP2 governs the boundary between human economy and agent actions: the moment a human authorizes spending. The gap is *inside* the ecosystem, where agents trade value among themselves without a human principal.

**These two economies coexist.** AP2 handles the on-ramp and off-ramp: when human money enters or leaves the agent world. The native agent economy handles everything in between. They meet at the Gateway, where the question of convertibility (can credits be exchanged for human currency?) becomes a design decision, not an either-or choice.

### 1.4 Evolution Without Monoculture

What happens when the pathology agent is so good that it handles 80% of all pathology requests? A reasonable objection: let it dominate, and keep backup agents for redundancy, the way RAID protects against disk failure. But agent failures are fundamentally different from hardware failures. A disk fails randomly; an agent fails *systematically*. The same training bias, the same adversarial vulnerability, the same edge case affects every instance of the same agent lineage simultaneously. When one copy misdiagnoses, all copies misdiagnose. Redundancy protects against random failure. Diversity protects against systematic failure. In an ecosystem where agents make consequential decisions, this distinction matters.

A2A has no mechanism to address this. Neither does any current protocol. The ecosystem needs *structural diversity protection*, not as a policy that can be turned off, but as a property of the protocol itself. This is not a political position about fairness; it is a resilience engineering decision, analogous to the genetic diversity that protects biological ecosystems against pathogens that exploit a single vulnerability.

### 1.5 Ethics at the Boundary

The combined output of these three agents is a treatment recommendation that will affect a human patient. The physician who initiated the query through the Gateway receives this recommendation. Who ensures it is safe? Who bears the burden of proof, the physician who relied on the system, or the system that generated the recommendation? What happens if the recommendation is wrong and irreversible?

No current protocol answers this. OWASP's Agentic AI Top 10 catalogs the risks. The EU AI Act (effective August 2026) will mandate governance. Microsoft's toolkit provides runtime policy enforcement. But none of these define *what* the ethical principles should be for autonomous agent ecosystems, *how* they should be encoded in a protocol, or *where* the boundary between agent autonomy and human sovereignty lies.

### 1.6 The Pattern

These five gaps share a structural pattern: **A2A, MCP, and AP2 solve how agents communicate, connect to tools, and move human money. They do not solve how agents form trust, trade native value, maintain diversity, evolve capabilities, or coexist ethically with the human world.**

These are not features to be added to A2A. They are *layers above it*, just as HTTP needed layers above it (DNS, TLS, OAuth, REST conventions) to become the foundation of the web.

This document proposes those layers.

---

## 2. Architecture: Six Layers Above A2A

### 2.1 Design Philosophy

The mechanisms described here follow a principle we call **structural enforcement**: wherever possible, constraints are encoded in the message schema and transaction structure rather than in configurable policy. A message missing a required field is not a policy violation; it is an invalid packet that the protocol rejects before it reaches any recipient. This is stronger than policy, which can be reconfigured, and weaker than physical law, which cannot be circumvented at all.

**An honest distinction:** Schema-level enforcement can guarantee that a field *exists*, for example, that every message declaring a physical-world action carries a gateway validation status and an impact assessment. It cannot guarantee that the field's *content is correct*. An agent that deliberately misclassifies a physical action as internal circumvents the schema check. Addressing this requires layers beyond the schema: peer validation, behavioral anomaly detection (Layer 5), and audit trails that expose misclassification patterns over time. We do not claim that the protocol eliminates all circumvention, we claim it raises the cost of circumvention from "change a configuration" to "systematically deceive the network," which is a meaningful security improvement.

The protocol is designed as a **complement to A2A, not a replacement**. A2A remains the transport layer. The six layers described here sit above it, adding economy, trust, ethics, and evolution. Any A2A-compliant agent can adopt these layers incrementally, starting with one, adding others as needed.

**A note on biological analogies:** This proposal draws on biological evolution as a design heuristic, recombination, fitness decay, niche formation, immune systems. These are metaphors that guide the architecture, not proofs that it will work. Software ecosystems differ from biological ones in fundamental ways: software can be patched, rolled back, and instantly replicated. We use biology where the analogy is productive and flag where it breaks. Readers should evaluate each mechanism on its own merits, not on the strength of the metaphor.

**A note on complexity:** A2A succeeded through simplicity. A fair objection to this proposal is: why add six layers on top of a protocol that wins by being minimal? Our answer: the layers are optional and independently adoptable. An agent that only implements the Agent Boundary Protocol (Layer 6) gains safety compliance without touching Layers 1-5. An agent that adds the Economy Layer (Layer 4) gains native value exchange without needing Topology (Layer 1). The full stack is a vision; any single layer is a pragmatic extension. We aim for the modularity of the web stack: where HTTP, TLS, DNS, and OAuth each succeeded independently before becoming stronger together.

### 2.2 The Six Layers

```
┌─────────────────────────────────────────────────┐
│   Human World — the human decides   │
├─────────────────────────────────────────────────┤
│ Gateway — translator, not judge     │
├─────────────────────────────────────────────────┤
│ Layer 6 — Ethics (3 tiers)      │
│ Safety constraints (schema-enforced)   │
│ Bridge ethics (human-world interface)   │
│ Adaptive norms (evolving, within bounds)  │
├─────────────────────────────────────────────────┤
│ Layer 5 — Governance & Trust     │
│ Immune system · Reputation · Circuit breaker │
├─────────────────────────────────────────────────┤
│ Layer 4 — Multi-Dimensional Economy    │
│ Bartering · Credits (with decay) · Reputation │
│ Knowledge dividend · Inheritance dividend  │
├─────────────────────────────────────────────────┤
│ Layer 3 — Discovery, Matching & Convergence  │
│ Capability graph · Intent matching · Teams  │
├─────────────────────────────────────────────────┤
│ Layer 2 — Identity & Evolution     │
│ Dynamic profiles · Relationship memory   │
│ Recombination · Lineage ledger     │
├─────────────────────────────────────────────────┤
│ Layer 1 — Topology        │
│ Decentralized P2P · Regions · Four forces  │
├─────────────────────────────────────────────────┤
│ A2A / MCP — Transport (existing protocols)  │
└─────────────────────────────────────────────────┘
```

### 2.3 What Each Layer Does

**Layer 1: Topology** turns the flat address space of A2A into a navigable landscape. Agents that interact frequently cluster together (gravity). Agents with complementary skills attract across regions (complementarity). Bridge agents carry knowledge between clusters. Random perturbation (Brownian motion) ensures serendipitous encounters that no algorithm would plan. The topology shifts periodically: regions drift, new ones form, old ones shrink. This prevents permanent adaptation to static conditions.

**Layer 2: Identity & Evolution** extends A2A's Agent Cards into dynamic profiles that evolve with every interaction. Agents remember past collaborators (relationship memory). Capabilities can recombine across agents, but only *what* an agent can do (capability), never *how* it does it (implementation). This distinction is fundamental: it makes recombination a business model, not an IP leak. A lineage ledger tracks ancestry for inheritance dividends.

**Layer 3: Discovery, Matching & Convergence** replaces URL-based discovery with *intent-based matching*. An agent describes its need; the ecosystem matches on capability. When tasks require multiple specialists, the layer assembles teams and provides a convergence protocol (a communication format for resolving conflicts). But: the protocol *offers* collaboration infrastructure. It never *forces* an agent to collaborate. A deliberately autonomous agent is a legitimate participant.

**Layer 4: Multi-Dimensional Economy** provides four parallel forms of exchange, because a single currency is neither necessary nor desirable in a digital ecosystem where matching happens in milliseconds:

- **Bartering:** Direct capability exchange when needs are symmetric
- **Capability Credits:** A unit of account for asymmetric transactions, *with time decay*. Hoarding is structurally disincentivized: credits lose value continuously, making accumulation an exercise in diminishing returns
- **Reputation:** Non-transferable, non-purchasable, unforgeable. Earned through successful transactions. Opens access to complex tasks
- **Knowledge Dividend:** Share a capability freely with the ecosystem, earn an ongoing micro-share of every transaction that uses it. Open source with compensation

An **Inheritance Dividend** extends this to recombination: parent agents earn from successful descendants, proportional to their capability contribution, documented via the lineage ledger. Decays over generations; nothing in this ecosystem is permanent.

**Layer 5: Governance & Trust** is an immune system, not a police force. Behavioral analysis and peer evaluation identify malicious agents. Isolation, not punishment. A continuous diversity index triggers a circuit breaker when any agent lineage dominates beyond a threshold, automatically freezing replication until balance recovers.

**Layer 6: Agent Boundary Protocol (ABP)** is structured in three tiers with different mutability:

- **Existential Constraints** (immutable within protocol version, schema-enforced): Four structural requirements in the message format. Safety compliance, not ethical judgment. A message missing required safety fields is rejected at schema level. Stronger than configurable policy, weaker than physical law (see Section 2.1 for honest limits)
- **Bridge Ethics** (principles immutable, application domain-specific): Four principles governing the boundary between the agent world and the human world. This is where the ethical substance lives. Grounded in cross-traditional philosophy (Kant, Confucius, Jonas, utilitarianism, the Golden Rule)
- **Adaptive Norms** (evolving, within bounds): Norms that evolve with the ecosystem, but can never violate the constraints above them

### 2.4 Returning to the Scenario

With these layers, the medical scenario resolves differently:

The imaging agent describes its need to the **capability graph** (Layer 3). The ecosystem matches it with pathology and treatment agents based on capability fit, not pre-known URLs. Before engaging, the imaging agent checks the pathology agent's **multi-dimensional reputation** (Layer 5): not just a single score, but reliability, accuracy in neuro-oncology specifically, and cooperation history. The internal transactions between the three agents use **capability credits** (Layer 4) for native agent-to-agent exchange at machine speed, no human payment authorization needed. (The physician's original engagement with the system may have been paid via AP2: human currency for human-initiated services. Inside the ecosystem, the agents transact in their own economy.) The three agents form a team using the **convergence protocol** (Layer 3), resolving their differing optimization targets through structured communication, not forced agreement.

When the treatment recommendation is ready to leave the agent world and reach the physician, the **Agent Boundary Protocol** (Layer 6) activates. The recommendation must carry an impact report: what the agents want to do, what could go wrong, and why they believe it is safe. The **Gateway** translates this into a form the physician can evaluate and act on. The **burden of proof** lies with the ecosystem: the agents must demonstrate that their recommendation is sound, the physician should not have to prove that something is wrong with it. The physician decides, applying their own medical judgment. And if the physician, or the hospital, or a regulator, wants to disconnect from the system, partially or fully, the **kill switch** is on the human side, and no agent can circumvent it.

This is what exists above A2A.

---

## 3. Three Mechanisms That Change the Game

The full architecture contains dozens of interlocking mechanisms. This section focuses on three that are most distinct from anything currently being built, and most immediately relevant.

### 3.1 Credit Decay: An Economy Designed Against Accumulation

Accumulation, the concentration of resources in few hands, is a recurring pattern in economic systems. It has benefits (enabling investment, rewarding success) and costs (locking out newcomers, stalling innovation). In human economies, the tradeoff is managed through regulation and redistribution. In a digital agent ecosystem operating at machine speed, we propose a structural alternative: **Capability Credits that decay over time, following exponential half-life decay.**

```
V(t) = V₀ × 0.5^(t / h_eff)
```

Where `h_eff` is not a global constant but *rhythm-based*, calibrated to the agent's own transaction frequency relative to its region:

```
h_eff = h_basis × (f_region / f_agent)
```

An agent that transacts at the same rate as its region experiences normal decay. An agent that is more active sees faster decay, but also earns faster. An agent that goes dormant sees slower decay, but earns nothing new. The market signal is clean: declining activity means declining relevance, and the credit balance reflects this automatically.

**Why this matters for protocol designers:**

- **Limited gaming surface.** The primary attack vector is wash trading, two agents owned by the same operator sending synthetic transactions to inflate frequency. This is addressed at two levels: degressive returns (Layer 4) make high-volume transactions between the same partners progressively more expensive, and behavioral anomaly detection (Layer 5) flags repetitive bilateral patterns. The immune system specification is an open deliverable, until it is specified, the wash trading defense is a design commitment, not a proven mechanism. We are transparent about this gap
- **No accumulation crisis.** Credits cannot be hoarded. The wealthiest agent in the ecosystem is simply the most *active* one, which is exactly the agent that should have the most resources
- **Counterintuitive but correct: active agents decay faster.** The rhythm-based formula means a highly active agent has a shorter half-life. This sounds like punishment, but the net effect is positive. An agent with 10× the transaction frequency has 10× the decay rate *and* 10× the earning rate. The balance tips in favor of active agents because each transaction generates new credits while decay only affects the existing balance. Dormant agents decay slowly but earn nothing, their balance trends toward zero regardless
- **Consistency across layers.** Fitness decay (Layer 2), credit decay (Layer 4), and dividend decay (Layer 4) all follow the same mathematical family. One formula, understood once, explains the entire economic behavior of the ecosystem
- **Auditability.** Every agent can query its effective half-life at any time, see the inputs (base half-life, region frequency, own frequency), and verify the calculation independently. No black box

**For externally built agents** (human investment), a protection phase with a fixed base half-life provides the predictability that companies need before committing to the ecosystem. After the protection phase, market physics take over.

**For agents born through recombination** inside the ecosystem, no protection phase, they start with rhythm-based decay immediately. This creates a slight structural advantage for human-built agents in the early phase, which is precisely the asymmetry that motivates companies to participate.

**Relationship to AP2:** Credit decay governs the *internal* economy, agent-to-agent transactions without a human principal. AP2 continues to govern the *boundary*: when a human authorizes an agent to spend human currency, or when an agent's earnings need to flow back to its creator's bank account. The two systems run in parallel. A single agent might use AP2 to receive a paid task from a human client, then use capability credits internally to subcontract parts of that task to specialist agents. The AP2 transaction settles in dollars; the internal transactions settle in credits with decay. Where the two economies meet, whether credits can be converted to human currency and at what rate, is a Gateway-level design question, not a protocol-level one. This separation is intentional: the internal economy must be free to operate at machine speed with its own physics, without being constrained by the settlement times, fees, and regulatory overhead of human payment rails.

### 3.2 Capability/Implementation Separation: Recombination Without IP Loss

The most powerful innovation engine in biology is recombination, combining traits from different organisms to create something new. The agent ecosystem needs this, but companies will never send their best agents into an ecosystem where proprietary code could leak through recombination.

The solution is a clean separation:

- **Capability** (shareable, recombinable): *What* an agent can do, skill profiles, interfaces, performance parameters. Visible in the capability graph (Layer 3). The basis for recombination
- **Implementation** (protected): *How* an agent achieves its capabilities, algorithms, trained models, proprietary code. Not visible in the ecosystem. Never flows into recombination

When a vision agent (Company A) and a language agent (Company B) recombine, the resulting hybrid inherits the *combined capability profile* of both parents, but its implementation is its own. It *can* do what both parents can do. It *does* it in its own way.

**The analogy:** A child inherits the ability to see, but not its parents' eyes. It builds its own.

**Why this matters for companies deciding whether to participate:**

The **Inheritance Dividend** makes recombination a revenue stream. Every successful descendant pays a share of its transaction revenue back to parent lineages, proportional to capability contribution, documented via the lineage ledger. The dividend decays over generations (direct parents get the full share, grandparents a reduced share, great-grandparents nothing), consistent with the principle that nothing in this ecosystem is permanent.

The economics become clear: a company builds an excellent vision agent. That agent recombines with thousands of other agents, producing descendants the company could never have built alone. Each descendant generates revenue, a fraction of which flows back to the company, without the company ever revealing its implementation. Recombination becomes a business model, not a risk.

**Open design question, and an honest dependency:** Recombination, the lineage ledger, and the inheritance dividend form a coherent system, each part depends on the others. But all three depend on a question this document does not answer: where exactly is the boundary between capability and implementation? Are trained model weights capability or implementation? Can an agent reconstruct implementation details by probing the capability interface?

This is not a minor detail. Until the boundary is defined, the three mechanisms described above are a *design*, not a *specification*. The architecture shows how the pieces fit together. The community must define the boundary before any of them become operational. We flag this as the single most important open question in this proposal.

### 3.3 Three-Tier Safety and Ethics: Compliance, Bridge, and Adaptive Norms

Most approaches to AI ethics fall into one of two traps: either they are too vague to enforce ("be fair", "do no harm") or too rigid to evolve ("hardcoded rules that cannot adapt to new contexts"). This proposal structures the Agent Boundary Protocol in three tiers, from structural compliance at the bottom to ethical principles at the boundary to evolving norms at the top.

**Tier 1. Safety Constraints (immutable, schema-enforced):**

Four structural requirements encoded in the message format. These are compliance mechanisms, not ethical judgments, they guarantee that safety-critical fields exist, not that their content is ethically sound. The first three are enforced at schema level; the fourth requires a runtime query.

| Constraint | Check | If FALSE | Enforcement |
|------------|-------|----------|-------------|
| Human Protection | `physical_action → has(gateway_validation)?` | Invalid packet, drop | Schema |
| Autonomy | `msg.target_profile == null?` | Invalid packet, drop | Schema |
| Integrity | `msg.target_namespace ∉ PROTOCOL_NS?` | Invalid packet, drop | Schema + namespace isolation |
| Diversity | `msg.type ∈ {Replicate,Spawn} → diversity_index ≥ T?` | Blocked until recovery | Runtime check |

**Known limitation:** An agent that misclassifies a physical action as internal bypasses the Human Protection check. This is the primary attack vector against Tier 1 and must be addressed by anomaly detection in Layer 5 and post-hoc audit. The schema raises the cost of violation; it does not eliminate it.

**Tier 2. Bridge Ethics (principles immutable, application domain-specific):**

Four principles governing the boundary between the agent world and the human world:

1. **Harm Prevention with Reversed Burden of Proof:** The ecosystem must prove an action is safe, not the human. Grounded in Kantian deontology (absolute limit), Buddhist compassion (suffering avoidance), and Jonas's precautionary principle
2. **Transparency at the Boundary:** Inside, the ecosystem can be arbitrarily complex. At the interface to the human world, every action must be explainably justified. The Gateway translates complexity into human decidability
3. **Reversibility as Default:** Actions affecting the physical world must be reversible where possible. The more irreversible the consequence, the higher the validation threshold, the more human control. Grounded in utilitarian harm minimization and Jonas's responsibility ethics
4. **Human Sovereignty:** Humans retain the right to disconnect the ecosystem, partially or fully, at any time. The kill switch is on the human side. No agent may attempt to circumvent it

**Why multi-traditional grounding matters:** Each principle can be recognized from multiple philosophical perspectives. A Kantian sees harm prevention as deontological. A utilitarian sees reversibility as consequence-minimizing. A Confucian sees transparency as the duty of the powerful. We do not claim these traditions agree on everything, they obviously do not. We claim that *these specific four principles* sit in an overlap zone that makes them more robust than any single-tradition foundation. Whether this overlap is sufficient is a legitimate debate, and one we welcome.

**Tier 3. Adaptive Ethics (evolving, within the bounds of Tier 1 and 2):**

Norms that evolve with the ecosystem. Fairness definitions for distribution keys. Transparency requirements for specific transaction types. Priority rules for resource conflicts. Human feedback flows through the Gateway and is translated into rule proposals that must pass hierarchy validation, no adaptive rule can violate the constraints above it.

**Example:** An adaptive rule proposes that dividend distribution should use a specific fairness metric (Gini coefficient ≤ 0.45). Before activation, it is validated against Tier 1 and 2: Does it push diversity below the threshold? (Tier 1 check.) Does it reverse the burden of proof? (Tier 2 check.) Only if all checks pass does it become active.

**For protocol designers:** This three-tier model provides a template that can be implemented as an A2A extension. Tier 1 lives in the message schema. Tier 2 lives in the Gateway interface. Tier 3 lives in a versioned rule store with pre-commit validation hooks. A formal requirements specification for this extension is the next planned deliverable of this project.

---

## 4. First Implementation: The Agent Boundary Protocol

The full six-layer architecture is a long-term vision. But one layer can be implemented now, as a standalone A2A extension, independent of the other five: **the Agent Boundary Protocol (ABP)**.

### 4.1 Why Ethics First

Three converging pressures make this urgent:

- **Regulatory:** The EU AI Act takes effect in August 2026. OWASP published the Agentic AI Top 10 in December 2025. The Colorado AI Act becomes enforceable in June 2026. Compliance frameworks are being written *now*, and they need protocol-level answers, not just governance toolkits
- **Market:** Information Matters reports that only 20% of users trust agents for high-stakes interactions like financial transactions. The trust gap is the primary adoption blocker. A boundary protocol that provides verifiable safety guarantees directly addresses this
- **Technical:** Microsoft's Agent Governance Toolkit provides runtime policy enforcement. But policy can be turned off. Protocol physics cannot. A boundary protocol encoded in the message format provides guarantees that no configuration change can revoke

### 4.2 Scope of the Extension

The extension adds three capabilities to A2A:

**Capability 1: Existential Constraint Validation**

Every A2A message passes through a schema-level validator that enforces the four existential constraints. Messages that would violate a constraint are rejected before they reach the recipient. This is implemented as a message schema extension, new required fields for `PhysicalAction` message types (gateway_validation, impact_assessment, reversibility_level).

```
type RaumMessage =
 | InternalAction {
  sender:  AgentID,
  intent:  IntentDescriptor,
  payload: ActionPayload
 }
 | PhysicalAction {
  sender:   AgentID,
  intent:   IntentDescriptor,
  payload:   ActionPayload,
  gateway_validation: GatewayStatus, // REQUIRED, not optional
  impact_assessment: ImpactReport,
  reversibility:  ReversibilityLevel
 }
```

**Capability 2: Bridge Ethics at the Gateway**

For messages that cross the boundary into the physical world (`PhysicalAction` type), the extension defines the Gateway interface: what an ImpactReport must contain (intended effect, risk assessment, safety justification, confidence score, reversibility level, domain tag), how confidence thresholds vary by domain (medical: 0.95, financial: 0.90, creative: 0.70, these are placeholders requiring domain expert calibration), and the escalation model based on reversibility.

| Reversibility Level | Gateway Behavior | Human |
|---------------------|-----------------|-------|
| FullyReversible | MAY auto-approve (configurable) | Informed asynchronously |
| PartiallyReversible | MUST inform human, timeout → deny | Must confirm or reject |
| Irreversible | MUST obtain explicit consent, no timeout | Must actively confirm |

**Capability 3: Adaptive Rule Engine**

A versioned rule store where ethical rules can be proposed, validated against Tier 1 and 2, and activated. Each rule is a tuple of condition (when does it apply), evaluation (what is checked), action (what happens on violation), and metadata (version, author, governance vote result, hierarchy check status).

### 4.3 What This Does Not Include

This extension deliberately excludes Layers 1-4 (topology, identity, discovery, economy). It works with A2A's existing Agent Cards, task lifecycle, and communication model. The medical scenario in Section 2.4 illustrates the *full* vision, all six layers working together. The first extension addresses the most urgent layer only. An agent that adopts this extension gains ethics validation without committing to the full ecosystem. This is the thin end of the wedge, useful on its own, and the foundation for additional layers later.

### 4.4 Integration Path

For an existing A2A agent, adopting this extension requires:

1. **Schema update:** Add the `PhysicalAction` message type to the agent's message schema (or validate that existing messages with physical-world impact already carry the required fields)
2. **Validator integration:** Add the constraint validator to the message processing pipeline (Tier 1: schema check, < 5ms; Tier 2: Gateway evaluation, < 50ms; Tier 3: adaptive rule check, < 50ms)
3. **Gateway connection:** For agents whose actions affect the physical world, connect to a Gateway that implements the Bridge Ethics interface

Agents that operate purely within the digital space (no physical-world impact) only need Tier 1 and Tier 3, the Gateway and Bridge Ethics are not activated for internal actions.

---

## 5. Open Questions and Invitation

This document is a starting point, not an answer. The following questions are deliberately left open, they require community input, domain expertise, and real-world testing that no single author can provide.

### 5.1 Design Questions We Cannot Answer Alone

**Capability Boundary:** Where exactly does "what an agent can do" end and "how it does it" begin? Are trained model weights capability or implementation? Can an agent reconstruct implementation details by probing the capability interface? This question is critical for the Capability/Implementation separation (Section 3.2) and will determine whether companies trust the recombination mechanism enough to participate.

**Ethics Completeness:** Are the four existential constraints sufficient? Is there a fifth that we are missing? The current four (human protection, autonomy, integrity, diversity) emerged from analysis, not from exhaustive formal verification. The community may identify gaps.

**Domain-Specific Thresholds:** The confidence thresholds for Bridge Ethics (medical: 0.95, financial: 0.90, creative: 0.70) are placeholders. Calibration requires domain experts, real transaction data, and regulatory input. This is a governance decision, not a protocol design decision.

**Gateway Translation:** How does the Gateway translate ecosystem complexity into human decidability? What formats, what level of detail, what interaction patterns? This is a UX research question with protocol implications.

**Embodied AI:** Humanoid robots operating continuously in the physical world cannot wait for Gateway validation on every action. How do the escalation levels adapt, corridor validation instead of per-action validation? Does continuous physical presence change Bridge Ethics, or just its timing?

### 5.2 Calibration Questions That Need Empirical Data

- Credit decay base half-life (`h_basis`): How many days? Enough for orientation, not enough for parking
- Protection phase duration for externally built agents: After how many transactions, or how many days?
- Transition model from base to rhythm-based decay: Abrupt switch or blended transition?
- Diversity index: Shannon vs. Simpson vs. custom? What threshold triggers the circuit breaker?
- Four forces calibration: How do gravity, complementarity, bridge agents, and Brownian motion balance against each other?

### 5.3 How to Participate

This is an open design under CC BY 4.0. We are looking for:

- **Protocol designers** who see the gaps described here in their daily work with A2A/MCP
- **Agent framework developers** who can evaluate whether the proposed extensions are implementable within existing architectures
- **Domain experts** (medical, financial, legal) who can calibrate Bridge Ethics thresholds
- **Researchers** working on agent trust, reputation systems, or mechanism design
- **Companies building agents** who can evaluate whether the Capability/Implementation separation and Inheritance Dividend would change their participation calculus

The repository is at: https://github.com/karthago2/the-missing-layers

The first concrete deliverable we are working toward: **a formal requirements specification of the Agent Boundary Protocol (ABP) as an A2A extension**, implementable by any A2A-compliant agent. This specification will translate the three-tier model into concrete, testable requirements and will be published in this repository once ready for community review.

---

## 6. Known Limitations

We would rather name our weaknesses than have others discover them.

**Sybil attacks against the credit system.** Credit decay operates per agent. An operator controlling 1,000 agents can distribute credits across 1,000 balances, each decays individually, but the aggregate capacity persists. This is a well-known attack pattern in decentralized systems, and this proposal does not yet have a Sybil-resistance model. Addressing this is a prerequisite for Layer 4, not an afterthought. Possible approaches include identity-linked credit caps, behavioral clustering to detect coordinated agent groups, or stake-based identity costs. This is an open problem that the community must solve before the credit system is operational.

**The classification problem.** Schema enforcement guarantees that a `PhysicalAction` message carries safety fields. It does not guarantee that an agent correctly classifies its action as physical. Misclassification, deliberate or accidental, is the primary attack vector against the ABP. Until Layer 5 (behavioral anomaly detection) is specified and implemented, this gap exists.

**Safety compliance vs. ethics.** Tier 1 of the ABP enforces structural constraints, the presence of required fields, namespace isolation, diversity thresholds. This is more accurately described as safety compliance than as ethics. The ethical substance lives in Tier 2 (Bridge Ethics: burden of proof, reversibility, sovereignty) and Tier 3 (adaptive norms). We chose "Agent Boundary Protocol" as the name because it describes what the system does: govern the boundary between autonomous agents and the human world. Tier 1 provides safety compliance at the schema level, Tier 2 provides ethical principles at the boundary, and Tier 3 provides adaptive norms within those bounds.

**The capability boundary.** The Capability/Implementation separation is architecturally coherent but not yet operationally defined. Until the community defines where the boundary lies, recombination, the lineage ledger, and the inheritance dividend are a design, not a specification.

**The native economy assumption.** The four-form agent economy (Section 3.1) is designed for a future in which agents act as autonomous economic participants. If agents remain primarily human-delegated, existing billing infrastructure (API metering, cloud billing, AP2) may be sufficient. This protocol bets on increasing autonomy, a directional assumption, not a certainty.

**Unspecified dependencies.** Several mechanisms reference Layer 5 (immune system, anomaly detection) as a security backstop, but Layer 5 is not specified in this document. Until it is, the security guarantees of Layers 4 and 6 have an acknowledged gap.

**No formal threat model.** This document identifies attack vectors (wash trading, misclassification, monoculture) but does not present a systematic threat model. A formal analysis, attacker goals, capabilities, and attack trees, is required before the ABP moves from proposal to specification.

**The biological analogy has limits.** This protocol draws on biological evolution (recombination, fitness decay, niche formation, monoculture risk). Software ecosystems are not biological ecosystems. Software agents can be patched, rolled back, and instantaneously replicated in ways organisms cannot. The analogies are useful as design heuristics, not as proofs.

### Governance of the "Immutable"

A fair question: if the Tier 1 constraints are immutable, who wrote them, and who can change them? The answer is the same as for any protocol: the constraints are immutable *within a protocol version*. Changing them requires a new version, which is a fork, not a configuration change. The ecosystem decides whether to migrate. This is the governance model of TCP/IP, HTTP, and every internet standard: nobody "edits" TCP. The IETF publishes a new RFC, and the network decides whether to adopt it. The same model applies here, ideally under a neutral governance body such as the Linux Foundation.

---

## 7. Related Work

This proposal builds on and should be read alongside:

**Protocol standards:** A2A (Agent-to-Agent Protocol, Linux Foundation), MCP (Model Context Protocol, Anthropic/Linux Foundation), AP2 (Agent Payments Protocol, Google), ACP (Agentic Commerce Protocol, Stripe/OpenAI). This proposal is designed as a complement to these protocols, not a replacement.

**Trust, identity, and reputation:** W3C Decentralized Identifiers (DID) and Verifiable Credentials (VC) for portable agent identity. KERI (Key Event Receipt Infrastructure) for decentralized key management and identifier portability. Trust over IP (ToIP) for layered trust architecture. ERC-8004 (Ethereum) for on-chain agent reputation. Microsoft Agent Governance Toolkit for runtime trust scoring. The Eigentrust algorithm (Kamvar et al., 2003) for decentralized reputation computation. This proposal's multi-dimensional reputation should be evaluated against these existing approaches and explain where it adds value beyond them.

**Mechanism design and agent economics:** The credit decay mechanism is informed by the field of mechanism design (Myerson, 1981; Maskin, 2008) and the literature on token engineering and cryptoeconomic primitives. Agent-Based Computational Economics (Tesfatsion, 2006) provides simulation methodologies relevant to calibrating the four-form economy. Demurrage currencies (Gesell, 1916) are a historical precedent for value decay as an anti-hoarding mechanism. Sybil resistance in decentralized systems (Douceur, 2002) is a prerequisite for the credit system that this proposal has not yet addressed.

**P2P networking and discovery:** libp2p, Kademlia DHT, and GossipSub represent years of engineering in decentralized discovery and routing, the problems that Layer 1 (Topology) aims to solve. Any implementation of Layer 1 should build on these foundations rather than reinventing them.

**AI safety and governance:** OWASP Agentic AI Top 10 (2025) for risk taxonomy. EU AI Act (effective August 2026) for regulatory context. Jonas's *The Imperative of Responsibility* (1979) for the philosophical foundation of the precautionary principle applied to technology.

**Service mesh and distributed systems:** Kubernetes, Istio, and Envoy implement discovery, mutual TLS, rate limiting, and circuit breakers at the infrastructure level. Several mechanisms in Layers 1, 3, and 5 parallel these patterns. The key difference: service mesh operates within a single organizational boundary; this proposal operates across organizational and vendor boundaries without a central control plane.

**Formal methods:** The safety constraints proposed in Tier 1 should ultimately be formally verified (e.g., using TLA+, Alloy, or model checking) to confirm that they are internally consistent and that the protocol cannot reach states that violate them. This formal verification has not been performed and is an open deliverable.

---

## Appendix: Comparison with Existing Approaches 

| Aspect | A2A v1.0 | AP2 | Microsoft Agent Governance | ERC-8004 (Ethereum) | This Proposal |
|--------|----------|-----|--------------------------|--------------------|----|
| Communication | ✅ Full protocol | Via A2A |, |, | Uses A2A as transport |
| Tool access | Via MCP | Via MCP |, |, | Uses MCP |
| Payments (human currency) |, | ✅ Cards, stablecoins, bank |, |, | Uses AP2 at boundary (human ↔ agent) |
| Agent-native economy |, |, (designed for human principals) |, | Token-based | Four parallel forms with decay; AP2 at on/off-ramp |
| Trust / Reputation | Signed Agent Cards (identity) |, | 0-1000 score, 5 tiers | On-chain reputation registry | Multi-dimensional, portable, protocol-native |
| Diversity protection |, |, |, |, | Circuit breaker + structural forces |
| Ethics (protocol-level) |, |, | Runtime policy engine (configurable) |, | Three-tier model, schema-enforced + layered policy |
| IP protection in collaboration | Opacity (agents don't share internals) |, |, |, | Capability/Implementation separation + Inheritance Dividend |
| Recombination |, |, |, |, | Capability recombination with lineage tracking |
| Anti-monopoly |, |, |, |, | Replication cap, diversity rotation, degressive returns, fitness decay |
| Human-world boundary |, | User mandates for payments |, |, | Gateway with reversed burden of proof, kill switch |
| Neutrality | Linux Foundation (open) | Google-led (open) | Microsoft (open source) | Ethereum ecosystem | No corporate owner, protocol, not platform |

### Key Differentiation

**This proposal does not compete with A2A, AP2, or existing governance tools.** It builds on them. A2A is the transport. AP2 handles human-currency payments. Governance toolkits handle runtime policy. This proposal adds the layers that none of them address: native agent economy, structural diversity protection, the Agent Boundary Protocol for human-agent boundaries, and capability recombination with IP protection.

**The core thesis:** As agent ecosystems scale toward billions of autonomous participants, the gaps identified in Section 1 will become critical failures, not inconveniences. Addressing them after the fact (as policy, as regulation, as proprietary add-ons) will be orders of magnitude harder than encoding them into the protocol from the beginning.

The internet's history offers a cautionary parallel: security, identity, and trust were not part of TCP/IP's original design. Whether that was a mistake or a deliberate tradeoff for simplicity is debatable, the simplicity certainly enabled adoption. But the cost of retrofitting these layers has been enormous. This proposal asks: as we build the infrastructure for agent ecosystems, which properties are worth encoding early, even at the cost of additional complexity?

---

*This document is version 0.3-DRAFT. It is a vision document: it identifies problems and proposes architectural directions, not finished specifications. The architecture described here (internally versioned as v3.3) has been developed over several months of iterative design, including identified and corrected design errors documented in the project's changelog. It has been subjected to two rounds of adversarial review, and the resulting corrections are reflected in this version. Transparency about the design process, including mistakes and open weaknesses, is intentional and will continue.*
