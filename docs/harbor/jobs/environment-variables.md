> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Environment variables

> Control which environment variables reach Harbor, sandboxes, agents, and verifiers.

Harbor separates variables by where they are needed: the Harbor process, the
sandbox, the agent phase, or the verifier phase.

> [!WARNING]
>
> Keep sandbox-provider credentials such as `DAYTONA_API_KEY` in the Harbor
> process. Passing them through `environment.env`, `--agent-env`, or
> `--verifier-env` unnecessarily exposes them inside the sandbox and is **not
> recommended**.

> [!NOTE]
>
> `export` and `--env-file` do not copy every variable into the sandbox. To pass
> a host variable such as `DATABASE_URL`, set `environment.env.DATABASE_URL` to
> `${DATABASE_URL}` explicitly.

> [tab] Shared verifier
>
> The agent and verifier run in the same sandbox.
>
> ![Matrix showing which variables reach the Harbor process, agent phase, verifier phase, and lifecycle commands](https://mintcdn.com/harborframework/XHi-ZTogIM2NSVlh/images/environment-variable-reach.png?fit=max\&auto=format\&n=XHi-ZTogIM2NSVlh\&q=85\&s=d6366aabc26d2bf45404a82980ebd484)
>
> Environment variable scope for shared verifier
>
> Lifecycle commands include health checks, step setup, and collection hooks.
> They run around the agent and verifier phases and receive sandbox variables only.
>
> | Scope and availability | Shell or CLI | `config.json` | `task.toml` | Example |
> | - | - | - | - | - |
> | **Harbor process** and environment provider | `export`, `--env-file` | — | — | `DAYTONA_API_KEY` |
> | **Shared sandbox:** agent, verifier, and lifecycle commands | — | `environment.env` | `[environment.env]` | `DATABASE_URL` |
> | **Agent phase only** | `--agent-env` (`--ae`) | `agents[].env` | — | `OPENAI_API_KEY` |
> | **Verifier phase only** | `--verifier-env` (`--ve`) | `verifier.env` | `[verifier.env]` | `REWARDKIT_MODEL` |
>
> ![Harbor process managing a shared sandbox with agent, verifier, and lifecycle command scopes](https://mintcdn.com/harborframework/XHi-ZTogIM2NSVlh/images/shared-sandbox-variables.png?fit=max\&auto=format\&n=XHi-ZTogIM2NSVlh\&q=85\&s=803471b2ac60d1db928eb879452e0c65)
>
> Environment variable architecture for shared verifier
>
> The agent, verifier, and lifecycle commands share one sandbox. Phase
> variables are added only while their phase runs and override sandbox
> variables with the same name.

> [tab] Separate verifier (default)
>
> A [separate verifier](https://docs.harborframework.com/tasks/separate-verifier.md) runs in a
> second sandbox. Agent-phase variables do not reach it; only
> [collected artifacts](https://docs.harborframework.com/jobs/artifact-collection.md) cross from
> the agent sandbox.
>
> ![Matrix showing which variables reach the Harbor process, agent sandbox, verifier sandbox, phases, and lifecycle commands](https://mintcdn.com/harborframework/PxOk-gLJgMnxNtMy/images/separate-verifier-environment-variable-reach.png?fit=max\&auto=format\&n=PxOk-gLJgMnxNtMy\&q=85\&s=da3f9bc79c00d4d974fe1b00da04d5b8)
>
> Environment variable scope for separate verifier
>
> `verifier.environment.env` sets baseline variables for the verifier sandbox,
> including its lifecycle commands. `verifier.env` adds variables only during
> the verifier phase.
>
> | Scope and availability | Shell or CLI | `config.json` | `task.toml` | Example |
> | - | - | - | - | - |
> | **Harbor process** and environment provider | `export`, `--env-file` | — | — | `DAYTONA_API_KEY` |
> | **Sandbox baseline** | — | `environment.env` (both sandboxes) | `[environment.env]` (agent sandbox) | `DATABASE_URL` |
> | **Agent phase only** | `--agent-env` (`--ae`) | `agents[].env` | — | `OPENAI_API_KEY` |
> | **Verifier phase only** | `--verifier-env` (`--ve`) | `verifier.env` | `[verifier.env]` | `REWARDKIT_MODEL` |
>
> ![Harbor process managing separate agent and verifier sandboxes with their phase and lifecycle command scopes](https://mintcdn.com/harborframework/PxOk-gLJgMnxNtMy/images/separate-verifier-sandbox-variables.png?fit=max\&auto=format\&n=PxOk-gLJgMnxNtMy\&q=85\&s=534a6821e7445bb80f092871ecd8f392)
>
> Environment variable architecture for separate verifier
>
> > [!NOTE]
> >
> > If separate mode is enabled without `[verifier.environment]`, Harbor
> > copies the top-level `[environment]`. Its `[environment.env]` variables
> > therefore become the verifier sandbox baseline as well.
>
> Verifier-phase variables override verifier-sandbox variables with the same
> name. Agent-phase variables follow the same rule in the agent sandbox.

## Automatic agent credential forwarding

Some [built-in agents](https://docs.harborframework.com/agents/pre-integrated-agents.md) recognize
selected host variables automatically. See [agent auth resolution](https://docs.harborframework.com/agents/agent-auth-resolution.md#automatic-secret-collection) for more details


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.