# Challenge 04 - Deploy a Hosted Weather Agent on Foundry

[< Previous Challenge](./Challenge-03.md) - **[Home](../README.md)** - [Next Challenge >](./Challenge-05.md)

## Pre-requisites

- Complete [Challenge 03](./Challenge-03.md) — you should be comfortable creating agents with the SDK, connecting tools, and testing in the Foundry portal.

## Introduction

In the previous challenges you built agents that are defined entirely by prompts, model configurations, and tool connections — whether through the portal or the SDK. These are **prompt-based agents**. They work well for many scenarios, but what happens when you need full control over the runtime? What if your agent is built with a different framework like LangGraph, Semantic Kernel, or CrewAI? What if you need custom protocols, stateful compute, or code that can't be expressed as a system prompt plus tools?

This is where **Hosted Agents** come in. Hosted agents are containerized agentic AI applications that run on Microsoft-managed infrastructure inside Foundry Agent Service. You write the agent code — using any framework you choose — package it in a container, and deploy it. Foundry handles the rest: compute provisioning, scaling, security, a dedicated Microsoft Entra ID identity, built-in observability via Application Insights, session persistence, and lifecycle management.

The key difference from previous challenges: you are no longer just _configuring_ an agent through the SDK — you are writing the actual **runtime code** that handles requests, calls models, and returns responses. Your code is the agent.

**Why this matters for production:** In the real world, teams build agents using different frameworks and languages. A company might have one team using LangGraph, another using Semantic Kernel, and a third with a custom orchestrator. Hosted agents provide a single deployment target — Foundry — regardless of how the agent was built. Every hosted agent gets its own endpoint, its own identity, and full observability — with zero infrastructure management on your part. This is the pattern for taking agents from prototype to production at scale.

In this challenge, you will build a **Weather Agent** using the **Agent Framework SDK** with a custom weather tool, test it locally, and deploy it as a hosted agent to Foundry Agent Service.

## Description

In this challenge you will create a hosted agent — a containerized Python application that runs on Foundry Agent Service — that answers weather questions using a custom tool. You will scaffold the project, write the agent code, test locally, and deploy to Foundry.

### Part 1: Set Up the Environment

Before building the agent, you need the development tools installed and your Azure environment ready.

- Install the **Azure Developer CLI (`azd`)** version 1.25.0 or later and the AI Agent extension (`azd ext install azure.ai.agents`)
- Authenticate with Azure using `azd auth login`
- Ensure **Docker Desktop** is installed (required for container-based deployment)
- Ensure you have the **Foundry Project Manager** role at project scope in your Foundry project
- Optionally, install the **Microsoft Foundry Toolkit** VS Code extension for an integrated development experience

### Part 2: Scaffold and Build the Weather Agent

Use `azd ai agent init` or the VS Code Foundry Toolkit to scaffold a hosted agent project. Then modify the generated code to create your Weather Agent.

Your project should contain the following files:

- **`main.py`** — the agent code that:
  - Authenticates using `DefaultAzureCredential` (no API keys)
  - Creates a `FoundryChatClient` connected to a `gpt-4.1` model deployment
  - Defines a custom `get_weather` tool using the `@tool` decorator that accepts a `location` parameter and returns weather information (simulated data is fine — the point is the tool integration pattern, not a real weather API)
  - Creates an `Agent` with system instructions defining its persona as a helpful weather assistant
  - Hosts the agent using `ResponsesHostServer` which exposes the Responses protocol on port 8088
- **`agent.yaml`** — the agent definition declaring the `responses` protocol, environment variables (including `AZURE_AI_MODEL_DEPLOYMENT_NAME`), and compute resources
- **`requirements.txt`** — including `agent-framework` and `agent-framework-foundry-hosting`
- **`Dockerfile`** — packaging the agent as a container image
- **`.env`** — local environment variables for your Foundry project endpoint and model deployment name

Your `get_weather` tool should use the `@tool` decorator with type-annotated parameters so the Agent Framework can automatically generate the JSON schema for the LLM. The agent's system instructions should tell it to always mention the location in its response and provide concise, helpful weather summaries.

### Part 3: Test the Agent Locally

Before deploying to the cloud, validate that the agent works on your machine.

- Start the agent locally (it should listen on `http://localhost:8088/`)
- Send test weather queries using `curl`, `azd ai agent invoke --local`, or the **Agent Inspector** in VS Code
- Verify the agent calls the `get_weather` tool and includes weather data in its response
- Test a **multi-turn conversation** — ask about one city, then follow up asking about another, using the `previous_response_id` from the first response
- Verify the agent stays in character and follows its system instructions

### Part 4: Deploy to Foundry Agent Service

Now deploy the agent so it runs as a persistent, hosted endpoint on Foundry.

- Deploy using `azd deploy` (CLI) or **Foundry Toolkit: Deploy Hosted Agent** (VS Code)
- Verify the deployment succeeds and the agent status is **Active**
- Invoke the deployed agent with a weather query and confirm you get a response
- Verify the deployed agent is visible in the Foundry portal under **Build** → **Agents**
- Open the agent in the **Foundry playground** and have a conversation with it

### Key Concepts

