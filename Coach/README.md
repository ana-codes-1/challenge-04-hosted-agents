# What The Hack - Hosted Agents on Azure AI Foundry - Coach Guide

## Introduction

Welcome to the coach's guide for the Hosted Agents What The Hack. Here you will find links to specific guidance for coaches for each of the challenges.

**NOTE:** If you are a Hackathon participant, this is the answer guide. Don't cheat yourself by looking at these during the hack! Go learn something. :)

## Coach's Guides

- Challenge 04: **[Deploy a Hosted Weather Agent](Solution-04.md)**
  - Build a weather agent with a custom tool, containerize it, deploy to Foundry, and invoke it

## Coach Prerequisites

This hack has pre-reqs that a coach is responsible for understanding and/or setting up BEFORE hosting an event. Please review the [What The Hack Hosting Guide](https://aka.ms/wthhost) for information on how to host a hack event.

The guide covers the common preparation steps a coach needs to do before any What The Hack event, including how to properly configure Microsoft Teams.

### Student Resources

Before the hack, it is the Coach's responsibility to download and package up the contents of the `/Student/Resources` folder of this hack into a "Resources.zip" file. The coach should then provide a copy of the Resources.zip file to all students at the start of the hack.

Always refer students to the [What The Hack website](https://aka.ms/wth) for the student guide: [https://aka.ms/wth](https://aka.ms/wth)

**NOTE:** Students should **not** be given a link to the What The Hack repo before or during a hack. The student guide does **NOT** have any links to the Coach's guide or the What The Hack repo on GitHub.

## Azure Requirements

This hack requires students to have access to an Azure subscription where they can create and consume Azure resources. These Azure requirements should be shared with a stakeholder in the organization that will be providing the Azure subscription(s) that will be used by the students.

- Azure subscription with **Owner** or **User Access Administrator** role (needed to assign the AcrPull role at registry scope)
- Azure AI Foundry project with a deployed model (e.g., `gpt-5.1`)
- Azure Container Registry (ACR) in the same resource group as the Foundry project
- Foundry Project Manager role at project scope

## Suggested Hack Agenda

- Challenge 04 (2-3 hours)
  - Parts 1-2: Define tool and instructions (~20 min)
  - Part 3-4: Assemble code and configure files (~15 min)
  - Part 5: Build container image (~15 min, mostly waiting)
  - Part 6: Deploy to Foundry (~20 min, includes polling)
  - Part 7: Invoke and verify (~10 min)
  - Buffer for troubleshooting and discussion (~40 min)

## Repository Contents

- `./Coach`
  - Coach's Guide and related files
- `./Coach/Solutions`
  - Solution notebook with completed example answers to the challenge
- `./Student`
  - Student's Challenge Guide
- `./Student/Resources`
  - Starter notebook and pre-built container files meant to be provided to students. (Must be packaged up by the coach and provided to students at start of event)
