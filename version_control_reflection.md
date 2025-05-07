# Version‑Control Reflection

*A summary of how our team applies Git practices to support DevOps objectives.*

## 1. Chosen Model

* **GitLab Workflow** with two long‑lived branches:

  * `main` (protected) → production
  * `dev` → integration
* Provides clear separation of concerns without the release‑branch overhead of full GitFlow.

## 2. Operational Flow

| Phase                  | Action                                                                                                                                                                            |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Feature implementation | Create short‑lived **feature** branch from `dev` (e.g., `feature/*`).                                                                                           |
| Validation             | Merge Request must pass automated tests & receive ≥ 1 peer review.                                                                                                                |
| Continuous Integration | Full CI suite (unit tests, linting) runs on the pull request into `dev`. After merge, only Playwright E2E tests execute. Promotion of `dev` → `main` is currently manual. |
| Continuous Delivery    | VM pulls `main` nightly; container images are rebuilt and tagged `latest`.                                                                                                        |

### Branch‑trigger Matrix

| Trigger                          | Workflow / Jobs                                                    | Branches           |
| -------------------------------- | ------------------------------------------------------------------ | ------------------ |
| **Push**                         | Playwright End‑to‑End tests (`e2e`)                                | `main`, `dev`      |
| **Push**                         | Continuous Delivery – build & push Docker image (`build-and-push`) | `main`             |
| **Workflow Run**<br>(CD success) | Continuous Deployment (`deploy`)                                   | `main`             |
| **Pull Request**                 | CI suite: Ruby tests, RuboCop, Dockerfile lint                     | Into `main`, `dev` |

## 3. Impact

* **Reduced integration risk** — small, frequent merges keep drift minimal.
* **Consistent quality** — mandatory reviews plus automated unit, integration, and E2E tests.
* **Short lead time to production** — average merge‑to‑deploy < 24 h.

## 4. Improvement Backlog

* Automate fast‑forward merge from `dev` → `main` once E2E tests pass.
* Automate back‑merge from `main` → `dev` for hot‑fix parity.

## 5. Lessons. Lessons

* Prioritise fast, automated feedback loops.
* Automate wherever manual steps add no value.
* Re‑evaluate the workflow regularly; process is iterative.

*Last updated: 2025‑05‑07*
