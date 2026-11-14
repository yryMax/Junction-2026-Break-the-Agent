> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Configuration

> Task configuration, metadata, and schema reference.

The **`task.toml`** file specifies the task configuration and metadata.

```toml theme={"system"}
schema_version = "1.4"

[task]
name = "apple/create-unix-os"
authors = [{ name = "Steve Jobs", email = "steve@apple.com" }]

[metadata]
difficulty_explanation = "Trivial task for demonstration"
category = "programming"

[verifier]
timeout_sec = 120.0
env = { API_KEY = "sk-test-123" }
user = "root"  # optional: run the verifier as this OS user

[agent]
timeout_sec = 120.0
user = "agent"  # optional: run the agent as this OS user

[solution]
env = { API_KEY = "sk-test-123" }

[environment]
network_mode = "allowlist"  # baseline; defaults to "public" when omitted
allowed_hosts = ["pypi.org"]
docker_image = "apple/unix-os:latest"
cpus = 1
memory_mb = 2048
storage_mb = 10240
```

## `[environment]` vs. `environment/`

By default, Harbor defers as much configuration as possible into the `environment/` spec rather than the `task.toml`. A lot of engineering effort has gone into designing `environment/` specs (like `Dockerfile`) and they provide conventions for common use cases.

Only when absolutely necessary does Harbor require configuration in the `task.toml`.

A few fields in `task.toml` are due to lack of convention or common pitfalls:

| Field | Explanation |
| - | - |
| `environment.docker_image` | Specifies the Docker image to use for the environment. (If this is set, `environment/` can be omitted entirely) |
| `environment.network_mode` | Sets the network isolation mode (e.g., `public`, `allowlist`). |
| `environment.allowed_hosts` | Lists domains/IPs allowed for outbound network access when `network_mode` is `allowlist`. |
| `environment.env` | Defines environment variables to inject into the environment. |
| `environment.healthcheck` | Configuration for checking if the environment is healthy or ready. |
| `environment.workdir` | Sets the working directory for commands run in the environment. This was added by popular demand to retroactively set a `workdir` without having to rebuild the image. |

## Field reference

### General

**schema\_version** `string` (default: "1.4")

Version of the task configuration format.

**multi\_step\_reward\_strategy** `"mean" | "final" | null` (default: null)

