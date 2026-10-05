# Day 88 — Multi-Tool DevOps Agent, MCP & CI/CD Analyzer

##  Day 88 Overview

Today I extended the DevOps AI agent to work with more than one DevOps area.

The main focus was:

- Docker troubleshooting
- Kubernetes troubleshooting
- Multi-tool AI agents
- Model Context Protocol (MCP)
- MCP server and client
- GitHub Actions failure analysis

The goal was to understand how an AI agent can use different tools depending on the problem.

---

##  What I Built

By the end of this task, the architecture included:

- Docker troubleshooting tools
- Kubernetes troubleshooting tools
- A multi-tool DevOps agent
- An MCP server exposing Kubernetes tools
- An MCP client connecting the tools to the agent
- A CI/CD Failure Analyzer for GitHub Actions

---

# 1. Multi-Tool DevOps Agent

Previously, the agent worked with Docker tools.

Now I added Kubernetes tools to the same agent.

## Docker Tools

The Docker side has three tools:

```text
list_containers()
get_logs(container_name)
inspect_container(container_name)
```

These tools are used for basic Docker troubleshooting.

## Kubernetes Tools

I added three Kubernetes tools:

```text
list_pods(namespace)
describe_pod(pod_name, namespace)
get_events(namespace)
```

These tools use `kubectl` commands to collect Kubernetes information.

---

# 2. Kubernetes Troubleshooting

For testing, a Kind cluster can be created:

```bash
kind create cluster --name devops-demo
```

A deliberately broken pod can then be deployed.

The pod starts and exits with an error after a short time.

This gives the agent a real problem to investigate.

Example questions:

```text
Why is broken-pod crashing?
```

```text
Describe the events in the default namespace
```

```text
List the pods in my cluster
```

The agent decides which Kubernetes tool is useful for the question.

---

# 3. How the Multi-Tool Agent Works

The basic flow is:

```text
User Question
      ↓
AI Agent
      ↓
Decides which tool to use
      ↓
Docker / Kubernetes Tool
      ↓
CLI Command
      ↓
Command Output
      ↓
Agent Analysis
      ↓
Answer
```

For example:

```text
Question about Docker
        ↓
Docker tools
```

While:

```text
Question about Kubernetes
        ↓
Kubernetes tools
```

And a question involving both can use tools from both domains.

---

# 4. Model Context Protocol (MCP)

## What is MCP?

MCP stands for **Model Context Protocol**.

It provides a standard way for AI applications to connect with external tools and data sources.

Instead of keeping tools directly inside one agent, tools can be exposed through an MCP server.

---

## Why MCP is Useful

Without MCP:

```text
Tools
 ↓
Specific Agent / Framework
```

With MCP:

```text
             MCP Server
          /      |       \
       Tool    Tool     Tool
          \      |       /
           MCP Clients
```

The same tools can then be used by different MCP-compatible clients.

Examples include:

- Claude Desktop
- VS Code / GitHub Copilot
- Cursor
- Claude Code
- Python agents

---

# 5. MCP Server

For Kubernetes, the MCP server exposes tools such as:

```text
list_pods()
describe_pod()
get_events()
```

The server uses FastMCP.

The basic structure is:

```python
from fastmcp import FastMCP

mcp = FastMCP("Kubernetes Tools")
```

Tools are registered using:

```python
@mcp.tool
```

The server starts with:

```python
mcp.run()
```

---

# 6. MCP vs Normal Agent Tools

There is an important difference between normal LangChain tools and MCP tools.

### Normal tool

```python
@tool
def list_pods():
    ...
```

The tool is defined directly inside the agent application.

### MCP tool

```python
@mcp.tool
def list_pods():
    ...
```

The tool is registered with the MCP server.

The MCP client can then discover the available tools.

---

# 7. MCP Client

The agent can connect to the MCP server using an MCP client.

The basic flow becomes:

```text
AI Agent
    ↓
MCP Client
    ↓
MCP Server
    ↓
Kubernetes Tools
    ↓
kubectl
```

The client dynamically discovers the tools from the MCP server.

This means the agent does not need to hardcode every Kubernetes tool locally.

---

# 8. CI/CD Failure Analyzer

The same tool-based agent pattern can also be used for CI/CD troubleshooting.

For this part, the GitHub CLI is used.

