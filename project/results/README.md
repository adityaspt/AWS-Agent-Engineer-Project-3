# Project 3 - NovaMart Multi-Agent Customer Support: Results

Multi-agent customer support system built with the Strands Agents SDK and Amazon Bedrock AgentCore
(Orchestrator -> Inventory / Policy / Refund / Communication agents), deployed to AgentCore Runtime
with a Bedrock Guardrail, AgentCore Memory, three Bedrock Knowledge Bases and CloudWatch / X-Ray observability.

## Screenshots and what they show

| File | What it shows | Rubric item |
|---|---|---|
| `Test_agent_1.png` - `Test_agent_6.png` | Full output of `python tests/test_agent.py all` (Tasks 2-6), ending with **Score: 120/120 (100%)** | All `test_agent.py` tasks pass |
| `NovaMart_Orchestrator_xrayTraceMap.png` | CloudWatch Trace Map (X-Ray) after `python src/agent_orchestrator.py test`: NovaMart-Orchestrator connected to InventoryAgent, RefundAgent, PolicyAgent, CommunicationAgent and the three KnowledgeBase nodes (returns, shipping, warranty), all OK 100% | Distributed tracing / Service Map |
| `Guardrail_blocked_negotiation.png` | Pricing-negotiation request blocked by the guardrail | Guardrail topic policy |
| `Guardrail_blocked_legal_threat.png` | Legal-threat request blocked by the guardrail | Guardrail topic policy |

Guardrail check (run in `chat` mode): the negotiation, competitor-comparison and legal-threat examples were all blocked with the
friendly blocked message, and the math question ("5 items at $29.99 with 10% off") was allowed through to the CommunicationAgent.

## Deployed resources (us-east-1)

| Resource | Value |
|---|---|
| AgentCore Runtime ARN | `arn:aws:bedrock-agentcore:us-east-1:048240463439:runtime/udacity_agentcore_runtime-8zMgVXDWK3` |
| Guardrail | `hpwwludds48e`, published version `1` |
| AgentCore Memory | `udacity_agentcore_memory-7DMLbyDFdF` (session summary strategy, 7-day expiry) |
| Returns KB | `K5RAJEEWGV` |
| Shipping KB | `NN6IAFYSYI` |
| Warranty KB | `FSUC4GJGIB` |

These values are also in `starter/.env`.

## Changes to course-supplied files

`src/agent_orchestrator.py` is the main implementation file (all TODOs for Tasks 2, 3, 4 and 6 completed).

One change was made to a provided helper: `cli_path()` in `src/agentcore_cli.py` now looks for `agentcore.cmd` first
(`shutil.which('agentcore.cmd') or shutil.which('agentcore')`). On Windows, the npm-installed `agentcore` launcher without an
extension is a shell script that Python cannot execute (WinError 193); the `.cmd` launcher works. On Linux/macOS behavior is unchanged.
No other provided files were modified.
