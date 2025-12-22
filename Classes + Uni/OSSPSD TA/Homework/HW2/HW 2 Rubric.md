The assignment is out of 100 points but there are 110 points you can get (so you have some leniency)

### 1. Repository (20pts)
- \[ 5 pts ] commit history is kept clean with low # of commits
* \[ 5 pts ] **`.gitignore`:** Ignores `.venv`, `__pycache__`, `*.pyc`, credentials (`credentials.json`, `token.json`), `.env`, build artifacts.
* \[ 5 pts ] **GitHub templates:** PR and Issue templates exist and are informative.
* \[ 5 pts ] **Commit quality:** Clean, imperative messages; no “wip” noise; logical, small commits.

---

### 2. Peer Review (10 pts)
* \[ 10 pts ] Team left constructive comments on some other teams PR that show they took care to review their changes.

---

### 3. CI (10 pts)
* \[ 6 pts ] **CI/CD:** `.circleci/config.yml` (or equivalent) runs lint (`ruff`), types (`mypy`), unit tests, and reports coverage.
* \[ 2 pts ] **CI is passing**
* \[ 2 pts ] **CI is public and we can see it**

---

### 4. Tooling & Configuration `pyproject.toml` (20 pts)

* \[ 4 pts ] **`uv` workspace:** Root `pyproject.toml` is a workspace; all `src/*` members listed.
* \[ 4 pts ] **Centralized configs:** `ruff`, `mypy`, `pytest`, coverage in root `pyproject.toml`.
* \[ 4 pts ] **Ruff strictness:** `select = ["ALL"]`; any `ignore` entries are justified with comments.
* \[ 4 pts ] **Mypy strictness:** `strict = true` with narrow, documented exceptions.
* \[ 4 pts ] **Deps hygiene:** Dev deps under `[project.optional-dependencies]`; runtime deps minimal per package ; dependencies of a component are not forced into the main pyproject.toml

---

### 5. Architecture (20 pts)

* \[ 4 pts ] **`src` layout:** All installable packages live under `src/`.
* \[ 4 pts ] **Clear componentization:** Interfaces/ports and implementations are split (e.g., `ticket_service_api` vs. `jira_ticket_service_impl`; `ticket` vs. `jira_ticket_models_impl`).
* \[ 4 pts ] **Interface purity:** Protocol/port/api packages have no dependencies on frameworks or concrete SDKs (no FastAPI/Pydantic/Jira SDK in api).
* \[ 4 pts ] **Dependency injection:** Implementation packages properly register/override factories in the API/port package.
* \[ 4 pts ] **Single Responsibility:** Each component does one thing (ports define contracts & implementations talk to external systems).

---

### 6. Domain Modeling & API Design (8 pts)

* \[ 2 pt ] **DTO Separation:** Business actions belong on the High Level Ports not on the data classes.
* \[ 2 pts ] **Framework-agnostic domain:** Use built-ins (dataclass or ABC) in domain/ports; use **Pydantic only at edges** (HTTP/adapters) for validation/serialization.
* \[ 1 pt ] **Error modeling:** Ports should raise typed errors instead of returning `None` for control flow.
* ~~\[ 1 pt ] **Async API consistency:** If adapters are async (e.g., HTTP), ports are `async` and consistently awaited throughout.~~ strikethrough cs might use later
* \[ 1 pt ] **Invariants & workflow:** Workflow/permission checks are enforced in **service/port/adapters**, not encoded as field tweaks on DTOs.
* \[ 2 pts ] **No leakage:** External concerns (auth tokens, HTTP clients, SDK types) do **not** leak into domain/port packages.
* ~~\[ 1 pt ] **Idempotency & retries:** Adapter behavior for retries, rate limits, and idempotency is documented and (where needed) implemented.~~ strikethrough cs might use later

---

### 7. Testing Strategy (“Testing Pyramid”) 12 pts

* \[ 4 pts ] **Unit tests (per component):**
  * Live under each component’s `tests/`.
  * Use `unittest.mock`/fakes for external services (no network).
  * Verify ports raise proper domain exceptions; DTOs remain behavior-free.
* \[ 4 pts ] **Integration tests (top-level `tests/integration/`):**
  * Validate DI wiring: calling the abstract factory returns the concrete adapter.
  * Exercise a real adapter path against a sandbox or recorded responses (or 1 live call when required), asserting domain mapping and error translation.
* \[ 3 pts ] **E2E tests (top-level `tests/e2e/`):**
  * Run the app entry point as a subprocess; black-box assertions on user-visible behavior.
* \[ 1 pt ] **Coverage:** Threshold defined in `pyproject.toml`; rationale documented.

---

### 8. Documentation (10 pts)

* \[ 4 pts ] **Root `README.md`:** Purpose, architecture, ports/adapters philosophy, setup/auth, how to run toolchain.
* \[ 2 pts ] **Component READMEs:** Each `src/*` package documents its API, dependencies, and role.
* \[ 2 pts ] **Architecture doc:** A `component.md` (or similar) describing what a component is.
* \[ 2 pts ] **MkDocs site:** `mkdocs.yml` present; `mkdocs` looks and clean updated.
