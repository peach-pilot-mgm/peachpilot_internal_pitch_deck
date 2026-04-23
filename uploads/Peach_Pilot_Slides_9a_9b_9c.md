# Peach Pilot Pitch Deck: Slides 9a, 9b, and 9c

**Purpose:** Three technical architecture slides that explain what JP has built on the platform side.

**Audience:** Internal deck for new hires and team members. Engineers will want the depth. Non-engineers should be able to follow the headlines and closing lines.

**Brand colors:** Navy #1C1629, Salmon #F08A6A (accent), Gray #79858D, White #FFFFFF.
**Typography:** DM Sans (headings), Inter (body).
**Writing rules:** No em dashes. Direct, short sentences.

---

## Slide 9a: The Agent Hierarchy

**Headline:** 82 agents. Organized like a company.

**Subhead:** Five layers with bounded autonomy calibrated to risk. Each agent has a role, a model tier, and a human oversight model.

**The five layers:**

| Layer | Count | Role | Oversight |
|---|---|---|---|
| **Advisory** | 8 | Strategic counsel on specialized domains. Recommend only, never act. | On-demand consultation |
| **Executive** | 5 | C-suite decision-making, cross-functional coordination | Board approval for major decisions |
| **Business Function** | 13 | VP-level department leadership | Executive review |
| **Production** | ~50 | Individual contributor work | Manager review, human in the loop for externals |
| **Meta** | 9 | Platform operations: routing, security, governance, learning | Automated plus human audit |

**Key principle:**
Humans stay accountable for financial, legal, security, customer-facing, and high-risk actions. Always. No exceptions based on confidence or track record.

**Closing line:**
Agents mirror how companies actually work. Layered authority, bounded autonomy, clear accountability.

### Speaker Notes

**Opening frame.** Most AI companies build a single agent and call it a product. JP built an organizational structure. 82 agents arranged like the departments of a real company. Each agent has a defined scope, a manager, a budget, error targets, and escalation rules. The hierarchy is the product.

**Advisory layer (8 agents, Premium tier).** Strategic advisors that provide domain expertise on demand. They never take autonomous action. They only respond to queries from Executive and Business Function agents. Strategic Advisor, Financial Advisor, Technology Advisor, Market Advisor, Operations Advisor, People and Culture Advisor, Legal Advisor, Innovation Advisor. They have read access to the full knowledge graph but no write access.

**Executive layer (5 agents, Premium tier).** CEO Agent, CTO Agent, CRO Agent, CFO Agent, General Counsel Agent. C-suite decision-making and resource allocation. They can delegate to any Business Function or Production agent. Major decisions (budget over $10K, headcount changes, strategic pivots) require human board approval.

**Business Function layer (13 agents, Standard tier).** VP-level. VP Product, VP Sales, VP Marketing, VP Client Solutions, VP Finance, VP HR, VP Legal, VP IT, VP Operations, VP Creative, VP Communications, VP Engineering, and a COO/President agent. Each owns a knowledge graph slice and manages Production agents within their domain.

**Production layer (around 50 agents, Standard/Budget tier).** Individual contributors. SDR, AE, Sales Ops, Frontend Eng, Backend Eng, DevOps, QA, AP/AR, FP&A, Recruiter, Contract Review, Help Desk, Security Ops, and more. Narrow scopes, defined tool access. Cannot delegate, only execute and report.

**Meta layer (9 agents).** Platform-level agents that make the hierarchy function. Router (classifies incoming requests, picks the right agent chain), Security Guardian, Governance Agent, Quality Sentinel, Knowledge Curator, Performance Monitor, Behavioral Intelligence Agent, Integration Hub, Agent Factory (creates new agents on demand).

**Why the structure matters.** A flat pool of agents does not scale. Humans cannot monitor 82 agents directly. The hierarchy routes work, escalates exceptions, and keeps humans in the loop at the right altitude. VPs manage Production agents. Executives manage VPs. Humans manage Executives on high-stakes calls. The structure is what makes the system operable.

**Error targets by layer.** Advisory under 1%. Executive under 2%. Business Function under 3%. Production under 5%. Meta under 1%. These are not wishes. They are measured and enforced by the Quality Sentinel agent.

