# Day 89 – KubeHealer: Production AI Agents & AIOps

## What I Learned

Today I worked on **AIOps (AI-powered IT Operations)** and learned how an AI agent can not only diagnose Kubernetes problems but also suggest and apply fixes with human approval.

The main project was **KubeHealer**, a production-style AI agent using:

- Kubernetes
- `kubectl`
- Claude
- Temporal
- Python

The main idea was simple:

```text
Kubernetes Cluster
       ↓
Find broken pods
       ↓
Diagnose the problem
       ↓
Claude reasons about the issue
       ↓
Propose a fix
       ↓
Human approval
       ↓
Apply the fix
```

---

## What is AIOps?

AIOps means using AI to help with IT operations such as:

- monitoring
- troubleshooting
- root-cause analysis
- remediation

The important point I learned is that AIOps does not mean giving AI unlimited control.

The agent should handle routine problems and involve a human when the situation is risky or unclear.

---

## Production Guardrails

Before allowing an AI agent to change infrastructure, I learned about six important guardrails.

### 1. Human Approval

The agent should ask before making important or destructive changes.

Example:

```text
3 broken pods found.

Here are the proposed fixes.

Approve?
```

### 2. Scope Limits

The agent should only work in allowed namespaces or clusters.

For example, it should not be allowed to randomly modify `kube-system` or production databases.

### 3. Audit Trail

Every important action should be recorded.

Temporal keeps the workflow history so we can see what happened.

### 4. Rollback Capability

Changes should be reversible.

Small targeted patches are safer than deleting and recreating resources.

### 5. Timeout and Retry Limits

The agent should not keep trying forever.

For example:

```text
Maximum retries = 3
Timeout = 5 minutes
```

### 6. Escalation Path

If the agent cannot safely fix something, it should stop and ask a human.

This is important because a good agent should know its limits.

---

## Setting Up KubeHealer

I cloned the project:

```bash
git clone https://github.com/TrainWithShubham/kubehealer.git
cd kubehealer
```

### Create Kind Cluster

```bash
kind create cluster --name kubehealer-demo
```

### Start Temporal

```bash
temporal server start-dev
```

Temporal UI:

```text
http://localhost:8233
```

### Python Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Claude API Key

```bash
export ANTHROPIC_API_KEY="your-api-key-here"
```

---

## Creating Broken Kubernetes Applications

To test the agent, I intentionally created three broken applications.

### App 1 – Image Typo

The image was:

```yaml
image: ngnix:latest
```

The correct image is:

```yaml
image: nginx:latest
```

Because of the typo, Kubernetes reported:

```text
ImagePullBackOff
```

This was a problem the agent could safely identify and fix.

---

### App 2 – Low Memory Limit

The application had:

```yaml
limits:
  memory: "1Mi"
```

The memory limit was intentionally too low, causing an:

```text
OOMKilled
```

The agent diagnosed the problem and proposed increasing the memory limit.

---

### App 3 – Missing ConfigMap

The pod referenced:

```yaml
configMapRef:
  name: app-config
```

But the ConfigMap did not exist.

The important part was that the agent did **not** blindly create something.

It diagnosed the problem and escalated it for human attention.

---

## Running KubeHealer

I started the worker:

```bash
python3 worker.py
```

In another terminal:

```bash
python3 starter.py
```

The agent followed this flow:

```text
Scan
  ↓
Find broken pods
  ↓
Diagnose
  ↓
Ask Claude
  ↓
Propose fixes
  ↓
Human approval
  ↓
Apply safe fixes
  ↓
Verify
```

The proposed fixes were:

```text
web-app
ngnix → nginx

memory-app
1Mi → 128Mi

config-app
Missing ConfigMap
→ Human attention required
```

After approval, the first two applications were fixed while the third remained unresolved.

---

## Temporal Crash Recovery

One of the most interesting parts was testing what happens when the agent crashes during a workflow.

I started the workflow and stopped the worker before the process completed.

Then I started the worker again.

Temporal had stored the workflow history, so the workflow could continue from where it stopped instead of starting everything again.

```text
Agent starts
    ↓
Scan
    ↓
Diagnose
    ↓
Worker crashes
    ↓
Worker restarted
    ↓
Temporal replays completed steps
    ↓
Workflow continues
```

This showed me why durable execution is important for infrastructure automation.

---

## Temporal UI

I also checked the Temporal UI:

```text
http://localhost:8233
```

It provides a history of the workflow execution, including the activities and their progress.

This works as an important audit trail for an infrastructure-changing AI agent.

---

## AI Agents vs Traditional Automation

One important lesson from today:

### Use AI Agents when:

- the problem needs reasoning
- there can be multiple possible causes
- there can be multiple possible fixes
- natural-language explanation is useful

Example:

```text
Why is this Kubernetes pod failing?
```

### Use Traditional Automation when:

- the solution is already known
- the condition is simple
- the same action should always happen

Example:

```text
If CPU > 80%, increase replicas.
```

AI should not be used just because AI is available.

---

## Day 87 → Day 89 Progress

The last three days showed a clear progression:

```text
Day 87
LLM explains errors
        ↓
Day 88
Agent investigates using multiple tools
        ↓
Day 89
Agent diagnoses + proposes + fixes with approval
```

This helped me understand how an AI system can move from simply answering questions to taking controlled actions.

---

## What I Learned

- AIOps is more than a chatbot.
- AI agents can help with real infrastructure problems.
- Human approval is important for risky changes.
- Scope limits prevent an agent from touching everything.
- Audit trails help understand what the agent did.
- Rollback and retry limits make automation safer.
- Temporal provides durable execution.
- A good agent should know when it cannot safely fix something.
- AI agents are useful for problems that require reasoning.
- Traditional automation is better for simple and predictable tasks.

---

## Cleanup

After completing the lab:

```bash
kind delete cluster --name kubehealer-demo
```

Stop Temporal with `Ctrl+C` and deactivate the environment:

```bash
deactivate
```

---

## Day 89 Completed

Today I built and tested a production-style Kubernetes healing agent with **Claude + Temporal + Kubernetes**.

The biggest takeaway for me was:

> An AI agent should not just be capable of taking action. It should also be controlled, traceable, and able to stop when it is not safe to continue.

Still learning and building step by step. 

#90DaysOfDevOps #DevOpsKaJosh #AIOps #Kubernetes #AI #DevOps #Temporal #Claude
