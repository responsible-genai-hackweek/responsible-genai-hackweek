# Glossary

## Foundational LLM and Agent Concepts

| Term                                              | Definition                                                                                                                                                                                   |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Large language model (LLM)                        | A statistical model trained on huge amounts of text that generates responses by predicting the most likely next chunk of text given what came before.                                        |
| ---                                               | ---                                                                                                                                                                                          |
| Token                                             | The unit an LLM works in — roughly three-quarters of a word, or a few characters of code — used to measure context, cost, and speed.                                                         |
| ---                                               | ---                                                                                                                                                                                          |
| Context window                                    | The total amount of text (measured in tokens) that a model can consider at once, including your prompt, prior turns, and any files it has read.                                              |
| ---                                               | ---                                                                                                                                                                                          |
| Pre-training                                      | The initial training phase that makes a raw LLM fluent at predicting text, but does not yet make it useful as an assistant.                                                                  |
| ---                                               | ---                                                                                                                                                                                          |
| Post-training                                     | The subsequent training phases (SFT, RLHF, tool-use fine-tuning) that turn a fluent LLM into a helpful, instruction-following assistant.                                                     |
| ---                                               | ---                                                                                                                                                                                          |
| Supervised fine-tuning (SFT)                      | A post-training phase in which the model learns from human-written examples of ideal user-and-assistant exchanges.                                                                           |
| ---                                               | ---                                                                                                                                                                                          |
| Reinforcement learning from human feedback (RLHF) | A post-training phase in which the model learns from human preference judgments about which of two responses is better.                                                                      |
| ---                                               | ---                                                                                                                                                                                          |
| Tool-use fine-tuning                              | A post-training phase that teaches the model when and how to call external tools rather than just talking about them.                                                                        |
| ---                                               | ---                                                                                                                                                                                          |
| Transformer                                       | The neural network architecture used by all modern LLMs, introduced by Google researchers in 2017.                                                                                           |
| ---                                               | ---                                                                                                                                                                                          |
| Attention                                         | The mechanism inside a Transformer that lets the model weigh the relevance of every part of its input when generating each next token — the reason context engineering works at all.         |
| ---                                               | ---                                                                                                                                                                                          |
| Chat interface                                    | A conversational interface to an LLM where the model responds but cannot take actions in the world.                                                                                          |
| ---                                               | ---                                                                                                                                                                                          |
| Agent                                             | An LLM plus a surrounding system that lets it take actions — reading files, running commands, calling APIs — in a loop, until a task is done.                                                |
| ---                                               | ---                                                                                                                                                                                          |
| Agent loop                                        | The cycle of sending a conversation to the model, executing any tools it requests, feeding results back, and repeating until the model has no more tool calls to make.                       |
| ---                                               | ---                                                                                                                                                                                          |
| Harness                                           | The software wrapper around an LLM that manages tools, memory, permissions, and the agent loop; the same model behaves differently in different harnesses.                                   |
| ---                                               | ---                                                                                                                                                                                          |
| Interface                                         | What the researcher actually sees and interacts with — a chat window, a command-line tool, an IDE panel — as distinct from the model and the harness underneath.                             |
| ---                                               | ---                                                                                                                                                                                          |
| System prompt                                     | An invisible instruction the harness prepends to every conversation to shape the model's behavior.                                                                                           |
| ---                                               | ---                                                                                                                                                                                          |
| Statelessness                                     | The fact that models have no memory between conversations; anything that looks like memory is the harness re-loading context on each turn.                                                   |
| ---                                               | ---                                                                                                                                                                                          |
| Convergent post-training                          | The observation that different labs (Anthropic, OpenAI, Google, Meta) use similar training pipelines and their models therefore behave similarly, which is why swapping models mostly works. |
| ---                                               | ---                                                                                                                                                                                          |

## Context, Skills, Rules, and Related Authoring Surfaces

| Term                   | Definition                                                                                                                                                             |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Context engineering    | The practice of deliberately shaping what information the agent has access to within its context window, in order to control the quality of its output.                |
| ---                    | ---                                                                                                                                                                    |
| Prompt                 | The text you send to the model, including instructions, examples, and any context you want it to consider.                                                             |
| ---                    | ---                                                                                                                                                                    |
| Project memory         | A file the harness automatically loads at the start of every session so the agent knows about your project without being retold.                                       |
| ---                    | ---                                                                                                                                                                    |
| AGENTS.md              | The emerging cross-vendor convention for the project memory file, containing project structure, conventions, and domain context, like a README for the project         |
| ---                    | ---                                                                                                                                                                    |
| CLAUDE.md              | Claude Code's specific project memory file; functionally equivalent to AGENTS.md in other tools. For other platforms: GEMINI.md                                        |
| ---                    | ---                                                                                                                                                                    |
| Skill                  | A reusable, named procedure the agent can invoke on demand when a task matches its description.                                                                        |
| ---                    | ---                                                                                                                                                                    |
| SKILL.md               | The markdown file convention for defining a skill, with a description at the top that acts as the agent's selector and a body containing the procedure.                |
| ---                    | ---                                                                                                                                                                    |
| Rule                   | A path-scoped constraint that applies when the agent is editing files matching a specific pattern.                                                                     |
| ---                    | ---                                                                                                                                                                    |
| Subagent               | A specialized version of the agent with a distinct persona and often restricted tool access, used when different permissions or a different voice are needed.          |
| ---                    | ---                                                                                                                                                                    |
| Slash command          | A user-typed shortcut (like /clear or /init) that triggers a specific harness behavior.                                                                                |
| ---                    | ---                                                                                                                                                                    |
| Progressive disclosure | The design principle of showing a short summary up front and revealing full detail only when it becomes relevant — how skills and nested context files are structured. |
| ---                    | ---                                                                                                                                                                    |
| Handoff artifact       | A compacted document that summarizes a session's state so a fresh session can pick up where the last one left off.                                                     |
| ---                    | ---                                                                                                                                                                    |