- **Hosted Agent**: A containerized agent application running on Foundry-managed infrastructure. Unlike prompt-based agents, you write and own the runtime code — Foundry handles compute, scaling, identity, and observability.
- **Responses Protocol**: The default protocol for conversational hosted agents. The platform manages conversation history, streaming lifecycle, and session state. Any OpenAI-compatible SDK works as a client.
- **Agent Framework**: The recommended Python SDK for building hosted agents. It provides native tool wiring, streaming, session management, and tight Foundry integration via `ResponsesHostServer`.
- **`@tool` Decorator**: Registers a Python function as a tool the agent can call. The framework automatically generates the JSON schema from type annotations and docstrings — the LLM uses this to decide when to invoke the tool.
- **`agent.yaml`**: The agent definition file specifying protocols, environment variables, and compute resources. Read by `azd` and the VS Code extension during deployment.
- **Agent Identity**: Every deployed hosted agent gets a dedicated Microsoft Entra ID created automatically at deploy time. The agent uses this identity to call Foundry models and downstream Azure services — no secrets in your code.
- **Prompt-based vs. Hosted Agents**: Prompt-based agents are defined entirely through configuration (prompts, tools, model connections). Hosted agents are your own code in a container — you control the full runtime behavior and can use any framework.

## Success Criteria

To complete this challenge successfully, you should be able to:

- Show the project structure with `main.py`, `agent.yaml`, `requirements.txt`, and `Dockerfile`
- Demonstrate a `main.py` that creates a Weather Agent with a custom `get_weather` tool using the `@tool` decorator
- Verify the agent authenticates using `DefaultAzureCredential` — no API keys or secrets in the code
- Verify the agent uses the `gpt-4.1` model deployment and has system instructions defining its weather assistant persona
- Show the agent running locally on port 8088 and responding to weather queries
- Demonstrate a multi-turn conversation where the agent handles follow-up questions about different cities
- Show the agent deployed to Foundry Agent Service with an **Active** status
- Invoke the deployed agent via `azd ai agent invoke` or the Foundry playground and receive a weather response
- Verify the deployed agent is visible in the Foundry portal
- Explain to your coach the difference between a prompt-based agent (Challenges 01–03) and a hosted agent, and when you would choose one over the other

## Learning Resources

- [What are Hosted Agents in Foundry Agent Service?](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/hosted-agents)
- [Quickstart: Create and Deploy a Hosted Agent](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent)
- [Deploy a Hosted Agent (Python SDK / REST)](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/deploy-hosted-agent)
- [Agent Framework GitHub Repository](https://github.com/microsoft/agent-framework)
- [Foundry Samples — Hosted Agents (Python)](https://github.com/microsoft-foundry/foundry-samples/tree/main/samples/python/hosted-agents)
- [Basic Hosted Agent Sample (01-basic)](https://github.com/microsoft-foundry/foundry-samples/tree/main/samples/python/hosted-agents/agent-framework/responses/01-basic)
- [Hosted Agent with Tools Sample (02-tools)](https://github.com/microsoft-foundry/foundry-samples/tree/main/samples/python/hosted-agents/agent-framework/responses/02-tools)
- [DefaultAzureCredential documentation](https://learn.microsoft.com/python/api/azure-identity/azure.identity.defaultazurecredential)
- [Microsoft Foundry Toolkit VS Code Extension](https://aka.ms/foundrytk)
- [Azure Developer CLI (azd)](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/)

## Tips

- **Start from the sample.** The [02-tools sample](https://github.com/microsoft-foundry/foundry-samples/tree/main/samples/python/hosted-agents/agent-framework/responses/02-tools) in the `foundry-samples` repo already has a `get_weather` tool — use it as a starting reference and adapt the system instructions for your Weather Agent persona.
- **Use `azd ai agent init` to scaffold.** It generates `agent.yaml`, Bicep files, and deployment config so you can focus on the agent logic instead of boilerplate.
- **Test locally first — always.** Run `python main.py` or `azd ai agent run` and validate with `curl` before deploying. Debugging a container in the cloud is much slower than debugging on your machine.
- **Use the Agent Inspector.** In VS Code, open the Command Palette → **Foundry Toolkit: Open Agent Inspector** to get a chat UI for your local agent — much nicer than `curl` for iterating.
- **Your weather tool can return fake data.** This challenge is about the hosting and deployment pattern, not integrating a real weather API. Returning randomized conditions is perfectly fine.
- **Check agent status after deploying.** Run `azd ai agent show` — if it says `creating`, wait a minute and try again. Provisioning typically takes under a minute.
- **`FOUNDRY_PROJECT_ENDPOINT` is injected automatically** by the platform at runtime — you only need it in your local `.env` for testing. Don't put it in `agent.yaml`.
- **If deployment fails with permission errors,** verify you have the **Foundry Project Manager** role. The `azd` tooling handles most RBAC assignments, but you need this base role.
- **Monitor logs in real time** with `azd ai agent monitor --follow` to see what your agent is doing after deployment.

## Advanced Challenges (Optional)

Too comfortable? Eager to do more? Try these additional challenges!

- **Bring a different framework:** Instead of Agent Framework, build the same Weather Agent using **LangGraph** or your own custom code and deploy it using the Bring Your Own (BYO) pattern with the `azure-ai-agentserver-responses` library. Compare the developer experience.
- **Add a second tool:** Give your Weather Agent a `get_forecast` tool that returns a 5-day forecast in addition to the current weather. Observe how the agent decides which tool to call based on the user's question.
- **Deploy with the Python SDK instead of `azd`:** Use `AIProjectClient.agents.create_version()` with a `HostedAgentDefinition` to deploy programmatically. This is the pattern you would use in a CI/CD pipeline.
- **Publish to the Foundry playground and share the endpoint** with a teammate. Have them invoke your hosted agent from their own code using an OpenAI-compatible client SDK.
