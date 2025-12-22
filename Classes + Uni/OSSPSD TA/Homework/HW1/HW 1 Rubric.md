HW1 Grading Rubric (Total: 100 pts)  
1. First Draft Submission (5 pts)  

- **Deadline:** Friday, October 3rd at Midnight
- PR submitted by deadline (5 pts)

2. Repository & Process (10 pts)  

- PR submitted correctly (hw-1 branch → root, not merged) (3 pts)
- Correct repository structure maintained (4 pts)
- Documentation updated (README.md and component-level docs) (3 pts)

3. FastAPI Service Implementation (25 pts)  

- Service structured as a package but is an application – has main/runner (5 pts)
- At least 2-3 endpoints implemented and working (5 pts)
- Server-side: FastAPI routes use the interface (not concrete implementation directly) (10 pts)
- Dependency injection configured correctly (imports trigger monkey patching) (5 pts)

4. Auto-Generated Client & Adapter (20 pts)  

- Auto-generated client exists (5 pts)
- Adapter package exists (5 pts)
- Adapter implements the mail_client_api interface (10 pts)
- Adapter correctly wraps/calls methods on the auto-generated client (5 pts)

5. Testing (15 pts)  

- Unit tests exist for service (5 pts)
- Unit tests exist for adapter (5 pts)
- Tests demonstrate understanding of mocking vs real calls (5 pts)

6. Code Quality & CI (10 pts)  

- CircleCI configured correctly and passing(4 pts)
- ruff and mypy checks implemented correctly and strict (3 pts)
- Code Quality Checks in repo.  (3 pts)

7. Documentation & Peer Review (10 pts)  

- **Deadline:** Tuesday, October 9th at Midnight
- contributing.md and design.md exist (5 pts)
- Peer reviewed assigned team (5 pts)