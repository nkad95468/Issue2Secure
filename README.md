# Feature to Deploy Orchestrator

A multi-agent system built on the GitLab Duo Agent Platform that automates the full software development lifecycle — from a GitLab issue to a security-reviewed Merge Request — without manual intervention.

## Overview

When a developer creates an issue describing a feature or bug fix, two coordinated AI flows take over. The first analyzes the issue and generates production-ready code. The second scans that code for security vulnerabilities and blocks the MR if critical issues are found.

The goal is to eliminate the friction between "I have an idea" and "the code is ready for secure review."

## How It Works

### Flow 1 — Feature to Deploy Orchestrator

1. A developer mentions `@ai-feature-to-deploy-orchestrator-gitlab-ai-hackathon` in a GitLab issue
2. The Planner Agent reads the issue, analyzes the existing codebase, and posts a structured implementation plan as a comment
3. The Code Generator Agent reads the plan and generates complete, production-ready code for every file
4. A Merge Request is created automatically, linked to the original issue

### Flow 2 — Security Scanner

1. After the code is generated, a developer mentions `@ai-security-scanner-gitlab-ai-hackathon` in the same issue
2. The Security Scanner Agent reads all issue comments to find the generated code
3. It analyzes the code for vulnerabilities: SQL injection, hardcoded secrets, missing authentication, race conditions, memory exhaustion, and more
4. It posts a detailed security report with severity levels and specific fixes for each vulnerability
5. If Critical or High vulnerabilities are found, the MR is automatically blocked with a comment
6. If the code is clean, the MR is approved

## Agents

### Planner Agent
Reads a GitLab issue and produces a structured implementation plan including complexity estimate, step-by-step breakdown, and list of files to create or modify. Analyzes the existing codebase before planning.

Tools: `get_issue`, `create_issue_note`, `find_files`, `read_file`

### Code Generator Agent
Receives the implementation plan from the Planner and generates complete, well-commented, production-ready code for every file in the plan. Creates a Merge Request with full description.

Tools: `get_issue`, `list_issue_notes`, `create_issue_note`, `create_merge_request`

### Security Scanner Agent
Reads the generated code from issue comments and performs a thorough security audit. Reports vulnerabilities with severity levels (Critical, High, Medium, Low), affected files, line references, and concrete remediation steps. Blocks or approves the MR based on findings.

Tools: `get_issue`, `list_issue_notes`, `create_issue_note`, `get_merge_request`, `update_merge_request`

## Usage

### Step 1 — Create an issue describing your feature
```
Title: Add JWT authentication endpoint
Description:
Create POST /api/auth/login that accepts email/password
and returns a JWT token. Use FastAPI + PostgreSQL.
```

### Step 2 — Trigger the Feature to Deploy Orchestrator

In the issue comments:
```
@ai-feature-to-deploy-orchestrator-gitlab-ai-hackathon analyze this issue and generate the implementation code
```

Wait for the Planner to post the implementation plan and the Code Generator to create the MR.

### Step 3 — Trigger the Security Scanner

In the same issue:
```
@ai-security-scanner-gitlab-ai-hackathon please scan the generated code in this issue for security vulnerabilities
```

The scanner will post a full security report and approve or block the MR automatically.

## Security Checks Performed

The Security Scanner audits generated code for the following:

- Hardcoded secrets and API keys
- SQL injection vulnerabilities
- Missing authentication and authorization
- Insecure password handling
- Race conditions and thread safety issues
- Memory exhaustion vectors
- IP spoofing in rate limiting
- Missing input validation
- Insecure JWT configuration
- Information disclosure via public endpoints
- Missing HTTPS enforcement
- CORS misconfiguration

## Project Structure
```
.
├── agents/
│   └── agent.yml                  # Feature Planner standalone agent
├── flows/
│   ├── flow.yml                   # Feature to Deploy Orchestrator
│   └── security-scanner.yml      # Security Scanner
├── src/
│   ├── main.py                    # FastAPI base application
│   ├── auth.py                    # Authentication utilities
│   └── ...                        # Generated code by the agents
├── tests/
│   └── test_auth.py               # Test suite
├── requirements.txt
└── README.md
```

## Example Output

### Planner Output
The Planner posts a structured comment with complexity estimate, implementation steps, and files to create.

### Code Generator Output
The Code Generator posts complete code for every file, then opens a Merge Request with full description, setup instructions, and API documentation.

### Security Scanner Output
The Security Scanner posts a report like:
```
Risk Level: CRITICAL

[CRITICAL] Missing Authentication on Logs Endpoint
[HIGH] No Rate Limiting
[MEDIUM] Missing Security Headers

Verdict: BLOCKED
```

And adds a blocking comment to the MR automatically.

## Requirements

- GitLab Duo Enterprise seat
- Flow execution enabled in your project
- Maintainer or Owner role to enable the flows

## License

MIT