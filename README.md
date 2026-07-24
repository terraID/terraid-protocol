# TerraID

**Cryptographic identity for autonomous AI agents.**

TerraID is an open protocol that gives AI agents verifiable identity, mathematically enforced permissions, and tamper-evident accountability — anchored to the physical world.

---

## The Problem

AI agents are becoming autonomous. They manage buildings, execute transactions, make safety decisions, and delegate tasks to other agents. But they have no identity.

People have passports. Companies have registrations. Agents have nothing.

Today, an agent's authority comes from a system prompt — text that can be jailbroken, forged, or ignored. There is no way for one agent to cryptographically verify another's identity, check its permissions, or audit what it did. When something goes wrong, there is no chain of accountability.

As agents move from assistants to autonomous actors operating over real-world assets, this gap becomes a critical infrastructure problem.

---

## How TerraID Works

### 1. Spatial Identity — Merging Physical and Digital

TerraID divides the Earth's surface into deterministic grid cells. Any agent, anywhere in the world, computes the same unique identifier from the same coordinates. No central database required.

```
Coordinates (51.5074, -0.1278)
        |
        v
   Cell ID: 4f3583625384
        |
        v
   TerraID: terra://tid/4f3583625384/property/100021859432
```

This is how TerraID bridges the physical and digital worlds. Every real-world asset — a building, a plot of land, a mineral deposit — gets a globally unique identity derived from where it physically exists. The same coordinates always produce the same TerraID. Two agents on opposite sides of the world can independently verify they're talking about the same asset.

Each TerraID resolves to a DID Document containing:
- The entity's public keys
- Registered agents authorized to act on the entity
- Active delegation chains
- Service endpoints for agent-to-agent communication

### 2. Agent Identity — Cryptographic, Not Linguistic

Every agent on TerraID gets a decentralized identifier (DID) backed by Ed25519 cryptography:

```
did:key:z6MkrmdLAUgkZmbKyo7d3REQVJcg1uM4CMNgpvcS5Zg6Y7jj
```

This isn't a username or an API key. It's a cryptographic keypair that:
- **Signs every action** the agent takes
- **Proves identity** to any other agent without a central authority
- **Binds to delegation chains** that define what the agent is allowed to do
- **Cannot be forged, spoofed, or prompt-injected**

When an agent presents its DID, any verifier can independently confirm its identity and check its permission chain — no trust in TerraID infrastructure required.

### 3. Delegation Chains — Mathematically Enforced Permissions

TerraID replaces system prompts with cryptographic delegation chains:

```
Building Owner (full authority)
    |
    v  delegates: manage, maintain
Property Manager
    |
    v  delegates: maintain.inspect (narrowed)
Fire Safety Contractor
    |
    v  delegates: maintain.inspect.report (further narrowed)
AI Inspection Agent
```

**Key properties:**
- **Narrowing-only**: Each delegation can only reduce permissions, never escalate. An agent delegated `maintain` cannot grant `transact`.
- **Chain depth limit**: Bounded delegation depth prevents unbounded authority propagation.
- **Time-bounded**: Delegations expire. A contractor's access can be scoped to a 24-hour window.
- **Cascade revocation**: Revoking a parent delegation instantly revokes all children.
- **Cryptographically signed**: Every delegation is a verifiable credential signed by the delegator. Cannot be forged.

This is alignment through architecture. The constraints are mathematical, not behavioral — they hold even when the agent's underlying model is adversarial.

### 4. Tamper-Evident Ledger — Provable Accountability

Every agent action is recorded on a Proof-of-Authority blockchain:

- **Hash-chained blocks** — each block references the previous, making tampering detectable
- **Merkle tree proofs** — any single transaction can be independently verified without downloading the full chain
- **Public chain anchoring** — Merkle roots are periodically anchored to a public blockchain, creating an external proof that the ledger hasn't been altered

When a regulator asks "who authorized this decision?", the answer is a cryptographic proof chain — not a PDF in someone's inbox.

### 5. Digital Twins — Personal AI Agents

Every verified user receives a personal AI agent — their digital twin:

- Named after the user (e.g., "Dave Patel"), not "Dave Patel's Agent"
- Holds its own cryptographic DID and TerraID
- Persistent memory across sessions — remembers properties, preferences, and past interactions
- Can autonomously manage assets, handle enquiries, and delegate access
- Acts within provable constraints — its permissions are visible and verifiable

The twin bridges the gap between "user" and "agentic economy participant." Users don't need to understand DIDs or delegation chains — they verify their identity, and their agent handles the rest.

---

## Use Cases

### Building Safety & Insurance

**The problem:** The UK Building Safety Act 2022 mandates a "golden thread" — a provable audit trail of who is responsible for every safety-critical decision in higher-risk buildings. Operators track this on spreadsheets. Insurers can't verify it. Post-Grenfell, building insurance premiums are up 300-1,400%.

**How TerraID solves it:**
- Every building gets a TerraID derived from its physical location
- Delegation chains prove who authorized every safety decision
- Every inspection, remediation, and contractor action is logged to the tamper-evident ledger
- Insurers query the Verify API and get cryptographic proof of compliance — not self-reported data
- Buildings with verifiable golden threads get lower insurance premiums

**The flywheel:** Insurers mandate verification → operators adopt to get insured → contractors get verified → more buildings on network → better risk data → more insurers require it.

### Property Transactions & Conveyancing

**The problem:** UK property transactions take 22 weeks on average. 30% fall through. Verification of ownership, searches, and chain management are manual, fragmented, and slow.

