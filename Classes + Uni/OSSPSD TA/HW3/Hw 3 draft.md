Here is an improved version of the assignment, integrating your requests for clarification, telemetry requirements, and infrastructure as code.

***

## The Assignment

### Objective

For this final assignment, you are no longer building in isolation. You will be building a **Smart Chat Bot** that integrates three distinct domains: **Chat, Tickets, and AI**. This bot will function as a deployed application, complete with monitoring and infrastructure managed as code.

The final product must be able to:
*   **Answer questions** by leveraging an AI model.
*   **Fetch and summarize tickets** from a ticketing system (e.g., "Show me my 3 most recent open tickets").
*   **Take action on tickets** (e.g., "Close this ticket").

The core challenge of this project lies in collaboration and integration. You will not be writing code for the systems you did not build. Instead, you will pull the API references for the other two systems, apply dependency injection, and make them work seamlessly with your component. This will require negotiation, agreeing on shared API contracts (using Abstract Base Classes), and potentially using Git submodules to manage dependencies.

### Core Pillars of the Project

1.  **Integration:** Your primary task is to make three disparate systems—Chat, Tickets, and AI—communicate effectively.
2.  **Deployment:** This is not a local-only project. You will deploy your application and manage its infrastructure as code.
3.  **Observability:** Your deployed application must emit telemetry data to monitor its health and performance.

### Instructions

#### 1. Interface Deliverable & API Alignment

Your first task is to collaborate within your vertical (Chat, Tickets, AI) to define a standardized API that all teams in that vertical will implement. This shared interface is crucial, as it will be the contract that the other two verticals depend on to integrate your system.

**Process:**
1.  Meet with the other teams in your vertical.
2.  Define a common API that abstracts away the specific details of each team's implementation (e.g., Discord vs. Slack). This should be defined using Python's `abc` (Abstract Base Classes) module.
3.  One representative from the vertical must submit a memo detailing the agreed-upon API.
4.  Every team must include a small plan of action describing how they will adapt their existing codebase to this new, standardized interface.

This is a **shared grade** per vertical. Failure to agree on and document a viable API will impact every team in that vertical.

---

##### **Vertical-Specific Guidance:**

**Chat Teams (Teams 1, 4, 7)**
*   **Services:** Discord, Slack
*   **Challenge:** Your APIs must abstract the differences between platforms. For instance, Team 4's Slack implementation lacks a delete message feature, and Team 1's Discord bot did not implement channels. The shared API must account for these potential differences gracefully.
*   **Suggested API Methods:**
    *   `get_messages(channel_id: str, limit: int) -> List[Message]`
    *   `get_message(message_id: str) -> Message`
    *   `send_message(channel_id: str, message_text: str) -> Message`
    *   `delete_message(message_id: str) -> None`
    *   `get_channels() -> List[Channel]`
    *   `get_channel(channel_id: str) -> Channel`

**Ticket Teams (Teams 2, 6, 8)**
*   **Services:** Google Tasks, Jira, Trello
*   **Challenge:** You need to unify list-based systems (Jira, Google Tasks) with a Kanban-board system (Trello). The key is to find a common abstraction. A Kanban board can be viewed as a collection of lists (e.g., "To Do," "In Progress," "Done"), so the Trello API can be adapted to expose a list-based interface.
*   **Suggested API Abstractions:**
    *   A `Ticket` data class with fields like `id`, `title`, `status`, `description`.
    *   `get_tickets(status: str = 'open') -> List[Ticket]`
    *   `get_ticket(ticket_id: str) -> Ticket`
    *   `create_ticket(title: str, description: str) -> Ticket`
    *   `update_ticket_status(ticket_id: str, new_status: str) -> None`

**AI Teams (Teams 3, 5, 9, 10)**
*   **Services:** OpenAI, Gemini, Claude
*   **Challenge:** The primary task is to standardize the input/output for interacting with the language model. You also need to devise a common authentication strategy that works for all providers.
*   **Suggested API Methods:**
    *   `send_message(prompt: str, context: Optional[Dict] = None) -> str`
    *   You must also agree on a unified approach for handling authentication keys and credentials for the different services.

---

#### 2. Infrastructure as Code & Telemetry

Your application must be deployed, and its infrastructure must be managed using an **Infrastructure as Code (IaC)** tool like Terraform or AWS CloudFormation. You are responsible for provisioning the necessary resources (e.g., servers, databases, environment variables) in a repeatable and automated way.

Furthermore, your deployed application must emit **telemetry data**. This is non-negotiable and critical for understanding the performance of a live service. You must implement monitoring for:
*   **Request Latency:** How long does each API call or user interaction take?
*   **Success Rate:** What percentage of requests are completed successfully?
*   **Failure Rate:** What percentage of requests result in errors?

You should use a monitoring or observability platform to collect and visualize this data.

#### 3. Integration & Submissions

The project is divided into three main submission milestones.

##### **First Submission: API Alignment**
By the turn of the month, your individual team's project must be refactored to implement the shared, standardized API for your vertical. This is a hard deadline, as other teams will be depending on this standardized interface for the next phase.

##### **Second Submission: First Integration**
Choose **one** of the other two verticals (either Tickets or AI) and integrate their system into your project. For example, if you are a Chat team, you might integrate an AI system first. Your submission must:
*   Demonstrate successful integration with at least two different providers from that vertical (e.g., a Chat bot working with both OpenAI and Gemini).
*   Include integration tests that verify the two systems are working together correctly.

##### **Final Submission: Full System & Demonstration**
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
*   **12/03 - First Submission:** All teams must have their code aligned with the shared API.
*   **12/03:** TAs will provide feedback on API alignment and interface designs.
*   **12/07 - Second Submission:** First cross-vertical integration is complete.
*   **12/10:** Peer feedback and TA feedback on the first integration.
*   **12/17 - Final Submission:** Final project, including the video demonstration, is submitted.
    *   **Note:** We can be flexible and extend this deadline to **December 21st**. The 17th can be a suggested deadline, with no penalty for submissions up to the 21st to accommodate proximity to the holidays. Please provide feedback on this.