> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Configs

> Complete schema for configuring Harbor jobs.

```bash theme={"system"}
harbor run -c "<config.yaml>"
```

Job config files provide the same controls as `harbor run` flags while supporting multiple agents, datasets, and tasks in one reusable JSON or YAML file.

Example:

```yaml config.yaml theme={"system"}
job_name: my-first-job
tasks:
  - path: "<task-path>"
agents:
  - name: codex
    model_name: openai/gpt-6-astra
environment:
  type: docker
```

## Creating a config

You can use

```bash theme={"system"}
harbor job init
```

to generate a config. It accepts the same flags as `harbor run`.

To generate a config with all available fields:

```bash theme={"system"}
harbor job init "<config.yaml>" --full
```

Use `--print-config` to inspect the resolved config without running the job:

```bash theme={"system"}
harbor run --config "<config.yaml>" --print-config
```

Use `harbor job schema` to print the config JSON Schema:

```bash theme={"system"}
harbor job schema
```

Use `--dry-run` to check the config, agent kwargs, credentials, and task sources without downloading tasks or running the job:

```bash theme={"system"}
harbor run --dry-run --config "<config.yaml>"
```

Add `--launch` to use Harbor Hub schemas and validation.

> [!NOTE]
>
> All top-level fields are optional. Harbor applies the defaults shown below. Fields marked as required are required only when their containing object is present.

## Job

**job\_name** `string` (default: current timestamp)

Job name. When omitted, Harbor uses the current time in `YYYY-MM-DD__HH-MM-SS` format.

**jobs\_dir** `string` (default: "jobs")

Directory where Harbor stores job results.

**n\_attempts** `integer` (default: 1)

Number of attempts for every task and agent combination.

**install\_only** `boolean` (default: false)

Run agent setup only, skipping the agent phase and disabling verification.

**timeout\_multiplier** `number` (default: 1.0)

Multiplier applied to task timeouts unless a phase-specific multiplier overrides it.

**agent\_timeout\_multiplier** `number | null` (default: null)

Multiplier applied to the agent execution timeout.

**verifier\_timeout\_multiplier** `number | null` (default: null)

Multiplier applied to the verifier timeout.

**agent\_setup\_timeout\_multiplier** `number | null` (default: null)

Multiplier applied to the agent setup timeout.

**environment\_build\_timeout\_multiplier** `number | null` (default: null)

Multiplier applied to the environment build timeout.

**debug** `boolean` (default: false)

Enable debug logging.

**n\_concurrent\_trials** `integer` (default: 4)

Maximum number of concurrent trials. Must be at least 1. Per-agent concurrency limits cannot exceed this value.

**quiet** `boolean` (default: false)

Suppress individual trial progress displays.

**retry** `RetryConfig` (default: \{})

Retry and exponential-backoff configuration.

**override\_error\_policy** `dict[string, ErrorPolicyOverride]` (default: \{})

