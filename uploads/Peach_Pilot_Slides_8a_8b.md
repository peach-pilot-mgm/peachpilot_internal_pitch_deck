# Peach Pilot Pitch Deck: Slides 8a and 8b

**Purpose:** Two slides that explain how JP's platform work and Mario's AINS strategy fit together.

**Audience:** Internal deck for new hires and team members.

**Brand colors:** Navy #1C1629, Salmon #F08A6A (accent), Gray #79858D, White #FFFFFF.
**Typography:** DM Sans (headings), Inter (body).
**Writing rules:** No em dashes. Direct, short sentences.

---

## Slide 8a: Two Builders, One Company

**Headline:** JP built the engine. Mario built the vehicle.

**Subhead:** JP built a horizontal AI platform. Mario uses it to deliver AI Native Services outcomes to companies.

**What we share:**

- **Context is the moat.** As models commoditize, the winner has the deepest understanding of the client. Not the best model. The best context.
- **Knowledge must compound.** A persistent graph that learns and remembers. Not a chatbot that forgets. Not a tool that resets.
- **Humans stay in the loop.** Governance is structural, not cosmetic. Every agent action is auditable. Every high-stakes decision needs a human.
- **Frontier models on top, specialized reasoning underneath.** Claude and GPT for reasoning. Our own graph, our own agents, our own methodology for everything around them.
- **Outcomes over outputs.** Do not just generate a deck. Execute the follow-ups. Run the capability. Own the result.

**Closing line:**
The engine without the vehicle never gets to market. The vehicle without the engine cannot move.

### Speaker Notes

**Opening frame.** If you read JP's PRD and my Strategic Architecture side by side, you might think they describe two different companies. They don't. JP built a horizontal AI platform. I built the go-to-market model to turn that platform into outcomes clients actually pay for. JP built the engine. I built the vehicle. Same company. Two angles.

**The shared bets are the key.** Anyone reading both documents will notice the differences first. The similarities are more important. We both bet that context, not model quality, is the moat. We both bet that compounding knowledge is the engine of value. We both bet on humans in the loop, frontier models plus specialized reasoning, and outcomes over outputs. These are non-trivial bets. Most AI companies are making the opposite bet. The fact that JP and I independently converged on these principles is a signal that the principles are right.

**Why this matters for new hires.** Most technology companies fail because they build an engine and never find the vehicle. Most services companies fail because they have a vehicle but no engine. Peach Pilot has both because JP and I each owned one side. If you are on the platform team, JP's technical work is closer to your daily work. If you are on the transformation or operations team, the AINS delivery model is closer. Everyone at Peach Pilot needs to understand both.

---

## Slide 8b: How the Platform Fits the Strategy

**Headline:** The engine and the vehicle.

**Subhead:** Every component JP specified maps into the Company Brain architecture we deploy at clients.

**The mapping:**

| Platform (JP's PRD) | Delivery Model (Strategy) |
|---|---|
| Context Graph (Neo4j, Q-Learning, GNN) | Knowledge Library (storage layer) |
| Two-tier agent model | Reasoning Engine (Strategic + Operational) |
| Expert Agent Library (built at Hive) | Expert Agent Library (library component) |
| Confidence-based HITL | Governance framework |
| Real-world action bridge | Enables capabilities in production |

**What this means in practice:**

- **The tooling JP specified** is how the Platform Team builds the Company Brain and the agent fleet.
- **The AINS methodology** is how the Transformation Team deploys that tooling into a real client engagement.
- **The MSP operations model** is how the Operations Team runs the capabilities in production after deployment.

**Closing line:**
JP's platform is what we build. The strategy is how we deliver it. Same company. Same bets. Different surfaces.

### Speaker Notes

**Opening frame.** This slide exists to answer a specific question new hires always ask: how does JP's PRD relate to what we're actually doing? The answer is in the mapping table. Every technical component JP specified shows up somewhere in the AINS delivery model. The platform and the delivery model are not separate things. They are the same thing described at different altitudes.

**The Context Graph becomes the Knowledge Library.** JP specified a Neo4j-based graph with Q-Learning edge weights, temporal decay functions, and a Graph Neural Network for multi-hop queries. That graph is the storage layer of the Company Brain. In JP's PRD it is described technically. In the strategy it is described commercially (six information types: Company Knowledge, Domain Knowledge, Operational Signal, Strategic Context, Market Intelligence, Expert Agent Library). Same system. Different language for different audiences.

**The two-tier agent model becomes the Reasoning Engine.** JP specified orchestrator instances that maintain long-running context plus specialist sub-agents spun up for discrete tasks. In the strategy that maps to Strategic Reasoning (deliberative, high-stakes, assembles multiple experts) and Operational Reasoning (continuous, fast, pattern-matching). Same architecture. Expressed in business terms instead of engineering terms.

**The Expert Agent Library is literally the same thing in both documents.** JP built it at Hive. It becomes one of the six components of the Knowledge Library at Peach Pilot. JP maintains and expands it. We deploy it per client.

**Confidence-based HITL becomes our governance framework.** JP specified a five-tier confidence system with Q-Learning permission calibration, anti-annoyance batching, and audit logging. In the strategy that is the governance layer that runs across every capability: audit trails, kill switches, escalation paths, human approval for strategic decisions. Same mechanism. Commercial framing.

**Real-world action bridge is the execution surface for capabilities.** JP specified the saga pattern for atomic multi-service actions (order Uber Eats + block calendar + notify Slack + text attendees). That is exactly what enables a capability to execute across the client's tools (Zendesk, Anjule, Go High Level, carrier APIs). The action bridge is the plumbing. The capability is the thing the client pays for.

**Why this framing matters internally.** If the team reads JP's PRD and thinks "that is what we are building" and then reads the strategy and thinks "that is what we are selling," they will see a split-brain company. It is not split. JP specified the engine. The strategy specified the vehicle. The engine without the vehicle never gets to market. The vehicle without the engine cannot move.