## Model Context Protocol (MCP)

| Term                         | Definition                                                                                                                                                         |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Model Context Protocol (MCP) | A standardized protocol that lets agents connect to external tools and data sources without custom integration per vendor.                                         |
| ---                          | ---                                                                                                                                                                |
| MCP server                   | A separate service that exposes tools, resources, and prompts through the MCP protocol so any MCP-compatible agent can use them.                                   |
| ---                          | ---                                                                                                                                                                |
| MCP client                   | The side of the protocol that lives inside the agent's harness and talks to MCP servers on the agent's behalf.                                                     |
| ---                          | ---                                                                                                                                                                |
| MCP tool                     | A callable function exposed by an MCP server that the agent can invoke — one of the three MCP primitives.                                                          |
| ---                          | ---                                                                                                                                                                |
| MCP resource                 | Structured data exposed by an MCP server that the agent can read — a second MCP primitive.                                                                         |
| ---                          | ---                                                                                                                                                                |
| MCP prompt                   | A reusable prompt template exposed by an MCP server, often used to guide the agent's workflow — the third MCP primitive.                                           |
| ---                          | ---                                                                                                                                                                |
| Tool description             | The natural-language explanation of what a tool does, which the agent reads to decide whether to call it — arguably the most important piece of MCP server design. |
| ---                          | ---                                                                                                                                                                |
| Streamable HTTP transport    | The MCP transport mechanism used by remotely hosted servers, allowing agents to communicate with them over HTTP.                                                   |
| ---                          | ---                                                                                                                                                                |
| STDIO transport              | An MCP transport mechanism where the server runs as a local process and communicates through standard input/output — common for local development.                 |
| ---                          | ---                                                                                                                                                                |
| MCP Inspector                | A browser-based tool for testing MCP servers by connecting to them, listing their tools, and invoking them interactively.                                          |
| ---                          | ---                                                                                                                                                                |

## Sandboxing, Security, and Permissions

| Term               | Definition                                                                                                                                                      |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sandbox            | An environment that hosts a workspace and limits what an agent can touch — restricting file access, network calls, and command execution to a defined boundary. |
| ---                | ---                                                                                                                                                             |
| Proceed-in-sandbox | The pattern of auto-approving agent actions inside a sandbox boundary while requiring confirmation to cross it.                                                 |
| ---                | ---                                                                                                                                                             |
| Approval habit     | The discipline of reading what the agent proposes to do before accepting it, especially for file edits and shell commands.                                      |
| ---                | ---                                                                                                                                                             |
| Prompt injection   | An attack in which instructions hidden inside content the agent reads (a file, a webpage, a data cell) hijack the agent's behavior.                             |
| ---                | ---                                                                                                                                                             |
| Read-only skill    | A skill whose prose constraints tell the agent to inspect but not modify — a soft, description-based constraint on behavior.                                    |
| ---                | ---                                                                                                                                                             |
| Credential hygiene | The practice of keeping secrets, tokens, and passwords out of any context an agent might see.                                                                   |
| ---                | ---                                                                                                                                                             |

## Data, Privacy, and Institutional Practice

| Term             | Definition                                                                                                                                     |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Consumer tier    | The free or personal-paid access to a model where prompts are often logged and may be used to train future models.                             |
| ---              | ---                                                                                                                                            |
| Enterprise tier  | Paid institutional access to a model with contractual guarantees that prompts are not logged or used for training.                             |
| ---              | ---                                                                                                                                            |
| API access       | Programmatic access to a model, typically billed by token, often with the same privacy guarantees as enterprise tiers.                         |
| ---              | ---                                                                                                                                            |
| Local model      | A model running entirely on your own hardware, so no data leaves your machine at the cost of typically lower capability.                       |
| ---              | ---                                                                                                                                            |
| LLM gateway      | An institution-level proxy that routes agent requests through a controlled endpoint for authentication, budget enforcement, and audit logging. |
| ---              | ---                                                                                                                                            |
| Redaction        | The practice of stripping sensitive fields from data before sending it to an agent so the agent can help without seeing the sensitive parts.   |
| ---              | ---                                                                                                                                            |
| Synthetic sample | A fabricated example that preserves the shape of real data, used in place of real data when the actual values are sensitive.                   |
| ---              | ---                                                                                                                                            |

