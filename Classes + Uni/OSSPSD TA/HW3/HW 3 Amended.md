## Amendment Purpose

Based on your memos and what we discussed on Wednesday, we have drafted a revised version of the HW. This reflects the reduced scope (e.g., single-list ticketing) and the specific architectural flow the professor described (using the AI as a translation layer between human text and API calls).

## Introduction

We have arrived at the final stretch of the semester. While we call these "homeworks," you have likely realized by now that they are actually iterative sprints in a larger engineering project.

At the end of the day, this entire course is built on the reality of group work and professional standards. By now, I hope you understand that we care about the process that makes you a deployable engineer.

If you are writing Python in an enterprise environment, the reality is strict:

  * You will use a deterministic (sometimes even custom) package manager.
  * You will face strict governance on how you commit, push, squash, and rebase.
  * Your CI pipeline will act as a gatekeeper, rejecting anything that breaks the build.
  * Your senior engineers will be brutal if you make brittle design choices.
  * Documentation is not optional.

That is the soul of this class. The leap in quality between HW1 and HW2 was significant, and I hope you are finding better ways to prototype and collaborate.

More remarks are at the bottom of the doc this time around.

## The Assignment

### Objective

For this final assignment, you are no longer building in isolation. You will be building a **Smart Chat Bot** that integrates three distinct domains: **Chat, Tickets, and AI**. This bot will function as a deployed application, complete with monitoring and infrastructure managed as code.

The final product must be able to:
*   **Answer questions** by leveraging an AI model.
*   **Fetch and summarize tickets** from a ticketing system (e.g., "Show me my 3 most recent open tickets").
*   **Take action on tickets** (e.g., "Close this ticket").

The core challenge of this project lies in collaboration and integration. You will not be writing code for the systems you did not build. Instead, you will pull the API references for the other two systems, apply dependency injection, and make them work seamlessly with your component. This will require negotiation, agreeing on shared API contracts, and finding ways to properly manage dependencies.

### Instructions

The core steps are:

1. **Integration:** Your primary task is to make three disparate systems—Chat, Tickets, and AI—communicate effectively.
2.  **Deployment:** This is not a local-only project. You will deploy your application and manage its infrastructure as code.
3.  **Observability:** Your deployed application must emit telemetry data to monitor its health and performance.

#### IoC & Telemetry

Your application must be deployed, and its infrastructure must be managed using an **Infrastructure as Code (IaC)** tool like Terraform or AWS CloudFormation. You are responsible for provisioning the necessary resources (e.g., servers, databases, environment variables) in a repeatable and automated way.

Furthermore, your deployed application must emit **telemetry data**. This is non-negotiable and critical for understanding the performance of a live service. You must implement monitoring for:
*   **Request Latency:** How long does each API call or user interaction take?
*   **Success Rate:** What percentage of requests are completed successfully?
*   **Failure Rate:** What percentage of requests result in errors?

You should use a monitoring or observability platform to collect and visualize this data.

