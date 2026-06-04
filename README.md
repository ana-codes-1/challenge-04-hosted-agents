# What The Hack - Hosted Agents on Azure AI Foundry

## Introduction

In the previous challenges you built agents that are defined entirely by prompts, model configurations, and tool connections. These prompt-based agents work well for many scenarios, but what happens when you need full control over the runtime? What if your agent is built with a different framework like LangGraph, Semantic Kernel, or CrewAI? What if you need custom protocols, stateful compute, or code that cannot be expressed as a system prompt plus tools?

Hosted agents are containerized agentic AI applications that run on Microsoft-managed infrastructure inside Foundry Agent Service. You write the agent code using any framework you choose, package it in a container, and deploy it. Foundry handles the rest: compute provisioning, scaling, security, a dedicated Microsoft Entra ID identity, built-in observability, and lifecycle management. This is the pattern for taking agents from prototype to production at scale.

## Learning Objectives

In this hack you will learn how to:

1. Define a custom tool function with typed parameters for a hosted agent
2. Write agent instructions that control persona and behavior
3. Assemble a complete hosted agent application from code components
4. Build a container image using Azure Container Registry (no local Docker needed)
5. Deploy a hosted agent to Foundry Agent Service via the Python SDK
6. Invoke a deployed hosted agent through the OpenAI-compatible Responses API

## Challenges

- Challenge 04: **[Deploy a Hosted Weather Agent](Student/Challenge-04.md)**
   - Build a weather agent with a custom tool, containerize it, deploy to Foundry, and invoke it

**NOTE:** This challenge is part of a larger series. Challenges 00-03 cover prerequisites, portal-based agents, code-based agents, and agent tools respectively. This challenge (04) assumes familiarity with those concepts.

## Prerequisites

- Access to an Azure subscription with **Owner** or **User Access Administrator** access (needed to assign the AcrPull role in Challenge 04)
- An Azure AI Foundry project with a deployed model (e.g., `gpt-5.1`)
- An Azure Container Registry in the same resource group
- Python 3.10+ with Jupyter notebook support
- Azure CLI installed and authenticated (`az login`)
- [Visual Studio Code](https://code.visualstudio.com/) with the Python and Jupyter extensions

## Repository Contents

- `./Coach`
  - Coach's Guide and related files
  - `./Coach/Solutions`
    - Solution notebook with completed example answers
- `./Student`
  - Student's Challenge Guide
  - `./Student/Resources`
    - Starter notebook and pre-built container files (Dockerfile, requirements.txt)

## Contributors

- Anastasia Nefedova