First, GitHub CLI authentication is required:

```bash
gh auth login
```

The analyzer works with GitHub Actions workflow information.

---

## CI/CD Tools

The analyzer has three main tools:

```text
list_workflow_runs()
get_failed_logs()
get_workflow_file()
```

### `list_workflow_runs()`

Lists recent GitHub Actions workflow runs.

### `get_failed_logs()`

Gets logs from the failed steps of a workflow run.

### `get_workflow_file()`

Reads a GitHub Actions workflow YAML file.

---

# 9. CI/CD Failure Analysis Flow

The basic process is:

```text
GitHub Actions Failure
        ↓
List failed workflow runs
        ↓
Get failed logs
        ↓
Read workflow file if required
        ↓
Agent analyzes information
        ↓
Explain likely failure
```

Example questions:

```text
What failed in my last CI run?
```

```text
Show me the recent workflow runs
```

```text
Read the gitops-ci.yml workflow file and explain what it does
```

---

# 10. Why Log Truncation Matters

CI/CD logs can become very large.

Sending the entire log to an LLM is not always useful.

The analyzer therefore limits the failed log output.

Example:

```text
Maximum useful output
        ↓
Focused information
        ↓
Less unnecessary context
        ↓
Better analysis
```

The example implementation truncates the output to around 5000 characters.

---

# 11. Tool Pattern

The most useful pattern I learned today is that almost any CLI command can become an AI tool.

The general pattern is:

```text
CLI Command
     ↓
Python Tool
     ↓
AI Agent
     ↓
Tool Selection
     ↓
Command Output
     ↓
Analysis
```

This can be used for many DevOps tasks.

Examples:

- Docker
- Kubernetes
- Terraform
- AWS CLI
- GitHub CLI
- Log searching

---

# 12. Possible Custom Tools

Some examples of tools that can follow the same pattern:

### Terraform Plan Analyzer

```text
terraform plan
       ↓
Tool
       ↓
Agent explains planned changes
```

### AWS Resource Checker

```text
aws ec2 describe-instances
       ↓
Tool
       ↓
Agent explains EC2 resources
```

### Kubernetes Log Searcher

```text
kubectl logs
       ↓
Search for keyword
       ↓
Return matching pods
```

---

# 13. Important Lessons

### 1. One agent can use multiple tools

The agent does not need to be limited to one DevOps platform.

### 2. Tool descriptions matter

The tool docstring helps the agent understand when a tool should be used.

For example:

```python
"""List all pods in a Kubernetes namespace with their status."""
```

is more useful than a vague description.

### 3. MCP makes tools reusable

Instead of tying tools to one agent, MCP allows compatible clients to discover and use them.

### 4. Keep LLM input focused

Large logs should be filtered or truncated before sending them to the model.

---

# 14. Architecture

The overall architecture I learned today looks like this:

```text
                    User
                      |
                      v
                 AI Agent
                      |
             +--------+--------+
             |                 |
             v                 v
        Docker Tools      MCP Client
                               |
                               v
                          MCP Server
                               |
                        +------+------+
                        |      |      |
                        v      v      v
                     Pods   Describe Events
                        |
                        v
                     kubectl


             CI/CD Analyzer
                    |
                    v
                  gh CLI
                    |
                    v
             GitHub Actions
```

---

# 15. Day 88 Takeaway

Today I understood how an AI agent can move beyond a single tool.

Instead of manually running every command, the agent can decide which tool is useful based on the question.

I also learned how MCP can separate tools from the agent and make them available to different AI clients.

The main pattern I am taking from today is:

```text
Define useful tools
       ↓
Connect tools to an agent
       ↓
Let the agent decide when to use them
       ↓
Collect the output
       ↓
Explain the result
```

This makes the idea of AI-powered DevOps troubleshooting much more practical.

---

##  Cleanup

After testing, the Kind cluster and broken container can be removed:

```bash
kind delete cluster --name devops-demo
```

```bash
docker rm -f broken-container 2>/dev/null
```

---

##  Reference

TrainWithShubham — Agentic AI for DevOps

Modules covered:

- Module 3
- Module 6

---

##  Progress

**Day 88/90 — Completed**

Learning step by step and continuing the journey.
#90DaysOfDevOps #DevOpsKaJosh
