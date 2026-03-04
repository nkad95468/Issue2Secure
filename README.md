# Feature to Deploy Orchestrator

A multi-agent flow built on the GitLab Duo Agent Platform that automates the full software development lifecycle — from a GitLab issue to a deployed Merge Request — without manual intervention.

## Overview

When a developer creates an issue describing a feature or bug fix, this orchestrator takes over. It analyzes the issue, generates a structured implementation plan, writes production-ready code, and opens a Merge Request — all autonomously.

The goal is to eliminate the friction between "I have an idea" and "the code is ready for review."

## How It Works

1. A developer mentions `@ai-feature-to-deploy-orchestrator-gitlab-ai-hackathon` in a GitLab issue
2. The **Planner Agent** reads the issue, analyzes requirements, and posts a structured implementation plan as a comment
3. The **Code Generator Agent** reads the plan and generates production-ready code files
4. A Merge Request is created automatically with the generated code, linked back to the original issue

## Agents

### Planner Agent
Reads a GitLab issue and produces a structured implementation plan including complexity estimate, step-by-step breakdown, and list of files to create or modify.

Tools: `get_issue`, `create_issue_note`, `find_files`, `read_file`

### Code Generator Agent
Receives the implementation plan and generates clean, well-commented code. Creates the necessary files in the repository and opens a Merge Request with a full description.

Tools: `get_issue`, `create_file_with_contents`, `create_merge_request`, `create_issue_note`

## Usage

In any issue in your project, mention the flow:
```
@ai-feature-to-deploy-orchestrator-gitlab-ai-hackathon analyze this issue and generate the implementation
```

The flow will respond with a session link where you can track progress in real time.

## Stack

- GitLab Duo Agent Platform (orchestration)
- GitLab Duo Chat (agent execution)
- Python + FastAPI (generated code target)
- PostgreSQL (generated code target)

## Project Structure
```
.
├── agents/
│   └── agent.yml          # Feature Planner standalone agent
├── flows/
│   └── flow.yml           # Feature to Deploy Orchestrator flow
└── README.md
```

## Requirements

- GitLab Duo Enterprise seat
- Flow execution enabled in your project
- Maintainer or Owner role to enable the flow

## License

MIT