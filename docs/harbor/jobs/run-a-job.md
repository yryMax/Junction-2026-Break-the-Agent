> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Run a job

> Run a job in Harbor.

A job is a collection of trials. A trial is an agent's attempt at completing a task. To run a job in Harbor, use the `harbor run` command.

> [tab] Local dataset
>
> ```bash theme={"system"}
> harbor run -p "<path/to/dataset>" -a "<agent>" -m "<model>"
> ```

> [tab] Local task
>
> ```bash theme={"system"}
> harbor run -p "<path/to/task>" -a "<agent>" -m "<model>"
> ```

> [tab] Hub dataset
>
> ```bash theme={"system"}
> harbor run -d "<org/dataset>@<ref>" -a "<agent>" -m "<model>"
> ```

> [tab] Hub task
>
> ```bash theme={"system"}
> harbor run -t "<org/task>@<ref>" -a "<agent>" -m "<model>"
> ```

> [tab] Custom registry
>
> ```bash theme={"system"}
> harbor run -d "<dataset>@<version>" \
>   --registry-path "<path/to/registry.json>" \
>   -a "<agent>" -m "<model>"
> ```
>
> ```bash theme={"system"}
> harbor run -d "<dataset>@<version>" \
>   --registry-url "<url/to/registry.json>" \
>   -a "<agent>" -m "<model>"
> ```
>
> See [Registries](https://docs.harborframework.com/datasets/registries.md).

> [tab] Git repository
>
> ```bash theme={"system"}
> harbor run --repo "<org/repo-name>" -p "<path/to/tasks>" \
>   -a "<agent>" -m "<model>"
> ```
>
> See [Git repos](https://docs.harborframework.com/datasets/git-repos.md).

## Configuring a job

Harbor exposes CLI flags for configuring jobs.

> [tab] Output
>
> ```bash theme={"system"}
> harbor run -p "<path>" -a "<agent>" -m "<model>" \
>   -o "<jobs-dir>" --job-name "<job-name>"
> ```

> [tab] Concurrency and retries
>
> ```bash theme={"system"}
> harbor run -p "<path>" -a "<agent>" -m "<model>" \
>   -n "<concurrent>" -k "<attempts>" -r "<retries>"
> ```

> [tab] Task filters
>
> ```bash theme={"system"}
> harbor run -p "<path>" -a "<agent>" -m "<model>" \
>   -l "<max-tasks>" -i "<some-*-glob-pattern>" -x "<some-name>"
> ```

> [tab] Sandbox
>
> ```bash theme={"system"}
> harbor run -p "<path>" -a "<agent>" -m "<model>" \
>   -e "<sandbox>" --allow-agent-host "<host>" --override-memory-mb "<memory-mb>"
> ```
>
> See [Pre-integrated sandboxes](https://docs.harborframework.com/sandboxes/pre-integrated-sandboxes.md), [Resources](https://docs.harborframework.com/tasks/resources.md), and [Network policies](https://docs.harborframework.com/tasks/network-policies.md).

> [tab] Simulated user
>
> ```bash theme={"system"}
> harbor run -p "<path>" -a "<agent>" -m "<model>" \
>   --user-agent "<user-agent>" --user-model "<user-model>"
> ```
>
> See [Simulate a user](https://docs.harborframework.com/jobs/simulate-a-user.md).

Run `harbor run --help` to see all available flags.

## Agents

Harbor has pre-integrated agents and support for custom agents.

Run `harbor run --help` to see the available agents.

Set reasoning effort with `--effort`, other agent options with `--ak`, and the model with `--model` (`-m`).

See [Model name parsing](https://docs.harborframework.com/agents/agent-auth-resolution.md#model-name-parsing) to look up how LiteLLM parses a model name.

```bash theme={"system"}
harbor run -a "<agent>" -m "<model>" --effort high
```

See [Pre-integrated agents](https://docs.harborframework.com/agents/pre-integrated-agents.md) or [Custom agents](https://docs.harborframework.com/agents/custom-agents.md) to learn more.

## Config files

Under the hood, configuring a job with CLI flags builds a `JobConfig` object and passes it to the runner.

You can see the config when you run `harbor run` with the `--print-config` flag, or by inspecting the `<job/path>/config.json` file after you run `harbor run`. You can also use `harbor job init` to create and save a config instead of running the job.

For frequent setups, we recommend creating and version controlling your configs.

> [!NOTE]
>
> You can pair a config with CLI flags to override values in the original config.

An example config is shown below.

```json theme={"system"}
{
  "n_attempts": 5,
  "n_concurrent_trials": 100,
  "retry": {
    "max_retries": 3
  },
  "agents": [
    {
      "name": "<agent>",
      "model_name": "<model>"
    }
  ],
  "datasets": [
    {
      "path": "<path>"
    }
  ]
}
```

See [Configs](https://docs.harborframework.com/jobs/configs.md) for the complete job config schema.

## Run on Harbor Hub

You can run your jobs on Harbor Hub by including the `--launch` flag.

Harbor Hub

* Orchestrates your trials in the cloud
* Enforces per-provider concurrency limits
* Pauses on cascading failures (e.g. usage limits)
* Backs off on rate limits
* Can be accessed using the UI or CLI to debug, trigger retries, share results, or monitor progress

First, sign in.

```bash theme={"system"}
harbor auth login
```

Then, run your job with the `--launch` flag.

```bash theme={"system"}
harbor run -d "<org/name>" -a "<agent>" -m "<model>" \
  -n "<concurrent>" --one-off-secret "<name>=<value>" --launch
```

See [Hosted jobs](https://docs.harborframework.com/harbor-hub/hosted-jobs.md) for more information about evaluating custom agents and Git-based tasks and datasets.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.