How to derive the trial-level reward from per-step verifier results. Only applies when `[[steps]]` is set; leave unset for single-step tasks (defaults to "mean" on multi-step). See [Multi-step](https://docs.harborframework.com/tasks/multi-step.md).

### `[task]`

**task** `PackageInfo | null` (default: null)

Optional \[task] section for registry package metadata. When present, task.name is required.

**task.name** `string`

Package name in org/name format (e.g., "harbor/hello-world"). Required when \[task] is declared.

**task.authors** `list[Author]` (default: \[])

List of task authors. Each entry has a required name and optional email (for example, `[{ name = "Jane", email = "jane@example.com" }]`).

**task.description** `string` (default: "")

Human-readable description of the task.

**task.keywords** `list[string]` (default: \[])

Keywords for search and categorization.

### `[metadata]`

**metadata** `object`

Arbitrary metadata provided by the task author.

### `[verifier]`

**verifier.timeout\_sec** `number` (default: 600)

Timeout in seconds for the verifier.

**verifier.network\_mode** `"no-network" | "public" | "allowlist" | null` (default: null (no override))

Optional override during verify(). Applied only when set and different from the verifier baseline. See [Network policies](https://docs.harborframework.com/tasks/network-policies.md).

**verifier.allowed\_hosts** `list[string] | null` (default: null)

Allowlist hostnames when verifier.network\_mode is allowlist.

**verifier.env** `object` (default: \{})

Environment variables to set when running the verifier.

**verifier.user** `string | int | null` (default: null)

Username or UID to run the verifier as. When set, the environment's default user is configured accordingly before verification. If not set, the environment's container default (typically root) is used.

**verifier.environment\_mode** `"shared" | "separate" | null` (default: null)

Where the verifier runs. "shared" (default when both this field and \[verifier.environment] are omitted) reuses the agent's container; "separate" launches a dedicated verifier container with its own image. Omitting this field while declaring \[verifier.environment] implies "separate". Declaring "shared" alongside \[verifier.environment] is a validation error.

**verifier.environment** `EnvironmentConfig | null` (default: null)

Optional verifier configuration, using the same schema as `[environment]`. Verifier images and `tests/` build definitions take precedence over the agent environment. Dedicated verifier images must bundle tests; falling back to the agent environment uploads them to `/tests/`. See [Image selection](https://docs.harborframework.com/tasks/separate-verifier.md#image-selection).

**verifier.environment.network\_mode** `"no-network" | "public" | "allowlist"` (default: "public")

Separate verifier env baseline when \[verifier.environment] is set. Same modes as environment.network\_mode.

**verifier.environment.allowed\_hosts** `list[string] | null` (default: null)

Allowlist hostnames when verifier.environment.network\_mode is allowlist.

### `[agent]`

**agent.timeout\_sec** `number | null` (default: null)

Timeout in seconds for the agent. If not set, no timeout is enforced.

**agent.network\_mode** `"no-network" | "public" | "allowlist" | null` (default: null (no override))

Optional override during agent.run(). Applied only when set and different from \[environment]. See [Network policies](https://docs.harborframework.com/tasks/network-policies.md).

**agent.allowed\_hosts** `list[string] | null` (default: null)

Allowlist hostnames when agent.network\_mode is allowlist.

**agent.user** `string | int | null` (default: null)

Username or UID to run the agent as. When set, the environment's default user is configured accordingly before agent setup and execution. If not set, the environment's container default (typically root) is used.

### `[solution]`

**solution.env** `object` (default: \{})

Environment variables to set when running the solution.

### `[environment]`

**environment.build\_timeout\_sec** `number` (default: 600)

Timeout in seconds for environment build.

**environment.network\_mode** `"no-network" | "public" | "allowlist"` (default: "public")

Agent env baseline. Also the shared verifier baseline when no separate verifier env overrides it. See [Network policies](https://docs.harborframework.com/tasks/network-policies.md).

**environment.allowed\_hosts** `list[string] | null` (default: null)

Allowlist hostnames when environment.network\_mode is allowlist.

**environment.docker\_image** `string | null` (default: null)

A pre-built Docker image to use for the environment. When set, environment/Dockerfile is optional for supported environment types.

**environment.os** `"linux" | "windows"` (default: "linux")

Target operating system for the task container. "linux" (default) or "windows". When "windows", Harbor uses Windows-style paths, cmd.exe execution, tar-over-exec file transfer, and filters script discovery to .bat only. The Docker daemon mode and image OS are validated against this value at start; mismatches fail fast.

**environment.cpus** `number | null` (default: null)

Number of CPUs requested by the task. When omitted, Harbor leaves CPU sizing to the selected provider. Fractional values require [provider support](https://docs.harborframework.com/tasks/resources.md#provider-support).

**environment.memory\_mb** `integer | null` (default: null)

Amount of RAM requested by the task in megabytes. When omitted, Harbor leaves memory sizing to the selected provider.

**environment.storage\_mb** `integer | null` (default: null)

Amount of storage requested by the task in megabytes. When omitted, Harbor leaves storage sizing to the selected provider.

**environment.gpus** `integer | null` (default: null)

Number of GPUs requested by the task. When omitted, Harbor does not request GPUs.

**environment.gpu\_types** `list[string] | null` (default: null)

List of acceptable GPU types (e.g., \['H100', 'A100', 'T4']). None means any GPU type is acceptable.

**environment.tpu** `TpuSpec | null` (default: null)

TPU slice specification (type + topology). When set, the environment requests a TPU node matching this spec; per-pod chip count is derived from the topology. Singular because a task allocates exactly one TPU slice per pod. Only supported on TPU-capable environments (currently GKE).

**environment.tpu.type** `string`

TPU accelerator type. Accepts either a user-friendly alias (e.g., 'v6e', 'trillium', 'v4') or a canonical GKE label (e.g., 'tpu-v6e-slice', 'tpu7x').

**environment.tpu.topology** `string`

TPU topology as 'NxM' or 'NxMxK' (e.g., '2x4', '2x2x1'). Required — GKE's implicit default topology is not part of a stable contract, so omitting it would make Harbor runs non-reproducible across GKE versions. Per-pod TPU chip count is computed as the product of dimensions (e.g. '2x2x1' → 4 chips, '2x4' → 8 chips). Each dimension must be a positive integer (no leading zeros).

**environment.allow\_internet** `boolean | null` (default: null)

Deprecated compatibility field. Prefer \[environment].network\_mode. When set and \[environment].network\_mode is omitted, false maps to no-network and true maps to public.

**environment.env** `object` (default: \{})

Environment variables required for the task, resolved from the host at runtime. Supports `${VAR}` and `${VAR:-default}` template syntax.

**environment.mcp\_servers.name** `string`

Unique name for each MCP server entry (declare servers as `[[environment.mcp_servers]]` in TOML). Compatible agents register them automatically. See [MCPs](https://docs.harborframework.com/tasks/environment.md#mcps).

**environment.mcp\_servers.transport** `"stdio" | "sse" | "streamable-http"` (default: "sse")

How the agent connects to the server. "sse" and "streamable-http" require url; "stdio" requires command. Legacy value "http" is normalized to "streamable-http".

**environment.mcp\_servers.url** `string | null` (default: null)

Endpoint URL when transport is "sse" or "streamable-http" (e.g. [http://mcp-server:8000/mcp](http://mcp-server:8000/mcp) for a Compose sidecar). Required for those transports.

**environment.mcp\_servers.headers** `object[string, string]` (default: \{})

HTTP request headers for "sse" and "streamable-http" servers. Values support embedded `${VAR}` and `${VAR:-default}` references resolved when the agent starts. Credential headers must use `${VAR}` references. Headers are invalid for "stdio" servers. See [MCPs](https://docs.harborframework.com/tasks/environment.md#mcps).

**environment.mcp\_servers.command** `string | null` (default: null)

Executable to spawn when transport is "stdio". Required for stdio.

**environment.mcp\_servers.args** `list[string]` (default: \[])

Arguments passed to command when transport is "stdio".

**environment.skills\_dir** `string | null` (default: null)

Path to a [skills directory](https://docs.harborframework.com/tasks/skills.md) in the environment. Contents are registered with compatible agents.

**environment.healthcheck** `HealthcheckConfig | null` (default: null)

Optional healthcheck block run after environment start (omit the whole \[environment.healthcheck] section to disable). When set, command is required; other fields use the defaults below.

**environment.healthcheck.command** `string`

Shell command to run as a healthcheck after environment start. Exit 0 means healthy. Required when \[environment.healthcheck] is present.

**environment.healthcheck.interval\_sec** `number` (default: 5)

Time in seconds between healthcheck attempts.

**environment.healthcheck.timeout\_sec** `number` (default: 30)

Maximum time in seconds for a single healthcheck command.

**environment.healthcheck.start\_period\_sec** `number` (default: 0)

Grace period in seconds after environment start during which failures don't count.

**environment.healthcheck.start\_interval\_sec** `number` (default: 5)

Interval in seconds between checks during the start period.

**environment.healthcheck.retries** `integer` (default: 3)

Number of consecutive failures before the healthcheck is considered failed.

**environment.workdir** `string | null` (default: null)

Default working directory for command execution in the environment. Overrides the container WORKDIR when set.

### Artifacts

**artifacts** `list[string | ArtifactConfig]` (default: \[])

Root-level paths to snapshot from the environment into the trial artifacts directory after the agent runs, before verification. For multi-step tasks, these paths are included in every step's collection pass, alongside `[[steps]].artifacts` — see [Multi-step](https://docs.harborframework.com/tasks/multi-step.md). Each entry is a container path string or a table with source / destination / exclude. See [Artifacts](https://docs.harborframework.com/tasks/artifacts.md) for examples.

**artifacts.source** `string`

Container path to download (file or directory). Required for table entries; a bare string in the artifacts list is shorthand for source only.

**artifacts.destination** `string | null` (default: null)

Relative path under the trial artifacts directory. When omitted, Harbor derives the host path from source.

**artifacts.exclude** `list[string]` (default: \[])

Glob patterns to exclude when source is a directory (passed as tar --exclude flags).

### Provenance

**source** `string | null` (default: null)

Optional source string for task provenance.

## Multi-step configuration

The field reference above covers single-step and shared task-root settings. Multi-step tasks add `[[steps]]` entries with per-step `agent`, `verifier`, `healthcheck`, `min_reward`, and `artifacts` overrides. Step fields, `workdir/setup.sh`, and trial-level reward rollup are documented on [multi-step tasks](https://docs.harborframework.com/tasks/multi-step.md).

## TOML templates

During task creation, you can pass a `--metadata-template` flag with a path to a TOML file to pre-populate `task.toml` with metadata fields and config defaults:

```bash theme={"system"}
harbor task init [org]/[name] --metadata-template task-template.toml
```

Sections in the template override Harbor's built-in defaults. Newly scaffolded tasks set `verifier.environment_mode = "separate"` unless the template overrides it; other omitted settings use the defaults listed above.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.