I recommend that you have a working version of this by the [[HW 3 v1#Second Submission]] deadline
#### Interface Deliverable

Your first task is to collaborate within your vertical (Chat, Tickets, AI) to define a standardized API that all teams in that vertical will implement. This shared interface is crucial, as it will be the contract that the other two verticals depend on to integrate your system.

##### Process

1.  Meet with the other teams in your vertical.
2.  Define a common API that abstracts away the specific details of each team's implementation (e.g., Discord vs. Slack). This should be defined using Python's `abc` (Abstract Base Classes) module.
3.  One representative from the vertical must submit a memo detailing the agreed-upon API.
4.  Every team must include a small plan of action describing how they will adapt their existing codebase to this new, standardized interface.

This is a **shared grade** per vertical. Failure to agree on and document a viable API will impact every team in that vertical. 

This can be a very small doc and I exhort you to not use AI when drafting it.

##### Chat Teams (Teams 1, 4, 7)
*   **Services:** Discord, Slack
*   **Challenge:** Your APIs must abstract the differences between platforms. For instance, Team 4's Slack implementation lacks a delete message feature, and Team 1's Discord bot did not implement channels. The shared API must account for these potential differences gracefully.
*   **Suggested API Methods:**
    *   `get_messages(channel_id: str, limit: int) -> List[Message]`
    *   `get_message(message_id: str) -> Message`
    *   `send_message(channel_id: str, message_text: str) -> Message`
    *   `delete_message(message_id: str) -> None`
    *   `get_channels() -> List[Channel]`
    *   `get_channel(channel_id: str) -> Channel`

##### Ticket Teams (Teams 2, 6, 8)
*   **Services:** Google Tasks, Jira, Trello
*   **Challenge:** You need to unify list-based systems (Jira, Google Tasks) with a Kanban-board system (Trello). The key is to find a common abstraction. A Kanban board can be viewed as a collection of lists (e.g., "To Do," "In Progress," "Done"), so the Trello API can be adapted to expose a list-based interface.
*   **Suggested API Abstractions:**
    *   A `Ticket` data class with fields like `id`, `title`, `status`, `description`.
    *   `get_tickets(status: str = 'open') -> List[Ticket]`
    *   `get_ticket(ticket_id: str) -> Ticket`
    *   `create_ticket(title: str, description: str) -> Ticket`
    *   `update_ticket_status(ticket_id: str, new_status: str) -> None`

##### AI Teams (Teams 3, 5, 9, 10)
*   **Services:** OpenAI, Gemini, Claude
*   **Tools:** Not many of you actually added the ability for the model to call tools in HW2. This is necessary for the ticket deletion. 
*   **Challenge:** You need to standardize the input/output for interacting with the language model. You also need to devise a common authentication strategy that works for all providers.
*   **Suggested API Methods:**
	*   Research what the standard approach is for calling tools and implement it.
    *   `send_message(prompt: str, context: Optional[Dict] = None) -> str`
    *   You must also agree on a unified approach for handling authentication keys and credentials for the different services.

#### First submission

By the turn of the month, your individual team's project must be refactored to implement the shared, standardized API for your vertical. This is a hard deadline, as other teams will be depending on this standardized interface for the next phase.

#### Second Submission

Choose **one** of the other two verticals (either Tickets or AI) and integrate their system into your project. For example, if you are a Chat team, you might integrate an AI system first. Your submission must:
*   Demonstrate successful integration with at least two different providers from that vertical (e.g., a Chat bot working with both OpenAI and Gemini).
*   Include integration tests that verify the two systems are working together correctly.

I recommend you also have your IoC and Telemetry setup by now.

#### Final Submission

This is the final deliverable. It includes your complete, three-part system, documentation, and a video demonstration.
*   **Pull Request:** A clean, well-documented PR with your final code.
*   **Video Demonstration:** You must record a video that includes:
    *   A clear explanation, in your own words, of how your complete project works.
    *   A live demonstration of its functionality, showcasing integration with different providers (e.g., swapping Jira for Trello).
    *   A walkthrough of your CircleCI deployment pipeline.
    *   An explanation of the tests being run on the cloud.
    *   A high-level overview of your end-to-end (e2e) and integration tests and what they verify.
    *   A view of your telemetry dashboard showing request latency and success/failure rates.

### Deadlines

*   **11/21:** Draft of this assignment is provided.
*   **11/26:** Draft is frozen.
*   **11/27 - Interface Deliverable:** Verticals must submit their agreed-upon shared interface memo and individual team alignment plans.
*   **11/30 - First Submission:** All teams must have their code aligned with the shared API.
*   **12/03:** TAs will provide feedback on API alignment and interface designs.
*   **12/07 - Second Submission:** First cross-vertical integration is complete.
*   **12/10:** Peer feedback and TA feedback on the first integration.
*   **12/17 - Final Submission:** Final project, including the video demonstration, is submitted.
    *   **Note:** We can be flexible and extend this deadline to **December 21st**. The 17th can be a suggested deadline, with no penalty for submissions up to the 21st to accommodate proximity to the holidays.

## Remarks

### Ruff / MyPy with external projects

If you pull a repo and it works out of the box, you can set it to be ignored in type checks (only if that is breaking your lint). However, if you had to make major edits to the external repo, you should align whatever code you pull from them to your hopefully stricter standards. If they are failing some basic check you can flag it to them and request a hotfix.
### PRs

A good example for how your PRs might look like (in the primary HW3 branch) is as follows, imagine I'm a chat team:

1. feat: Align to shared API

2. feat: Integrated AI system.
- verified at least 2 different AIs work
- added integration tests between our two systems
- other ai specific stuff

3. fix: Align AI system to TA feedback

4. feat: Integrated ticketing system.
- verified at least 2 different ticketing systems work
- added integration tests between our two systems
- other ticket specific stuff

5. feat: Add e2e tests & main.py
- verified full app workflow 
- stuff

### Notes on Tooling

Before we start this assignment, I want you to reflect on your dev loop. Now that you know how to make `ruff` and `mypy` work, don't just run them at the end.

I swear by a hyper-strict configuration. When you are deep in the code, specifically when working with things like ML models, untyped Python makes me get lost in the sauce. You will routinely see code where variables are passed around without context. Is `x` a tensor? A numpy array? A feature vector? A raw file path?

Strict typing prevents this confusion. To help you out, here is the VS Code configuration I use personally. It turns Python into a much more rigorous language (folks with statically typed language backgrounds will appreciate this IMO):

```json
"python.analysis.typeCheckingMode": "strict",
"python.analysis.inlayHints.variableTypes": true,
"python.analysis.inlayHints.functionReturnTypes": true,
"python.analysis.autoFormatStrings": true,
"python.analysis.enablePytestSupport": true,
"python.analysis.supportDocstringTemplate": true,
"python.analysis.typeEvaluation.strictListInference": true,
"python.analysis.typeEvaluation.strictSetInference": true
```

I think your senior engineers will be impressed when you work with care. Meaning: making tight interfaces and respecting the cognitive load of the developers who will read your code next. There is nothing cleaner than breaking a complex problem down into a set of strictly typed method signatures.