Override `should_retry`, `should_grade`, or `has_valid_reward` by exception type. Omitted fields keep their defaults. See [Error classification](https://docs.harborframework.com/jobs/error-classification.md#overriding-the-policy).

**environment** `EnvironmentConfig` (default: \{})

Shared environment-provider configuration applied to every trial.

**verifier** `VerifierConfig` (default: \{})

Shared verifier configuration applied to every trial.

**metrics** `list[MetricConfig]` (default: \[])

Job-level metrics appended to each dataset's metrics.

**agents** `list[AgentConfig]` (default: \[\{"name":"oracle"}])

Agents evaluated by the job.

**user\_agent** `UserAgentConfig | null` (default: null)

Optional simulated-user agent and bridge applied to every trial.

**datasets** `list[DatasetConfig]` (default: \[])

Dataset sources expanded into tasks.

**tasks** `list[TaskConfig]` (default: \[])

Individual task sources.

**artifacts** `list[string | ArtifactConfig]` (default: \[])

Environment paths collected after each trial. A string is shorthand for an artifact with only `source` set.

**extra\_instruction\_paths** `list[string]` (default: \[])

Files appended to each task instruction, before `extra_instructions`.

**extra\_instructions** `list[string]` (default: \[])

Inline text appended to each task instruction after `extra_instruction_paths`.

**source\_jobs** `list[SourceJobConfig]` (default: \[])

Source jobs used for regrading. When set, Harbor derives one new trial from each matching source trial instead of expanding tasks, agents, and attempts normally.

## Retry

**retry.max\_retries** `integer` (default: 0)

Maximum retry attempts. Must be at least 0.

**retry.include\_exceptions** `set[string] | null` (default: null)

Exception class names eligible for retries. `null` includes every exception not excluded below.

**retry.exclude\_exceptions** `set[string] | null` (default: built-in non-retryable exceptions)

Exception class names that are never retried. Exclusions take precedence over inclusions. The defaults are the non-retryable types in [Error classification](https://docs.harborframework.com/jobs/error-classification.md).

**retry.wait\_multiplier** `number` (default: 1.0)

Multiplier for exponential-backoff wait times.

**retry.min\_wait\_sec** `number` (default: 1.0)

Minimum delay between retries in seconds.

**retry.max\_wait\_sec** `number` (default: 60.0)

Maximum delay between retries in seconds.

## Agents

**agents\[].name** `string | null` (default: "oracle")

Pre-integrated agent name. Use `import_path` for a custom agent.

**agents\[].import\_path** `string | null` (default: null)

Custom agent import path in `module.path:ClassName` format.

**agents\[].model\_name** `string | null` (default: null)

Model identifier passed to the agent.

**agents\[].n\_concurrent** `integer | null` (default: null)

Per-agent limit on concurrent `agent.run()` phases. Must be at least 1 and cannot exceed `n_concurrent_trials`.

**agents\[].concurrency\_group** `string | null` (default: null)

Shared concurrency-pool name. Agents in the same group must set the same `n_concurrent` value.

**agents\[].skills** `list[string]` (default: \[])

Local skill directories, Git URLs, or `org/name[@ref]` skill sources.

**agents\[].override\_timeout\_sec** `number | null` (default: null)

Replace the task's agent timeout with this value in seconds.

**agents\[].override\_setup\_timeout\_sec** `number | null` (default: null)

Replace the agent setup timeout with this value in seconds.

**agents\[].max\_timeout\_sec** `number | null` (default: null)

Upper bound for the effective agent timeout in seconds.

**agents\[].resume\_trajectory** `boolean` (default: false)

Resume the agent's native session between steps of a multi-step task. Requires agent resume support.

**agents\[].load\_trajectory** `string | null` (default: null)

Load a native `.jsonl` or ATIF `.json` trajectory before the first step. Requires the corresponding agent capability.

**agents\[].extra\_allowed\_hosts** `list[string]` (default: \[])

Hostnames, IP addresses, or CIDR ranges added to the allowlist during `agent.run()` only.

**agents\[].include\_logs** `list[string]` (default: \[])

Glob patterns selecting agent log files to download.

**agents\[].exclude\_logs** `list[string]` (default: \[])

Glob patterns excluded from downloaded agent logs after applying `include_logs`.

**agents\[].kwargs** `object` (default: \{})

Integration-specific keyword arguments passed to the agent constructor.

**agents\[].env** `object[string, string]` (default: \{})

Environment variables exposed only during the agent phase.

**agents\[].mcp\_servers** `list[MCPServerConfig]` (default: \[])

MCP servers made available to the agent.

See [Pre-integrated agents](https://docs.harborframework.com/agents/pre-integrated-agents.md), [Custom agents](https://docs.harborframework.com/agents/custom-agents.md), [Skills](https://docs.harborframework.com/jobs/skills.md), and [Loading trajectories](https://docs.harborframework.com/jobs/loading-trajectories.md).

### MCP servers

**agents\[].mcp\_servers\[].name** `string`

Required server name.

**agents\[].mcp\_servers\[].transport** `"stdio" | "sse" | "streamable-http"` (default: "sse")

MCP transport. The legacy value `http` is normalized to `streamable-http`.

**agents\[].mcp\_servers\[].url** `string | null` (default: null)

Server URL. Required for `sse` and `streamable-http` transports.

**agents\[].mcp\_servers\[].headers** `object[string, string]` (default: \{})

HTTP request headers for `sse` and `streamable-http` servers. Supported by `claude-code` and `codex`. Values support embedded `${VAR}` and `${VAR:-default}` references resolved before the agent is set up. Credential headers must use `${VAR}` references. Other agents reject configurations with headers. See [MCPs](https://docs.harborframework.com/tasks/environment.md#mcps).

**agents\[].mcp\_servers\[].command** `string | null` (default: null)

Executable command. Required for the `stdio` transport.

**agents\[].mcp\_servers\[].args** `list[string]` (default: \[])

Arguments passed to a `stdio` server command.

## Simulated user

`user_agent` supports every field in `agents[]`, plus the fields below. The `bridge` field is required when `user_agent` is configured.

**user\_agent.user\_persona\_path** `string | null` (default: null)

Path to a file defining the simulated user's persona.

**user\_agent.user\_prompt\_template\_path** `string | null` (default: null)

Path to the Jinja2 prompt template used by the simulated user.

**user\_agent.bridge** `BridgeConfig`

Required bridge connecting the simulated user to the primary agent.

**user\_agent.bridge.kind** `"acp"`

Required bridge implementation. Currently only `acp` is supported.

**user\_agent.bridge.prompt\_path** `string | null` (default: null)

Optional replacement for the bridge instructions.

**user\_agent.bridge.kwargs** `object` (default: \{})

Bridge-specific keyword arguments.

See [Simulate a user](https://docs.harborframework.com/jobs/simulate-a-user.md).

## Environment

**environment.type** `EnvironmentType | null` (default: "docker")

Pre-integrated environment provider: `docker`, `podman`, `daytona`, `e2b`, `modal`, `runloop`, `langsmith`, `ec2`, `gke`, `ack`, `openshift`, `novita`, `apple-container`, `singularity`, `islo`, `tensorlake`, `cwsandbox`, `use-computer`, `cua-cloud`, `blaxel`, `opensandbox`, `beam`, `skypilot`, `hf-sandbox`, `hyperbrowser`, `vercel`, `sail`, `superserve`, or `sbx`. See [Pre-integrated sandboxes](https://docs.harborframework.com/sandboxes/pre-integrated-sandboxes.md).

**environment.import\_path** `string | null` (default: null)

Custom environment import path in `module.path:ClassName` format.

**environment.force\_build** `boolean` (default: false)

Rebuild the environment even when a cached build exists.

**environment.delete** `boolean` (default: true)

Delete the environment after the trial finishes.

**environment.cpu\_enforcement\_policy** `"auto" | "limit" | "request" | "guarantee" | "ignore"` (default: "auto")

How the provider enforces the task's CPU value.

**environment.memory\_enforcement\_policy** `"auto" | "limit" | "request" | "guarantee" | "ignore"` (default: "auto")

How the provider enforces the task's memory value.

**environment.override\_cpus** `number | null` (default: null)

Replace the task's CPU value at runtime. Fractional values require [provider support](https://docs.harborframework.com/tasks/resources.md#provider-support).

**environment.override\_memory\_mb** `integer | null` (default: null)

Replace the task's memory value in MB.

**environment.override\_storage\_mb** `integer | null` (default: null)

Replace the task's storage value in MB.

**environment.override\_gpus** `integer | null` (default: null)

Replace the task's GPU count.

**environment.override\_tpu** `TpuSpec | null` (default: null)

Replace the task's TPU specification.

**environment.suppress\_override\_warnings** `boolean` (default: false)

Deprecated. This field is accepted but has no effect and is excluded when serializing configs.

**environment.mounts** `list[ServiceVolumeConfig] | null` (default: null)

Docker Compose long-syntax volume mounts for the agent environment only.

**environment.extra\_docker\_compose** `list[string]` (default: \[])

Additional Docker Compose overlay files for the agent environment only.

**environment.env** `object[string, string]` (default: \{})

Baseline environment variables exposed inside the sandbox.

**environment.kwargs** `object` (default: \{})

Provider-specific keyword arguments passed to the environment constructor.

**environment.extra\_allowed\_hosts** `list[string]` (default: \[])

Hostnames, IP addresses, or CIDR ranges added to the environment network baseline.

See [Resources](https://docs.harborframework.com/tasks/resources.md), [Network policies](https://docs.harborframework.com/tasks/network-policies.md), and [Custom sandboxes](https://docs.harborframework.com/sandboxes/custom-sandboxes.md).

### TPU override

**environment.override\_tpu.type** `string`

Required TPU alias or canonical GKE accelerator label, such as `v6e` or `tpu-v6e-slice`.

**environment.override\_tpu.topology** `string`

Required topology in `NxM` or `NxMxK` format, such as `2x4`.

### Mounts

**environment.mounts\[].type** `"bind" | "volume" | "image"`

Required mount type.

**environment.mounts\[].source** `string`

Required host path, named volume, or image source.

**environment.mounts\[].target** `string`

Required target path inside the container.

**environment.mounts\[].read\_only** `true` (default: omitted)

Set to `true` for a read-only mount.

**environment.mounts\[].bind.create\_host\_path** `false` (default: omitted)

Set to `false` to prevent Docker Compose from creating a missing bind source path.

**environment.mounts\[].bind.selinux** `"z" | "Z"` (default: omitted)

Optional SELinux relabeling mode for a bind mount.

**environment.mounts\[].volume.subpath** `string` (default: omitted)

Optional subpath within a named volume.

**environment.mounts\[].image.subpath** `string` (default: omitted)

Optional subpath within an image mount.

## Verifier

**verifier.override\_timeout\_sec** `number | null` (default: null)

Replace the task's verifier timeout with this value in seconds.

**verifier.max\_timeout\_sec** `number | null` (default: null)

Upper bound for the effective verifier timeout in seconds.

**verifier.include\_logs** `list[string]` (default: \[])

Glob patterns selecting verifier log files to download. The reward file is always downloaded.

**verifier.exclude\_logs** `list[string]` (default: \[])

Glob patterns excluded from verifier logs after applying `include_logs`.

**verifier.env** `object[string, string]` (default: \{})

Environment variables exposed only during the verifier phase.

**verifier.import\_path** `string | null` (default: null)

Custom verifier import path in `module.path:ClassName` format.

**verifier.kwargs** `object` (default: \{})

Keyword arguments passed to the custom verifier.

**verifier.disable** `boolean` (default: false)

Skip verification. Automatically set to `true` when `install_only` is enabled.

**verifier.extra\_allowed\_hosts** `list[string]` (default: \[])

Hostnames, IP addresses, or CIDR ranges added to the allowlist during `verify()`, and from startup in a separate verifier environment.

See [Custom verifiers](https://docs.harborframework.com/jobs/custom-verifiers.md) and [Environment variables](https://docs.harborframework.com/jobs/environment-variables.md).

## Datasets

**datasets\[].path** `string | null` (default: null)

Local dataset directory. With `repo`, this selects a repo-relative implicit dataset directory.

**datasets\[].name** `string | null` (default: null)

Harbor Hub dataset in `org/name` format, or a bare dataset name for a custom or Git-repository registry.

**datasets\[].version** `string | null` (default: null)

Version selected from a JSON registry or a named Git-repository registry.

**datasets\[].ref** `string | null` (default: null)

Tag, revision, or digest selected for a Harbor Hub dataset.

**datasets\[].registry\_url** `string | null` (default: null)

URL of a custom `registry.json` file.

**datasets\[].registry\_path** `string | null` (default: null)

Path to a custom `registry.json`. With `repo`, this path is relative to the repository.

**datasets\[].repo** `string | null` (default: null)

Git repository shorthand or URL, optionally pinned with `@ref`.

**datasets\[].overwrite** `boolean` (default: false)

Overwrite cached remote tasks.

**datasets\[].download\_dir** `string | null` (default: null)

Directory used to cache downloaded tasks.

**datasets\[].task\_names** `list[string] | null` (default: null)

Glob patterns selecting tasks by name.

**datasets\[].exclude\_task\_names** `list[string] | null` (default: null)

Glob patterns excluding tasks after applying `task_names`.

**datasets\[].n\_tasks** `integer | null` (default: null)

Maximum number of tasks after inclusion and exclusion filters are applied.

Each dataset must select exactly one source shape. Without `repo`, set either `path` or `name`, but not both. With `repo`, `path` selects an implicit dataset while `name` selects a named registry dataset. `version` and `ref` cannot both be set.

See [Datasets](https://docs.harborframework.com/datasets/datasets.md), [Registries](https://docs.harborframework.com/datasets/registries.md), and [Git repos](https://docs.harborframework.com/datasets/git-repos.md).

## Tasks

**tasks\[].path** `string | null` (default: null)

Local task directory, or the path within a Git repository when `git_url` is set.

**tasks\[].git\_url** `string | null` (default: null)

Git repository containing the task.

**tasks\[].git\_commit\_id** `string | null` (default: null)

Git commit containing the task. Requires `git_url`.

**tasks\[].name** `string | null` (default: null)

Harbor Hub task name in `org/name` format.

**tasks\[].ref** `string | null` (default: null)

Harbor Hub task tag, revision, or digest. Requires `name`.

**tasks\[].overwrite** `boolean` (default: false)

Overwrite the cached remote task.

**tasks\[].download\_dir** `string | null` (default: null)

Directory used to cache the downloaded task.

**tasks\[].source** `string | null` (default: null)

Optional source label used when grouping tasks and metrics.

Every task must set either `path` or `name`, but not both. A Git task uses `path` with `git_url`; a Harbor Hub task uses `name` with an optional `ref`.

See [Tasks → Overview](https://docs.harborframework.com/tasks/overview.md).

## Metrics

**metrics\[].type** `"sum" | "min" | "max" | "mean" | "uv-script"` (default: "mean")

Metric implementation used to aggregate task rewards.

**metrics\[].kwargs** `object` (default: \{})

Keyword arguments passed to the metric implementation. `uv-script` requires `script_path`.

See [Metrics](https://docs.harborframework.com/datasets/metrics.md).

## Artifacts

**artifacts\[].source** `string`

Required environment path to collect. It cannot contain `..` path components.

**artifacts\[].destination** `string | null` (default: null)

Optional path under the trial's artifact directory. Harbor derives it from `source` when omitted.

**artifacts\[].exclude** `list[string]` (default: \[])

Patterns excluded when downloading a directory artifact.

**artifacts\[].service** `string | null` (default: null)

Docker Compose service to collect from. `null` and `main` target the agent container.

See [Artifact collection](https://docs.harborframework.com/jobs/artifact-collection.md).

## Regrade sources

**source\_jobs\[].action** `"regrade"`

Required derivation action. Currently only `regrade` is supported.

**source\_jobs\[].type** `"local" | "hub"`

Required source location.

**source\_jobs\[].job\_id** `string | null` (default: null)

Source job UUID. Required for a Hub source and optional for a local source.

**source\_jobs\[].path** `string | null` (default: null)

Source job directory. Required for a local source and invalid for a Hub source.

Regrading cannot be combined with `install_only`. See [Regrade](https://docs.harborframework.com/jobs/regrade.md).


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.