> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Changelog

> Release notes and product updates for Harbor.

**October 25, 2026**

v0.25.0

* **Task linting:** Check task configuration, files, dependencies, and verifier
  setup with `harbor lint`. Supports JSON output and rule selection in CLI
  flags, `task.toml`, and `dataset.toml`.
* **Agent auth:** Configure inference with shared `--base-url`, `--api-key-name`,
  and `--api-format` flags. Inspect supported settings with
  `harbor agent auth <agent>`.
* **Failure classification:** Trial errors now share a policy for retries,
  verification, and whether rewards count toward results.
* **Task file exclusions:** Use `.harborignore` in tests and solution directories
  to exclude files from sandbox uploads, publication, and task content hashes.
* **Network controls:** Restricted runs automatically allow resolved inference
  hosts. Added `--allow-verifier-host` to extend verifier allowlists.
  `no-network` and `allowlist` now disable supported agents' built-in web tools
  by default; explicit `disable_web_search` settings take precedence.
* **New integrations:** Added NVIDIA NeMo Fabric, plus Sail, Docker Sandboxes
  (`sbx`), and Kernel environments.
* **Trajectories:** Kimi Code now records ATIF trajectories and token usage.
  Simulated-user artifacts are preserved in uploaded result archives.
* **RewardKit:** Added `Worker` to run submitted code as an unprivileged user
  inside the verifier container. Agent judges now receive `reasoning_effort`.

> [!WARNING]
>
> **Breaking changes:** New tasks created with `harbor tasks init` or
> `harbor init --task` now use separate verifiers by default. To opt out, set
> `[verifier].environment_mode = "shared"` and install verifier dependencies
> in the agent image.
>
> Local sandbox logs and artifacts are collected after execution instead of
> automatically mounted on the host. Use `--ek auto_mounting=true` to restore
> live mounts. The default skills path moved from `/harbor/skills` to
> `/runtime/skills` (`C:/runtime/skills` on Windows); update tasks and verifiers
> that read the old path.
>
> RewardKit's `all-pass`, `any-pass`, and `required-pass` now require scores
> of at least `1.0` to count as passing. Use `weighted-mean` or `threshold`
> for partial credit.
>
> Regrading now rejects symlinks outside recorded outputs, dangling links,
> and cycles. Collect referenced files into the trial's recorded outputs.
> See the [repository changelog](https://github.com/harbor-framework/harbor/blob/v0.25.0/CHANGELOG.md)
> for additional migration details.

**October 5, 2026**

v0.24.0

* **Live streaming:** Follow agent trajectories and browse sandbox files during
  a run with `--stream`. Supports Claude Code and Codex on Docker, Daytona,
  Modal, Tensorlake, and Smol Machines.
* **Results viewer:** Added Pareto and Outcomes views, range filters, and
  improved evaluation tables and trial navigation.
* **Job comparison:** Use `harbor run --diff` to compare against previous jobs
  and preview which trials can be reused, regraded, or rerun.
* **Agent controls:** Added `--effort` and a common `disable_web_search` option
  for supported agents. Harbor now validates MCP and skills capabilities
  before starting a sandbox.
* **New integrations:** Added the Code Puppy agent and Prime, Mosaic, and
  Smol Machines sandboxes. Docker now supports GPU allocation through Compose.
* **Simulated users:** Codex and OpenCode can now act as primary agents in
  simulated-user evaluations.
* **Hosted workflows:** Regrade jobs and trials on Harbor Hub with `--launch`,
  share jobs during launch, and share packages with organizations when publishing.
* **RewardKit:** Added JEV judges, rubric criteria, repeated judge sampling,
  and cost estimation for agent and LLM judges.

> [!WARNING]
>
> **Breaking changes:** RewardKit now groups criteria by Python file.
> Migrate directory-wide scoring to per-file configuration.
>
> Separate verifiers now prefer their own images and build definitions,
> falling back to the agent's definition with tests uploaded to `/tests`.
> Dedicated verifier images still need bundled tests.

**September 12, 2026**

v0.23.0

* **Agent options:** Discover supported settings with `harbor agent schema <name>`.
  Built-in agents validate options before starting a sandbox.
* **Job configuration:** Compose config files and validate runs with
  `harbor run --dry-run` before downloading tasks or starting environments.
* **New integrations:** Added Muse Code, Strands, and the FX dev-channel agent,
  plus Podman, Runta, and Kata sandboxes.
* **Trajectories:** ATIF v1.8 supports audio content. Agent results now record
  token usage by model.
* **RewardKit:** Judges can receive trajectory images and audio. Added FX agent
  judges and signed weighted aggregation.
* **Multi-step tasks:** Added Hermes session resume and regrading for completed
  multi-step trials.
* **Hosted workflows:** Added Hub job renaming, ownership transfer, and agent
  catalog discovery.

**August 22, 2026**

v0.22.0

* **Simulated users:** Run multi-turn evaluations in which a user agent and a
  primary agent communicate through ACP.
* **Trajectory loading:** Tasks can provide prior ATIF context through
  `trajectory.json` before the agent's first turn.
* **New agents:** Added MCode, Junie, and FX integrations.
* **New sandboxes:** Added Hyperbrowser and Vercel Sandbox integrations.
* **Hosted workflows:** Added hosted custom agents, resumable trial uploads,
  and task and dataset sharing commands.
* **RewardKit:** Added nested dimension groups and simplified agent judges.

**August 10, 2026**

v0.21.0

* **Trial handoff:** Resume a completed Claude Code session locally to ask the
  agent about its run.
* **Regrading:** Use `harbor job regrade` or `harbor trial regrade` to run a
  new verifier against recorded trials without rerunning the agent.
* **Trajectory loading:** Claude Code and Codex can load native or ATIF
  trajectories at run time.
* **Package versioning:** Task and dataset packages now record versions and
  warn when a selected version has been yanked.
* **New agents:** Added Cortex Code, Kimi Code, and Google Antigravity.
* **New sandbox:** Added Hugging Face Sandbox.

> [!WARNING]
>
> **Breaking security change:** GKE sandboxes no longer mount Kubernetes
> service-account credentials. Workloads that need cluster access must provide
> credentials explicitly.

> [!NOTE]
>
> This changelog highlights user-facing stable releases and is not an exhaustive
> commit history. See the [repository changelog](https://github.com/harbor-framework/harbor/blob/main/CHANGELOG.md),
> [GitHub releases](https://github.com/harbor-framework/harbor/releases), or
> [PyPI history](https://pypi.org/project/harbor/#history) for earlier releases.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.