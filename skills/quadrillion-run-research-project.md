---
name: Run a research project with notebook execution
description: Create a project, start a kernel for its notebook, execute code, and record claims and evidence in the knowledge graph.
api: openapi/quadrillion-cloud-openapi-original.json
operations: [create_project_endpoint_api_projects_post, get_project_tasks_api_projects__project_id__tasks_get, start_kernel_v1_kernel_kernels_start_post, execute_code_v1_kernel_kernels__notebook_id__execute_post, list_project_claims_api_projects__project_id__claims_get, add_evidence_endpoint_api_evidence_post]
---

# Run a research project with notebook execution

Drive a Qualia research project through the Quadrillion Cloud API: set up the project, run code in a compute kernel, and capture findings in the central knowledge graph.

## Steps

1. **Create the project** — `POST /api/projects` (`create_project_endpoint_api_projects_post`). Keep the returned `project_id`.
2. **Inspect tasks** — `GET /api/projects/{project_id}/tasks` (`get_project_tasks_api_projects__project_id__tasks_get`) to see the project's task graph.
3. **Start a kernel** — `POST /v1/kernel/kernels/start` (`start_kernel_v1_kernel_kernels_start_post`) for the notebook you are working in.
4. **Execute code** — `POST /v1/kernel/kernels/{notebook_id}/execute` (`execute_code_v1_kernel_kernels__notebook_id__execute_post`) to run analysis in the kernel.
5. **Record findings** — list existing claims with `GET /api/projects/{project_id}/claims` (`list_project_claims_api_projects__project_id__claims_get`), then attach supporting evidence with `POST /api/evidence` (`add_evidence_endpoint_api_evidence_post`) so results stay auditable and sourced.

## Rules

- Authenticate every call with a valid Quadrillion API key or WorkOS SSO session (`authentication/quadrillion-authentication.yml`).
- Stop or restart a kernel via `/v1/kernel/kernels/{notebook_id}/stop` and `/restart` rather than leaving compute running.
- Validation errors return HTTP 422 with a `detail[]` array; handle them per `errors/quadrillion-problem-types.yml`.