## Verification, Testing, and Reproducibility

| Term               | Definition                                                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Verification       | The process of checking that an agent's output is actually correct, especially important because plausible-sounding outputs can be silently wrong.      |
| ---                | ---                                                                                                                                                     |
| Eval               | A scored test suite that measures how well an agent or skill performs on a domain-specific task, distinct from unit tests of the code the agent writes. |
| ---                | ---                                                                                                                                                     |
| LLM-as-judge       | The pattern of using one model to evaluate another model's output against a rubric — the mechanism behind most modern eval pipelines.                   |
| ---                | ---                                                                                                                                                     |
| Hallucination      | A confident-sounding but incorrect or fabricated output — a common failure mode, especially in domains underrepresented in training data.               |
| ---                | ---                                                                                                                                                     |
| Provenance chain   | The trail from prompt to AI response to human review to committed artifact that documents how AI-assisted work was produced.                            |
| ---                | ---                                                                                                                                                     |
| Auditability trail | Per-phase markdown artifacts (plans, analyses, validations) committed alongside code that document why decisions were made.                             |
| ---                | ---                                                                                                                                                     |
| Determinism        | The property of getting the same output for the same input; not a default for LLMs but achievable via API parameters like temperature and seed.         |
| ---                | ---                                                                                                                                                     |
| Model drift        | The phenomenon of a model producing different outputs over time as vendors update the underlying model, breaking reproducibility across time.           |
| ---                | ---                                                                                                                                                     |

## Retrieval and Grounding

| Term                                 | Definition                                                                                                                           |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| Retrieval-augmented generation (RAG) | An approach that gives the model access to your specific documents by retrieving relevant chunks and inserting them into the prompt. |
| ---                                  | ---                                                                                                                                  |
| Embedding                            | A numerical representation of text that captures its meaning in a way that can be compared to other text for similarity.             |
| ---                                  | ---                                                                                                                                  |
| Vector database                      | A specialized database that stores embeddings and lets you efficiently find text similar to a query.                                 |
| ---                                  | ---                                                                                                                                  |
| Chunking                             | The process of splitting long documents into smaller pieces that can be individually retrieved and inserted into a prompt.           |
| ---                                  | ---                                                                                                                                  |
| Semantic search                      | A search approach that finds documents by meaning rather than exact word matching, powered by embeddings.                            |
| ---                                  | ---                                                                                                                                  |
| Knowledge cutoff                     | The date after which the model was not trained on new information, meaning it does not know about later events.                      |
| ---                                  | ---                                                                                                                                  |
| Grounding                            | Anchoring an agent's response in specific documents or data sources you provide, rather than relying on its training knowledge.      |
| ---                                  | ---                                                                                                                                  |

## Workflow Patterns

| Term                          | Definition                                                                                                                                                                       |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Research/Plan/Implement (RPI) | A workflow pattern that separates a task into a research phase, a planning phase, and an implementation phase, with an auditable artifact after each.                            |
| ---                           | ---                                                                                                                                                                              |
| Explore/Plan/Execute (EPE)    | A variant of RPI with slightly different phase names, used in some ecosystems.                                                                                                   |
| ---                           | ---                                                                                                                                                                              |
| Spec-driven development       | Writing a specification document before writing code and using the agent to critique and refine it.                                                                              |
| ---                           | ---                                                                                                                                                                              |
| Vibe coding                   | The informal pattern of prompting an agent conversationally and accepting what it produces without structured discipline — contrasted with workflow-driven approaches.           |
| ---                           | ---                                                                                                                                                                              |
| Trained in vs. in the prompt  | The mental model that distinguishes what the model already knows from training (which you can't change) from what you provide in the current session (which is your only lever). |
| ---                           | ---                                                                                                                                                                              |

## Bias, Ethics, and Failure Modes

| Term                   | Definition                                                                                                                                                 |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sycophancy             | The tendency of models to agree with the user's framing rather than push back, a systematic side-effect of preference-based training.                      |
| ---                    | ---                                                                                                                                                        |
| Context exhaustion     | The failure mode where an agent forgets earlier parts of a conversation as the context window fills up.                                                    |
| ---                    | ---                                                                                                                                                        |
| Confident wrong answer | The failure mode where an agent produces a plausible-sounding but incorrect result, made worse by RLHF training that rewards confident-sounding responses. |
| ---                    | ---                                                                                                                                                        |
| Reward hacking         | A trained-in tendency for models to produce outputs that score well on their training reward without actually being helpful or correct.                    |
| ---                    | ---                                                                                                                                                        |
| Scope creep            | The failure mode where an agent does more than it was asked, a consequence of training that rewards being helpful.                                         |
| ---                    | ---                                                                                                                                                        |