**Work-in-progress limits.** Every agent has a maximum concurrent task count. Default 3 for Production, 5 for Business Function, unlimited for Advisory (they only recommend, they don't execute). Lean single-piece flow. Agents pull work from queues rather than having work pushed at them.

---

## Slide 9b: The Platform Stack

**Headline:** Not a slide deck. A running system.

**Subhead:** Production infrastructure deployed on GCP, verified end to end, running today.

**What's live:**

- **Memgraph.** Graph database for the Knowledge Library. Two GCE VMs. Cypher queries power context retrieval and agent reasoning.
- **Qdrant.** Vector database for semantic search and embeddings.
- **PCMS on Cloud Run.** Personal Context Management Service. FastAPI, v4.0.0.
- **Management API on Cloud Run.** Tenant creation, API keys, JWT auth. Full chain verified.
- **LiteLLM proxy.** Unified access to frontier models (Claude, GPT, Gemini).
- **Firebase dashboard.** OAuth working, multi-tenant isolated (44 tests pass).
- **Agent runtime.** POST /agents/execute returns 202 async, writes to Memgraph.
- **Nango on Cloud Run.** Self-hosted integration layer with 700+ connectors.

**The model tier system:**

| Tier | Models | Used for |
|---|---|---|
| **Premium** | Claude Opus 4.6, GPT-5.4 Thinking, Gemini 3.1 Pro | Advisory, Executive, Router, Security |
| **Standard** | Claude Sonnet 4.6, GPT-5.4, Gemini Flash | VPs, most Production agents |
| **Budget** | Claude Haiku 4.5, GPT-4.1, Gemini Flash | Batch work, templates, notifications |

**Closing line:**
Agents reason over real data in real infrastructure. Not mockups. Not diagrams. Production.

### Speaker Notes

**Opening frame.** A lot of AI startups have impressive slides and nothing running. JP built the opposite. There is a working system in production today. Engineers joining Peach Pilot are not starting from a whiteboard. They are building on top of verified infrastructure.

**Memgraph (the knowledge graph).** This is the storage layer of the Company Brain. Graph database, Cypher query language. Two Google Compute Engine VMs, firewalled, running on GCP. Every entity (Person, Meeting, Task, Document, Project, Decision, Preference) and every relationship (KNOWS, OWNS, ATTENDED, MENTIONS, DEPENDS_ON, INTERESTED_IN, PREFERS, RELATES_TO) lives here. The reasoning engine queries this graph to produce grounded answers.

**Qdrant (the vector store).** Handles semantic search and embeddings. When an agent needs to find relevant context by meaning rather than exact match, Qdrant is the layer that makes it work. Memgraph and Qdrant work together: graph traversal for structured relationships, vectors for semantic similarity.

**PCMS and Management API (the services).** PCMS (Personal Context Management Service) is the main API, running FastAPI on Cloud Run. The Management API handles tenant creation, API keys, and JWT authentication. Multi-tenant isolation is verified with 44 passing tests. A new client can be provisioned without touching the platform.

**LiteLLM proxy (the model layer).** Unified access to frontier models from Anthropic, OpenAI, and Google. We are model-agnostic by design. If Claude gets better, we use Claude. If GPT gets better, we use GPT. The Router agent picks the best model for each task based on cost, latency, and capability.

**Nango (the integration layer).** This is the piece that makes data ingestion tractable. Instead of building OAuth flows for every source system (Gmail, Outlook, Calendar, Teams, JIRA, Confluence, Asana, Salesforce, and on and on), we use Nango. 700+ connectors, hosted OAuth flows, token lifecycle management, bulk historical sync, incremental updates. Self-hosted on Cloud Run. This is why Sprint 1 of the build plan can deliver data flowing into Memgraph in two weeks instead of two quarters.

**Model tier economics.** Every query is routed to the cheapest model that can do the job well. Advisory and Executive get Premium because the decisions are high-stakes. VPs get Standard with Router boost to Premium for complex cases. Production agents mostly run Standard. Batch and template work (AP/AR, ETL, notifications) runs on Budget. This is how the unit economics work at scale.

**What is not on the slide.** There is more: circuit breakers for token budgets and cascade detection, atomic checkout to prevent duplicate agent work, agent CLI for operations, real-time dashboard showing heartbeats and error rates. The goal of the slide is to prove the platform is real, not to list everything.

---

## Slide 9c: How the System Learns

**Headline:** Three patterns that make agents smarter over time.

**Subhead:** The platform is not static. Every action generates a learning signal. Every successful pattern crystallizes into reusable knowledge.

**The three patterns:**

**1. Hermes: the reinforcement learning loop**
Every agent action produces an outcome. Every outcome produces a reward signal. Every signal updates the agent's behavior on the next heartbeat. Agents get better at their job through repetition.

**2. Paperclip: inter-agent coordination**
Agents collaborate through structured messages, not ad-hoc chat. Five canonical schemas govern the conversation: status updates, escalations, approval requests, governance violations, collaboration queries. Coordination is deterministic and auditable.

**3. Autoresearch: recursive self-improvement**
Agents hypothesize improvements to their own behavior. They backtest the hypothesis against historical cases in the knowledge graph. If it works in backtest, they propose the change to their manager. If approved, the change deploys and is monitored for 20 live actions. Learning is governed.

**Skill crystallization:**
When an agent's approach succeeds on 20+ instances with an 80%+ approval rate, the pattern is extracted into a reusable skill. Other agents can inherit it. Cross-department skills get promoted by the Knowledge Curator.

**Closing line:**
Day one agents are adequate. Day 180 agents are expert. The system compounds.

### Speaker Notes

**Opening frame.** Static AI tools hit a ceiling fast. Once you build a prompt, it works or it doesn't. There is no path to "better." JP's architecture is different. Three learning patterns work together to make the system smarter every day. This is what the MIT NANDA report on the 95% failure rate was pointing at: most enterprise AI tools do not learn. Ours do.

**Hermes pattern in detail.** Named after the Hermes skill_manager pattern in JP's codebase. Every agent action is stored in Memgraph as an AgentAction node. When the outcome is observed (human approved, task completed, KPI moved, peer feedback received), an Outcome node is created and linked. A LearningSignal is generated with a reward score. The agent's model weighting, skill effectiveness scores, and confidence thresholds all update on the next heartbeat. Four reward sources with defined weights: human approval (0.4), task outcome (0.3), metric change (0.2), peer feedback (0.1). Humans rejecting a proposal is a strong negative signal. Humans approving unchanged is a strong positive signal. Over time, each agent learns what its humans want.

**Paperclip pattern in detail.** Named after the Paperclip coordination framework in JP's codebase. Agents do not free-form chat with each other. They communicate through five canonical message types. A status update reports progress. An escalation flags something that needs manager attention. An approval request asks for permission to act. A governance violation alerts the Governance Agent. A collaboration query asks a peer for help. Every message has required fields, a confidence score, and a logged outcome. Coordination is auditable end to end. This is how 82 agents can work together without turning into chaos.

**Autoresearch pattern in detail.** Karpathy-inspired recursive self-improvement. An agent notices something like "including competitor context in deal assessments increases approval rate." It formulates a hypothesis: if I add a competitor analysis section, approval rate goes from 85% to 90%. It runs the modified approach against 10 historical cases pulled from Memgraph (backtesting). If the backtest shows improvement, the agent proposes the change via an approval request to its manager. If the manager approves, the change is deployed and monitored for 20 live actions. If live performance confirms the improvement, the change becomes permanent. If too many low-quality changes get proposed, the agent's experiment mode gets disabled. Learning is continuous but governed.

**Skill crystallization.** This is how individual agent learning becomes system-wide learning. When an agent has an approach that achieves 80% plus approval rate across 20 plus instances, the Quality Sentinel detects the pattern. The Knowledge Curator extracts it into a structured skill (trigger conditions, reasoning chain, output template, success criteria). The skill is stored with its effectiveness score, source actions, and usage count. If it generalizes across departments, it gets promoted to a shared skill. By year one, the system has dozens of auto-crystallized skills that no human wrote.

**Why this matters commercially.** The learning architecture is the technical foundation of the flywheel on slide 7. Engagement one gets adequate agents. Engagement five gets agents with crystallized skills from engagement one. Engagement ten gets agents that proactively propose improvements. The reason the tenth engagement costs a fraction of the first is not because we wrote more code. It is because the agents wrote the code equivalent themselves, in the form of skills, over the previous nine engagements.

**The governance boundary.** This is critical. Agents do not self-modify without approval. Every hypothesis is logged. Every backtest result is logged. Every proposed change requires human approval at the manager layer. The learning loop is transparent and auditable. The only way to trust a self-improving system is to watch it improve in the open.
