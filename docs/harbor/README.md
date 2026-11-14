# Harbor docs (local mirror)

Mirrored from https://docs.harborframework.com/ on 2026-11-13. Each page is the site's own Markdown export. `llms-full.txt` holds all pages in a single file.


- [Quick start](getting-started/quick-start.md): Run your first job.
- [Installation](getting-started/installation.md): Install Harbor.
- [Core concepts](core-concepts/index.md): Core concepts for understanding Harbor.
- [Overview](tasks/overview.md): A Harbor task is an instruction, environment, and test script.
- [Instruction](tasks/instruction.md): Defining the objective of the task for the agent.
- [Configuration](tasks/configuration.md): Task configuration, metadata, and schema reference.
- [Environment](tasks/environment.md): The environment for a task.
- [Skills](tasks/skills.md): Bundle reusable agent instructions with a task.
- [Solution](tasks/solution.md): Task solutions.
- [Verifier](tasks/verifier.md): Task verification.
- [Artifacts](tasks/artifacts.md): Declare task outputs to preserve and pass to the verifier.
- [Resources](tasks/resources.md): Managing environment resources.
- [Multi-container](tasks/multi-container.md): Multi-container documentation for Harbor.
- [Network policies](tasks/network-policies.md): Network policies documentation for Harbor.
- [Multi-step](tasks/multi-step.md): Evaluate agent performance across multiple milestones.
- [Separate verifier](tasks/separate-verifier.md): Run verification in a separate environment.
- [Lint](tasks/lint.md): Check task configuration, files, and common security risks without running a sandbox.
- [Datasets](datasets/datasets.md): Collections of tasks for evaluation and training.
- [Create a Dataset](datasets/create-a-dataset.md): Create a dataset of Harbor tasks
- [Git repos](datasets/git-repos.md): Run a dataset from a Git repo.
- [Registries](datasets/registries.md): Create custom registries of datasets.
- [Metrics](datasets/metrics.md): Custom dataset metrics.
- [Run a job](jobs/run-a-job.md): Run a job in Harbor.
- [Configs](jobs/configs.md): Complete schema for configuring Harbor jobs.
- [Environment variables](jobs/environment-variables.md): Control which environment variables reach Harbor, sandboxes, agents, and verifiers.
- [Error classification](jobs/error-classification.md): The single reference for how Harbor treats every trial error: retry, grade, and whether the reward counts.
- [Skills](jobs/skills.md): Inject reusable instructions from local directories or git repositories
- [Loading trajectories](jobs/loading-trajectories.md): Load a previous trajectory into an agent session.
- [Handoff](jobs/handoff.md): Resume a completed trial in your local agent CLI.
- [Regrade](jobs/regrade.md): Run an updated verifier against recorded outputs without rerunning the agent.
- [Simulate a user](jobs/simulate-a-user.md): Evaluate an agent through a multi-turn conversation with a simulated user.
- [Custom verifiers](jobs/custom-verifiers.md): Customize the job verification process.
- [Artifact collection](jobs/artifact-collection.md): Preserve files produced during a trial.
- [Stream](jobs/stream.md): Follow agent actions and explore sandbox files while a job runs.
- [API server](harbor-serve/api-server.md): Start a Harbor server and connect through HTTP, the CLI, the Python SDK, or MCP.
- [Pre-integrated agents](agents/pre-integrated-agents.md): Run agents supported by Harbor.
- [ACP](agents/acp.md): Run agents from the Agent Client Protocol registry.
- [Custom agents](agents/custom-agents.md): Integrate and run your own agent with Harbor.
- [ATIF](agents/atif.md): Record and exchange agent trajectories in a standard JSON format.
- [Agent auth resolution](agents/agent-auth-resolution.md): Choose models, secrets, endpoints, and API formats for your agents.
- [Base URL management](agents/base-url-management.md): Base URL management for networking and configuration.
- [Pre-integrated sandboxes](sandboxes/pre-integrated-sandboxes.md): Run tasks with local or remote environments supported by Harbor.
- [ASP](sandboxes/asp.md): Agent Sandbox Protocol: run any agent harness's tools in a remote sandbox over SSH.
- [Custom sandboxes](sandboxes/custom-sandboxes.md): Integrate and run your own sandbox with Harbor.
- [Existing plugins](plugins/existing-plugins.md): Connect Harbor jobs to observability platforms.
- [Custom plugins](plugins/custom-plugins.md): Run custom logic around Harbor jobs and trials.
- [View job results](results/view-job-results.md): Browse jobs, inspect trials, and compare results locally.
- [Handoff](results/handoff.md): Interview an agent after a completed trial.
- [Publish](harbor-hub/publish.md): Publish datasets and tasks to the Hub.
- [Upload](harbor-hub/upload.md): Upload job results to the Hub.
- [Download](harbor-hub/download.md): Download datasets, tasks, jobs, trials, and trajectories.
- [Hosted jobs](harbor-hub/hosted-jobs.md): Submit hosted jobs and check their status.
- [Leaderboards](harbor-hub/leaderboards.md): Create, rank, and share evaluation results on Harbor Hub.
- [Sharing](harbor-hub/sharing.md): Sharing tasks, datasets, and jobs.
- [Hosted Harbor](hosted-harbor/index.md): Launching Jobs On The Hub
- [Harbor Hub API key](hosted-harbor/api-key.md): Create a Harbor Hub API key
- [Web UI](hosted-harbor/web-ui.md): Add secrets and launch a remote rollout from Harbor Hub
- [CLI](hosted-harbor/cli.md): Launch remote rollouts and browse jobs, trials, and secrets from the harbor CLI
- [API overview](hosted-harbor/api.md): Base URL, authentication, and error shapes for the remote rollout API
- [Submitting jobs](hosted-harbor/submitting-jobs.md): Launch a remote rollout with POST /job-submit and check its status
- [Custom agents](hosted-harbor/custom-agents.md): Run an ACP agent from a GitHub repository
- [Managing secrets](hosted-harbor/secrets.md): Store, list, revoke, and preflight hosted secrets over the API
- [Registry credentials](hosted-harbor/registry-credentials.md): Store credentials for pulling private task images
- [Quick start](rewardkit/quick-start.md): Define and run verifiers that produce reward scores.
- [Judge criteria](rewardkit/judge-criteria.md): Grade tasks with LLM or agent judges configured in TOML.
- [Built-in criteria](rewardkit/built-in-criteria.md): Reference for all built-in criterion functions.
- [Motivation & design](rewardkit/motivation-and-design.md): Why Rewardkit exists and the principles behind its design.
- [Create a task](tutorials/create-a-task.md): Create a task documentation for Harbor.
- [Create a verifier with Rewardkit](tutorials/create-a-verifier-with-rewardkit.md): Build a task whose verifier combines programmatic criteria, weights, and an optional LLM judge.
- [Contributing](contributing/contributing.md): Contributing documentation for Harbor.
- [Release policy](contributing/release-policy.md): Harbor's stable and nightly release policy.
- [Usage stats](telemetry/telemetry.md): What Harbor collects and how to opt out.
- [Submit Job](api-sdk/jobs/submit-job.md): Submit a job, or set dry_run=true to return the resolved trial count without execution.
- [List Jobs](api-sdk/jobs/list-jobs.md): List jobs with filters and pagination. Pass next_page_token as page_token.
- [Get Job](api-sdk/jobs/get-job.md): Get a job's state, configuration, rollout counts, and lock URI.
- [Pause Job](api-sdk/jobs/pause-job.md): Pause queued rollouts in the selected jobs; running rollouts continue.
- [Resume Job](api-sdk/jobs/resume-job.md): Resume paused rollouts in the selected jobs.
- [Cancel Job](api-sdk/jobs/cancel-job.md): Cancel selected jobs. Set force=false to let running rollouts finish.
- [List Rollouts](api-sdk/rollouts/list-rollouts.md): List rollouts, optionally within a job, with filters and pagination.
- [Get Rollout](api-sdk/rollouts/get-rollout.md): Get a rollout's state, result, error information, and artifact URIs.
- [Pause Rollout](api-sdk/rollouts/pause-rollout.md): Pause selected queued rollouts. Running rollouts cannot be paused.
- [Resume Rollout](api-sdk/rollouts/resume-rollout.md): Resume the selected paused rollouts.
- [Cancel Rollout](api-sdk/rollouts/cancel-rollout.md): Cancel selected rollouts. Set force=false to let running rollouts finish.
- [Retry Rollout](api-sdk/rollouts/retry-rollout.md): Retry selected latest terminal attempts after cleanup. Returns new IDs in input order.
- [Download](api-sdk/files/download.md): Download an artifact or job lock by its registered URI.
- [Inspect file](api-sdk/files/inspect-file.md): Download an artifact or job lock by its registered URI.
- [Job Filters](api-sdk/queries/job-filters.md): Discover available job filter values and counts, optionally narrowed by other filters.
- [Rollout Filters](api-sdk/queries/rollout-filters.md): Discover rollout filter values and counts, optionally within a job.
- [Query Schema](api-sdk/queries/query-schema.md): Discover public SQL views and columns before using query_sql; optionally select one view.
- [Query Sql](api-sdk/queries/query-sql.md): Run read-only SQL on public views. Supply statements as SQL strings or [SQL, parameters...]. Use LIMIT for large results.
- [List Agents](api-sdk/agents/list-agents.md): List built-in agents without loading their optional dependencies.
- [Get Agent Options](api-sdk/agents/get-agent-options.md): Get the JSON Schema for a built-in agent's user-facing kwargs.
- [List Sandboxes](api-sdk/sandboxes/list-sandboxes.md): List built-in sandbox providers without loading their optional dependencies.
- [Get Sandbox Options](api-sdk/sandboxes/get-sandbox-options.md): Get the JSON Schema for a built-in sandbox provider's user-facing kwargs.
- [Upload Job](api-sdk/hub/upload-job.md): Upload a finished or cancelled job to Hub using the server's Hub credentials.
- [News](news.md)
- [Cracking down on reward hacking](news/reward-hacking.md): Infrastructure hardening, best practices, and tools to mitigate reward hacking in Harbor tasks.
- [Stop zipping your job results](news/job-result-sharing.md): Upload and share Harbor job results on Harbor Hub instead of zipping and sending them manually.
- [Verify Harbor tasks in a separate sandbox](news/separate-verifier-sandboxes.md): Run task verification in a sandbox separate from the agent, with explicit artifact handoff between the two environments.
- [Multi-step tasks](news/multi-step-tasks.md): The first major addition to the Harbor task format: tasks split into sequential steps, each with its own instruction and verifier.
- [Explore the Harbor cookbook](news/harbor-cookbook.md): Introducing the Harbor cookbook: recipes for building Harbor tasks and optimization loops
- [The Harbor registry is getting an upgrade](news/harbor-registry.md): Introducing a new way to package and distribute Harbor tasks and datasets.
- [Changelog](changelog.md): Release notes and product updates for Harbor.

## OpenAPI Specs

- [openapi](/api-sdk/openapi.json)

## Optional

- [Harbor Hub](https://hub.harborframework.com)
- [Discord](https://discord.gg/QVvyhRw5UQ)


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.