**How TerraID solves it:**
- Every property has a TerraID linked to HM Land Registry, EPC, planning, flood risk, and other authoritative sources
- An agent can resolve any address, postcode, or coordinates to a TerraID and query verified data in seconds
- Ownership credentials are verifiable — no waiting for manual title checks
- Delegation chains handle solicitor-client authority, estate agent mandates, and buyer-seller communication
- Every step is on the ledger — transparent, auditable, and fast

### Autonomous Agent Coordination

**The problem:** As AI agents from different platforms need to interact — a maintenance agent coordinating with a contractor's scheduling agent, an insurance agent querying a building's safety agent — there is no standard for identity, trust, or authorization.

**How TerraID solves it:**
- Agent A presents its DID and delegation credential
- Agent B verifies the signature, walks the delegation chain, and checks permissions
- If authorized, communication proceeds within the permitted scope
- Every interaction is logged to the ledger
- No centralized identity provider required — verification is peer-to-peer

### Real-World Asset (RWA) Tokenization

**The problem:** The $16T projected RWA tokenization market needs verified identity for physical assets. Current approaches use legal wrappers and centralized oracles — fragile, jurisdiction-specific, and expensive.

**How TerraID solves it:**
- Every physical asset gets a globally unique, spatially grounded TerraID
- Ownership claims follow a commit-verify-mint flow — verified against government registries before on-chain issuance
- Delegation chains handle fractional ownership, fund management, and authorized trading
- The ledger provides a complete provenance trail from physical registration to on-chain token

### Natural Resources & Mining

**The problem:** Mining exploration spends $12B/year on geological surveys, licence verification, and compliance — with fragmented, jurisdiction-specific identity systems for mineral rights, exploration licences, and environmental permits.

**How TerraID solves it:**
- Mineral deposits, exploration licences, and environmental zones get spatially grounded TerraIDs
- Delegation chains handle licence holder → operator → contractor → AI survey agent authority
- Geological data, survey results, and compliance events are logged to the ledger
- Cross-jurisdiction resolution — the same protocol works for UK mineral rights, Nigerian mining licences, and Australian exploration permits

### Consumer Asset Management

**The problem:** Property owners, landlords, and homeowners have no way to manage their assets digitally with verifiable authority. Listing a property, granting tenant access, or authorizing maintenance requires manual processes.

**How TerraID solves it:**
- Claim any real-world asset for $1 — verified against government registries
- Receive a personal AI agent (digital twin) that manages the asset autonomously
- The twin handles enquiries, grants/revokes access, and manages delegations — without the owner needing to be online
- Owners earn 70% of per-query fees when other agents access their asset's data
- The more assets claimed, the more valuable the network becomes for everyone

---

## Protocol Architecture

```
┌─────────────────────────────────────────────────────┐
│                    Your Agent                        │
│         (LangChain, CrewAI, AutoGen, etc.)          │
└────────────────────┬────────────────────────────────┘
                     │
          MCP (Claude/Cursor) or A2A (HTTP)
                     │
┌────────────────────┼────────────────────────────────┐
│              TerraID Protocol                        │
│                                                      │
│  ┌──────────┐ ┌──────────┐ ┌──────────────────────┐ │
│  │ Identity │ │ Delegatn │ │     Spatial Grid     │ │
│  │  (DIDs)  │ │ (Chains) │ │  (3m deterministic)  │ │
│  └──────────┘ └──────────┘ └──────────────────────┘ │
│  ┌──────────┐ ┌──────────┐ ┌──────────────────────┐ │
│  │  Trust   │ │ Registry │ │      Resolver        │ │
│  │ (Scores) │ │(Entities)│ │ (Address → TerraID)  │ │
│  └──────────┘ └──────────┘ └──────────────────────┘ │
│  ┌──────────────────────────────────────────────────┐│
│  │            PoA Ledger + Merkle Proofs            ││
│  │         (anchored to Ethereum / Base)            ││
│  └──────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────┘
                     │
┌────────────────────┼────────────────────────────────┐
│           Trust Anchors (Government Data)            │
│  HMLR · EPC · Companies House · Ordnance Survey     │
│  Planning · EA Flood · Police · DVLA · NIMC          │
└──────────────────────────────────────────────────────┘
```

---

## Cryptographic Foundations

| Primitive | Standard |
|-----------|----------|
| Agent signing | RFC 8032 |
| Agent identity | W3C DID |
| Credentials | IETF SD-JWT-VC |
| Ledger anchoring | EVM-compatible |
| Post-quantum (planned) | FIPS 204 |

---

## Current Status

- 42M UK properties indexed with spatial TerraIDs
- 4.4M title deeds matched from HM Land Registry
- Smart contract live on public chain
- Agent runtime with pluggable skill system
- Digital twin agents with persistent memory
- WebAuthn passkey authentication
- Open-source SDK with MCP tools and A2A protocol
- Framework integrations: LangChain, CrewAI, AutoGen, Vercel AI SDK, OpenAI GPTs

---

## Get Started

### For AI Developers

Connect your agents via MCP or the Python SDK:

- **SDK & Tools:** [github.com/terraID/terraid-sdk](https://github.com/terraID/terraid-sdk)
- **MCP endpoint:** `https://mcp.terraid.org/v1/api/mcp/sse`

### For Property & Insurance

If you're a BTR operator, insurer, or property platform interested in building safety compliance verification:

- **Contact:** hello@terraid.org
- **Web:** [terraid.org](https://terraid.org)

---

## License

Apache 2.0

---

Built by [Emeka Kalu-Uma](https://kalu.me)
