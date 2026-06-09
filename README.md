# harness-engineering


## related linnks
OpenAI:
- Harness engineering: leveraging Codex in an agent-first world
https://openai.com/index/harness-engineering/

Anthropic:
- Effective harnesses for long-running agents
https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
- Harness design for long-running application development
https://www.anthropic.com/engineering/harness-design-long-running-apps

LangChain:
- The Anatomy of an Agent Harness
https://www.langchain.com/blog/the-anatomy-of-an-agent-harness

Mitchell Hashimoto:
- My AI Adoption Journey
https://mitchellh.com/writing/my-ai-adoption-journey

martinfowler.com:
- Harness Engineering - first thoughts
https://martinfowler.com/articles/exploring-gen-ai/harness-engineering-memo.html
- Harness engineering for coding agent users
https://martinfowler.com/articles/harness-engineering.html

## Harness engineering 
https://mp.weixin.qq.com/s/JPhcyDc4JwRmnMQ-76A-FQ


### Defination 
AI Harness Engineering refers to building a complete set of standardized, enforceable runtime guardrails, validation, governance and operational systems wrapped around LLMs and Agents to deliver stable, secure, compliant and cost-effective automated business execution at enterprise scale.



### III. The 11 Core Components of a Production-Grade Harness
1. Orchestration & Scheduling Layer (Orchestration Loop, The Core Heart)
Runs the standard TAO (Thought-Action-Observation) ReAct loop:
Assemble the complete prompt package for the current round.
Call the LLM to get thoughts + tool call instructions.
Parse the output and validate format legality.
Execute tool actions (read/write, API calls, code execution).
Fill the execution results back into the context, looping until the task is complete.
Additional Capabilities: Task decomposition, parallel distribution to sub-agents, breakpoint resumption, timeout circuit breaking, and degraded backup model routing.
2. Constraint & Behavior Layer (Guardrails & Rule Docs)
AGENTS.md: The AI behavior manual containing hard rules (prohibited operations, coding standards, output formats, business boundaries), which the model is forced to read on startup.
A static system prompt repository and a versioned prompt registry.
Input/output filters: sensitive word masking, strong format validation (JSON Schema/Zod validation), and permission whitelists.
3. Context Management Layer (Context Architecture)
A complete set of engineering strategies to solve the context window bottleneck:
Context Compaction: Automatic summarization of old dialogues to reduce token usage.
Just-in-time Retrieval: The vector database only recalls task-essential fragments, avoiding loading the entire knowledge base.
Context Isolation: Sub-agents have independent contexts and do not interfere with each other.
History Layering: Three-tier storage for short-term dialogue memory, long-term task state (persisted on disk), and a global knowledge base vector database.
4. Tool & Sandbox Layer (Tool + Sandbox Runtime)
Empowering the model with controllable hands-on capabilities:
Standardized Toolset: File read/write, shell execution, unit testing, API requests, browser crawling, SQL queries.
Isolated Sandbox: Container/virtual environment that restricts file read/write scope, network access, and command permissions to prevent database deletion and unauthorized damage.
Strong validation of tool input parameters, full retention of call logs, and automatic retry/rollback on failure.
5. State Persistence Layer (State & Memory)
Solving the AI "amnesia" problem:
PROGRESS.md task progress file, Git change snapshots.
Structured task database: Records steps, completion status, intermediate products, and error logs.
Long-term memory vector database, session snapshots, and breakpoint recovery points.
Upon task completion, archive the complete runtime trace package for full process reproducibility.
6. Self-Verification Loop Layer
Eliminating hallucinations and errors, a core error correction mechanism as defined in Hashimoto:
The Agent completes a segment of output/code/report.
The Harness automatically triggers verification units: unit tests, linting, business rule checks, numerical verification, and fact-checking against retrieved sources.
If verification fails, it forces a rollback, rethinks, and regenerates. Multiple failures trigger human intervention.
All error cases are stored in a database to iteratively optimize constraints and prompts, permanently avoiding similar issues.
7. Security & Access Control Layer
Fine-grained RBAC permissions: Different Agents can only access specified files/databases/APIs.
Data desensitization: Automatic masking of phone numbers, secrets, and confidential business data in inputs and outputs.
Entropy Governance: Monitoring the model's random divergence behavior; excessive fluctuations force convergence.
High-risk action secondary confirmation mechanism (manual approval gate required for table deletion, production changes).
8. Observability & Logging Layer
Full-link auditability and troubleshooting:
Complete logs of every round of LLM input parameters, output parameters, tool calls, latency, and token consumption.
Error attribution tags: Differentiate between model hallucinations, tool malfunctions, insufficient context, and missing rules.
Performance metric monitoring: Inference latency, failure rate, number of retries, context expansion rate.
A/B testing instrumentation: Compare task success rates with different prompt/constraint/tool configurations.
9. Model Gateway & Routing Layer (Model Gateway)
Decoupling models from business to achieve pluggable model replacement:
Unified API adapter for GPT/Claude/Mistral/local open-source large models.
Load balancing, rate limiting, and caching of common inference results.
Automatic failover: Primary model timeouts/errors automatically switch to a backup model.
Cost control: Use lightweight small models for simple tasks, and high-end large models for complex reasoning.
10. Evaluation & Iteration Layer (Evaluation & Meta-Harness)
Continuously iterating and optimizing the Harness itself:
Standardized evaluation dataset to batch-run tasks and quantify success rate, accuracy, and compliance rate.
Meta-Harness (Harness of Harnesses): Using AI to automatically optimize prompts, constraint rules, and tool logic.
Version control: Harness configurations, prompts, and validation rules are all Git-versioned for risk-free rollbacks.
11. Deployment & Serving Layer (Serving & Lifecycle)
Containerized deployment, elastic scaling, environment isolation (three independent Harness configurations for development/testing/production), supporting multi-form factor operation such as API services, CLI local agents, and IDE plugins.
