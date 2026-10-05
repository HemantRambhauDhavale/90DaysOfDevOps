# Day 87 – Introduction to Agentic AI for DevOps

## What I Learned

Today I started a new part of my 90 Days of DevOps journey: **Agentic AI for DevOps**.

Until now, most of my work was around Linux, Docker, CI/CD, Kubernetes, Terraform, Ansible, monitoring, Helm, EKS and GitOps. Today I learned how AI agents can be connected with DevOps tools and actually use them to investigate problems.

The main difference I understood is:

- A **chatbot** mainly gives us text answers.
- An **AI agent** can use tools, run commands, read the output and decide what to do next.

For example, instead of manually running `docker ps`, `docker logs` and `docker inspect`, an agent can decide which commands it needs to run to find the problem.

---

## 1. Understanding Agentic AI

An AI agent uses an LLM together with tools.

The basic flow is:

```text
User Question
     ↓
LLM
     ↓
Choose a Tool
     ↓
Run the Tool
     ↓
Read the Output
     ↓
Reason Again
     ↓
Final Answer
```

For DevOps, this is useful because we work with many CLI tools such as:

- Docker
- kubectl
- Terraform
- AWS CLI
- GitHub CLI
- Ansible

The agent can use these tools and understand their output.

---

## 2. Understanding the ReAct Pattern

The agent I worked with uses the **ReAct pattern**:

**Reason → Act → Observe**

For example:

```text
User: Why is broken-app crashing?

Reason:
I should check the containers.

Act:
list_containers()

Observe:
broken-app is restarting.

Reason:
I should check its logs.

Act:
get_logs("broken-app")

Observe:
The application starts and then exits.

Reason:
I should inspect the container.

Act:
inspect_container("broken-app")

Observe:
Exit code is 1.

Answer:
The container exits with code 1 after starting.
```

The important part for me was that I did not manually tell the agent which command to run. The agent selected the tools based on the question.

---

## 3. Setting Up the Environment

I cloned the Agentic AI for DevOps repository and created a Python virtual environment.

```bash
git clone https://github.com/TrainWithShubham/agentic-ai-for-devops.git
cd agentic-ai-for-devops

python3 -m venv .venv
source .venv/bin/activate

pip install -r requirements.txt
```

I also set up Ollama and the Gemma 4 model:

```bash
ollama serve &
ollama pull gemma4
```

Then I checked the model:

```bash
ollama list
```

Finally, I ran the setup verification:

```bash
python3 module-0/verify_setup.py
```

The expected result was:

```text
[PASS] Python 3.10+
[PASS] Docker
[PASS] kubectl
[PASS] Kind
[PASS] Ollama + gemma4

5/5 -- you're ready for Day 1!
```

---

## 4. Docker Error Explainer

The first practical task was simple: give a Docker error to an LLM and let it explain the problem.

The application uses a system prompt like:

```text
You are a Docker expert. When given a Docker error, explain:
1. What went wrong
2. Most likely cause
3. How to fix it
Keep it short.
```

The important thing I learned here was the difference between an LLM call and an agent.

This part does not use tools or an agent loop. It is simply:

```text
Docker Error
     ↓
LLM
     ↓
Explanation
```

I also learned why a lower temperature such as `0.3` is useful for technical answers because it makes the output more deterministic.

---

## 5. Building the Docker Troubleshooter Agent

Next, I created a container that intentionally crashes:

```bash
docker run -d --name broken-app nginx:alpine sh -c "echo 'app starting...' && sleep 2 && exit 1"
```

Then the agent was given three tools:

```python
@tool
def list_containers() -> str:
    ...

@tool
def get_logs(container_name: str) -> str:
    ...

@tool
def inspect_container(container_name: str) -> str:
    ...
```

These tools basically wrap Docker commands:

```text
list_containers()   → docker ps -a
get_logs()          → docker logs
inspect_container() → docker inspect
```

The `@tool` decorator makes the function available to the agent.

One important thing I learned is that the **docstring matters**. The LLM reads the tool description to understand when it should use that tool.

---

## 6. Running the Agent

I ran:

```bash
python3 module-2/agent.py
```

Then I asked:

```text
Why is broken-app crashing?
```

The agent followed the troubleshooting process:

1. Listed the containers.
2. Found `broken-app` restarting.
3. Read the container logs.
4. Inspected the container.
5. Found the exit code.
6. Explained the likely reason for the crash.

This was the main difference from a normal chatbot for me.

The agent was not only answering from existing knowledge. It was using the actual Docker environment to collect information first.

---

## 7. Understanding the Architecture

The complete flow looked like this:

```text
[User Question]
       |
       v
[LLM: Gemma 4]
       |
       v
[Tool Selection]
       |
       +----> list_containers() ---> docker ps -a
       |
       +----> get_logs() ---------> docker logs
       |
       +----> inspect_container() -> docker inspect
       |
       v
[Tool Output]
       |
       v
[LLM reasons again]
       |
       v
[Final Answer]
```

What I found interesting is that the same architecture can be used with other DevOps tools.

For example:

```text
Docker Tools
     ↓
Kubernetes Tools
     ↓
Terraform Tools
     ↓
AWS CLI Tools
```

The tools change, but the basic agent pattern remains similar.

---

## 8. Adding My Own Tool

I also experimented with adding a Docker image tool:

```python
@tool
def list_images() -> str:
    """List all Docker images on this machine with their sizes."""
    result = subprocess.run(
        ["docker", "images"],
        capture_output=True,
        text=True
    )
    return result.stdout or result.stderr
```

Then I added it to the tools list:

```python
tools = [
    list_containers,
    get_logs,
    inspect_container,
    list_images
]
```

Now the agent can answer questions about Docker images by calling the new tool.

This helped me understand that a CLI command can be wrapped as a tool and exposed to an AI agent.

---

## 9. Important Safety Lesson

I also tried the idea of adding a `restart_container` tool.

That made me think about an important difference between **read-only tools** and **action tools**.

Reading:

```text
docker ps
docker logs
docker inspect
```

is one thing.

Allowing an agent to run:

```text
docker restart
```

is different because the agent can now change the environment.

In a real production setup, guardrails and confirmation should be considered before allowing an AI agent to perform actions automatically.

---

## Key Takeaways

Today I learned:

- What an AI agent is.
- How an agent is different from a normal chatbot.
- What the ReAct pattern means.
- How Ollama can run an LLM locally.
- How LangChain connects an LLM with tools.
- How Python functions can wrap CLI commands.
- Why tool docstrings are important.
- How an agent can troubleshoot a Docker container.
- Why read-only tools and action tools need different safety considerations.
- How the same idea can later be used with Kubernetes, Terraform and AWS CLI.

The biggest thing I understood today is that **Agentic AI is not just about asking AI questions. It is about giving AI controlled access to tools so it can investigate a real environment and decide what to do next.**

---

## Day 87 Submission

Created:

```text
2026/day-87/day-87-agentic-ai-intro.md
```

The documentation covers the agent concept, ReAct pattern, environment setup, Docker Error Explainer, Docker Troubleshooter Agent, architecture, custom tool and system prompt/temperature concepts.

---

## What's Next?

Day 88 will move from Docker to **Kubernetes tools**, which should make the agent much more useful for real DevOps troubleshooting.

#90DaysOfDevOps #DevOpsKaJosh #AgenticAI #DevOps #Docker #AI
