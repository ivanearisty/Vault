# HW1 Grading Rubric — Spring '26

The assignment is out of 100 points but there are 110 points you can get (so you have some leniency).

---

### 1. Repository & Process (15 pts)

- [ 3 pts ] **PR submitted correctly:** `hw-1` branch → `main`, **not merged**.
- [ 3 pts ] **Repository structure:** Correct layout; all installable packages live under `src/` or `components/`. No stray top-level modules.
- [ 3 pts ] **`.gitignore`:** Ignores `.venv`, `__pycache__`, `*.pyc`, credentials (`credentials.json`, `token.json`), `.env`, build artifacts.
- [ 3 pts ] **Commit quality:** Clean, imperative messages; no noise; logical, small commits. PR has descriptive title and clear summary following `pull_request_template.md`.
- [ 3 pts ] **GitHub templates:** PR and Issue templates exist and are informative.

---

### 2. Tooling & Configuration (15 pts)

- [ 3 pts ] **`uv` workspace:** Root `pyproject.toml` is a workspace; all members listed. No `requirements.txt` or `pip`.
- [ 3 pts ] **Centralized configs:** `ruff`, `mypy`, `pytest`, coverage settings in root `pyproject.toml` only.
- [ 3 pts ] **Ruff strictness:** `select = ["ALL"]`; any `ignore` entries are justified with comments.
- [ 3 pts ] **Mypy strictness:** `strict = true` with narrow, documented exceptions. No blanket `type: ignore` without justification.
- [ 3 pts ] **Import hygiene:** Only absolute imports used. No `__all__` in `__init__.py`. No `__init__.py` in `tests/` directories.

---

### 3. Interface Component (`[vertical]_client_api`) (15 pts)

- [ 5 pts ] **Abstract contract:** Interface defined using ABCs in a dedicated `.py` file (not `__init__.py`). Methods clearly define what the client does, not how.
- [ 5 pts ] **Interface purity:** No dependencies on implementation details, frameworks, or concrete SDKs. No leakage of auth tokens, HTTP clients, or provider-specific types.
- [ 5 pts ] **Dependency injection setup:** Abstract `get_client()` factory method defined or similar. Interface package has zero coupling to the implementation package.

---

### 4. Implementation Component (`[vertical]_client_impl`) (15 pts)

- [ 5 pts ] **Implements interface:** Concrete class inherits from the abstract interface. All abstract methods implemented.
- [ 5 pts ] **Dependency injection:** Importing the implementation package automatically registers/injects itself via the interface's `get_client()` factory (or similar). Users code against the interface, not the implementation.
- [ 5 pts ] **Authentication & credentials:** Auth handled appropriately for chosen provider (OAuth, API keys, etc.). Credentials never hardcoded; environment variables used for all secrets.

---

### 5. Testing Strategy (20 pts)

- [ 6 pts ] **Unit tests (per component):**
  - Live under each component's `tests/`.
  - Implementation tested with mocked provider APIs (no network calls).
  - Tests are fast, isolated, and deterministic.
- [ 5 pts ] **Integration tests (`tests/integration/`):**
  - Verify Dependency Injection wiring.
- [ 5 pts ] **E2E tests (`tests/e2e/`):**
  - Run the complete workflow against real infrastructure with test credentials.
  - Validate: client creation → API call → response handling.
- [ 2 pts ] **Coverage:** Threshold defined in `pyproject.toml`; intentionally untestable lines marked with `# pragma: no cover`.
- [ 2 pts ] **Test results visible:** Results accessible in CircleCI's Tests dashboard.

---

### 6. CI/CD (10 pts)

- [ 4 pts ] **CircleCI pipeline:** `.circleci/config.yml` runs lint (`ruff`), types (`mypy`), and all tests (unit, integration, E2E).
- [ 2 pts ] **CI is passing** on the `hw-1` branch.
- [ 2 pts ] **CI is public** and reviewers can see it.
- [ 2 pts ] **Coverage reports:** Uploaded and browsable from the CircleCI UI.

---

### 7. Documentation (10 pts)

- [ 3 pts ] **Root `README.md`:** Purpose, architecture overview, setup/auth instructions, how to run the toolchain.
- [ 2 pts ] **Component READMEs:** Each `src/*` (or `components/*`) package documents its API, dependencies, and role.
- [ 2 pts ] **`contributing.md` & `design.md`:** Contributing guide and design document for the new components exist and are informative.
- [ 3 pts ] **MkDocs:** `mkdocs.yml` present, `docs/` directory populated, documentation builds without errors.

---

### 8. Peer Review (10 pts)

- [ 5 pts ] **Review given:** Team left constructive, substantive comments on assigned team's PR that show genuine engagement with their code and design.
- [ 5 pts ] **`contributing.md` & `design.md` quality:** Documents are thorough enough for an outside contributor to understand the project and the design decisions behind the new components.

---

### Extra Credit (+10 pts)

Up to +10 points for going above and beyond. Examples:

- Exceptionally clean interface design with notably low surface area and high functionality.
- Comprehensive error modeling with typed domain exceptions.
- Resilience patterns (retries, rate-limit handling, idempotency).

---

### First Draft

The first draft deadline is for feedback, not grading. However, teams that submit nothing or an empty PR by the first draft deadline forfeit the iterative feedback cycle, which historically correlates with lower final scores. The final submission is what gets graded.