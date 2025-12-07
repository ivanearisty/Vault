Here is a draft for the announcement.

**Subject:** Updates: HW3 Scope, Deadlines, and Shared Interfaces

Hi everyone,

We are making some adjustments to the final stretch to ensure everyone crosses the finish line with a solid project. We are updating the deadlines to the following:

*   **11/21:** Draft of this assignment is provided.
*   **11/26:** Draft is frozen.
*   **12/27 - Interface Deliverable:** Verticals must submit their agreed-upon shared interface memo and individual team alignment plans. *(Note: Please double check this date, as it falls after the submissions below).*
*   **12/8 - First Submission:** All teams must have their code aligned with the shared API.
*   **12/12 - Second Submission:** First cross-vertical integration is complete.
*   **12/14:** Peer feedback and TA feedback on the first integration.
*   **12/19 - Final Submission:** Final project, including the video demonstration, is submitted.

**The Source of Truth for Interfaces**

To streamline this process so I don't have to go 1-by-1 giving feedback on every single memo, I have created a repository containing the Abstract Base Classes (ABCs) that serve as the standard for each vertical.

You should look at this repo immediately: **https://github.com/ivanearisty/OSS-APIs**

This is now the single source of truth. However, I don't know your projects better than you do. If you spot an error, or if you simply don't like a specific design choice, **fork the repo and submit a PR.** Rationalized changes are welcome.

**TL;DR on the Architecture Changes:**

1.  **Chat:** We stripped the Chat interface down to a pure I/O pipe. It shouldn't worry about channel management logic, just moving messages.
2.  **AI:** We simplified the AI interface to focus on **Structured Output** rather than Tool Calling. If you do not know what Structured Output is, please look it up.
3.  **Tickets:** Standardized to abstract away the differences between Kanban and List views.

I think the AI teams in particular will be happy to see their interface :)

**HW3 Document Update**

Finally, a stripped-down version of the HW3 assignment document has been uploaded. It contains the updated deadlines, a revised **Overview** section, and a new section on **User Flow** to help clarify exactly how these components should interact.

Good luck with the integration.