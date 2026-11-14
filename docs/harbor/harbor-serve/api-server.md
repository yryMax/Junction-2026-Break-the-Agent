> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# API server

> Start a Harbor server and connect through HTTP, the CLI, the Python SDK, or MCP.

`harbor serve` runs jobs and manages sandboxes. Clients connect to the same server and see the same jobs; execution continues when a client disconnects.

## Start the server

With Docker running, start the server:

```bash theme={"system"}
harbor serve
```

Leave `HARBOR_API_KEY` unset for this local example. The examples below use the public [hello-world task](https://hub.harborframework.com/tasks/hello-world/hello-world) and the **oracle agent**, which runs its reference solution. No Hub or model API key is needed.

> [accordion] Use Daytona instead of Docker
>
> Configure the sandbox provider on the **server**, then use `"environment": {"type": "daytona"}` in your job config, or `--env daytona` in the CLI.
>
> ```bash theme={"system"}
> uv pip install "harbor[daytona]"
> export DAYTONA_API_KEY="your-daytona-key"
> harbor serve
> ```

## Choose a client

Once the server is running, there are **four ways to interact with it**:

1. **HTTP / cURL**
2. **Harbor CLI** with `--server`
3. **Python SDK** (`Harbor` / `AsyncHarbor`)
4. **MCP**

Choose one of the examples below to submit a job:

> [tab] HTTP / cURL
>
> Open [Submit job](https://docs.harborframework.com/api-sdk/jobs/submit-job.md), select **Local server**, leave the Bearer field empty, and click **Send** on the prefilled example. Or run:
>
> ```bash theme={"system"}
> curl http://127.0.0.1:8390/jobs/submit \
>   -H 'Content-Type: application/json' \
>   -d '{
>     "config": {
>       "tasks": [{"name": "hello-world/hello-world"}],
>       "agents": [{"name": "oracle"}],
>       "environment": {"type": "docker"}
>     }
>   }'
> ```
>
> > [!NOTE]
> >
> > The playground sends real requests from your browser. Allow local-network access if prompted. If blocked, use cURL or `http://127.0.0.1:8390/docs`.

> [tab] Harbor CLI
>
> The CLI submits a job config to `POST /jobs/submit`. `--watch` follows progress:
>
> ```bash theme={"system"}
> harbor run --server http://127.0.0.1:8390 \
>   --task hello-world/hello-world \
>   --agent oracle --env docker --watch
> ```
>
> To submit an existing config, use `harbor run --server http://127.0.0.1:8390 --config job.json --watch`.

> [tab] Python SDK
>
> `Harbor()` defaults to `http://127.0.0.1:8390` and reads `HARBOR_API_KEY` if set. Run this with `python submit_job.py`:
>
> ```python submit_job.py theme={"system"}
> from harbor import Harbor
>
> client = Harbor()
> job = client.jobs.submit(config={
>     "tasks": [{"name": "hello-world/hello-world"}],
>     "agents": [{"name": "oracle"}],
>     "environment": {"type": "docker"},
> })
> job_id = job.job_id
> print(job_id)
> print(client.jobs.get(job_id))
> print(client.rollouts.list(job_id=job_id))
> client.close()
> ```
>
> `config` accepts a dictionary or `JobConfig`; responses are typed models. Set `base_url` to connect remotely. `AsyncHarbor` provides the same methods with `await`.
>
> Select **Python** on any [endpoint page](https://docs.harborframework.com/api-sdk/jobs/submit-job.md) for its SDK example.

> [tab] MCP
>
> Harbor exposes MCP at `http://127.0.0.1:8390/mcp`. No authentication is needed for this local example.
>
> > [accordion] Configure MCP permanently
> >
> > ```bash Claude Code theme={"system"}
> > claude mcp add --transport http --scope user harbor http://127.0.0.1:8390/mcp
> > ```
> >
> > ```bash Codex theme={"system"}
> > codex mcp add harbor --url http://127.0.0.1:8390/mcp
> > ```
> >
> > ```json Cursor theme={"system"}
> > {
> >   "mcpServers": {
> >     "harbor": {
> >       "url": "http://127.0.0.1:8390/mcp"
> >     }
> >   }
> > }
> > ```
> >
> > For Cursor, add the entry to `~/.cursor/mcp.json`. Other clients can use the same URL with Streamable HTTP.
>
> **Per conversation:** an agent with HTTP tools can communicate with MCP directly using the prompt below. Otherwise, configure MCP above first.
>
> > Use the Harbor MCP server at [http://127.0.0.1:8390/mcp](http://127.0.0.1:8390/mcp) to run hello-world/hello-world with oracle on Docker. Show the job ID, job, and rollouts.

The server downloads the task from Hub and returns `job_id`. For local tasks, use `path` instead of `name`; paths refer to the **server's filesystem**.

> [accordion] Run Codex with an OpenAI API key
>
> Keep the server running. Supply your **OpenAI key per job**:
>
> > [tab] HTTP / cURL
> >
> > ```bash theme={"system"}
> > curl http://127.0.0.1:8390/jobs/submit \
> >   -H 'Content-Type: application/json' \
> >   -d '{
> >     "config": {
> >       "tasks": [{"name": "hello-world/hello-world"}],
> >       "agents": [{
> >         "name": "codex",
> >         "model_name": "openai/gpt-6.1-sol",
> >         "env": {"OPENAI_API_KEY": "your-openai-key"}
> >       }],
> >       "environment": {"type": "docker"}
> >     }
> >   }'
> > ```
>
> > [tab] Harbor CLI
> >
> > ```bash theme={"system"}
> > harbor run --server http://127.0.0.1:8390 \
> >   --task hello-world/hello-world \
> >   --agent codex --model openai/gpt-6.1-sol \
> >   --agent-env OPENAI_API_KEY=your-openai-key \
> >   --env docker --watch
> > ```
>
> > [tab] Python SDK
> >
> > ```python submit_job.py theme={"system"}
> > from harbor import Harbor
> >
> > client = Harbor()
> > job = client.jobs.submit(config={
> >     "tasks": [{"name": "hello-world/hello-world"}],
> >     "agents": [{
> >         "name": "codex",
> >         "model_name": "openai/gpt-6.1-sol",
> >         "env": {"OPENAI_API_KEY": "your-openai-key"},
> >     }],
> >     "environment": {"type": "docker"},
> > })
> > job_id = job.job_id
> > print(job_id)
> > print(client.jobs.get(job_id))
> > print(client.rollouts.list(job_id=job_id))
> > client.close()
> > ```
>
> > [tab] MCP
> >
> > > Use the Harbor MCP server at [http://127.0.0.1:8390/mcp](http://127.0.0.1:8390/mcp) to run hello-world/hello-world with Codex (openai/gpt-6.1-sol) on Docker, setting OPENAI\_API\_KEY=your-openai-key in the agent env. Show the job ID, job, and rollouts.
>
> Replace `your-openai-key` with your key. This makes real OpenAI API calls; no server restart is needed.

> [accordion] Run Codex on Daytona with an OpenAI API key
>
> Restart the server with **Daytona credentials**:
>
> ```bash theme={"system"}
> uv pip install "harbor[daytona]"
> export DAYTONA_API_KEY="your-daytona-key"
> harbor serve
> ```
>
> Submit the **OpenAI key per job** using your chosen client:
>
> > [tab] HTTP / cURL
> >
> > ```bash theme={"system"}
> > curl http://127.0.0.1:8390/jobs/submit \
> >   -H 'Content-Type: application/json' \
> >   -d '{
> >     "config": {
> >       "tasks": [{"name": "hello-world/hello-world"}],
> >       "agents": [{
> >         "name": "codex",
> >         "model_name": "openai/gpt-6.1-sol",
> >         "env": {"OPENAI_API_KEY": "your-openai-key"}
> >       }],
> >       "environment": {"type": "daytona"}
> >     }
> >   }'
> > ```
>
> > [tab] Harbor CLI
> >
> > ```bash theme={"system"}
> > harbor run --server http://127.0.0.1:8390 \
> >   --task hello-world/hello-world \
> >   --agent codex --model openai/gpt-6.1-sol \
> >   --agent-env OPENAI_API_KEY=your-openai-key \
> >   --env daytona --watch
> > ```
>
> > [tab] Python SDK
> >
> > ```python submit_job.py theme={"system"}
> > from harbor import Harbor
> >
> > client = Harbor()
> > job = client.jobs.submit(config={
> >     "tasks": [{"name": "hello-world/hello-world"}],
> >     "agents": [{
> >         "name": "codex",
> >         "model_name": "openai/gpt-6.1-sol",
> >         "env": {"OPENAI_API_KEY": "your-openai-key"},
> >     }],
> >     "environment": {"type": "daytona"},
> > })
> > job_id = job.job_id
> > print(job_id)
> > print(client.jobs.get(job_id))
> > print(client.rollouts.list(job_id=job_id))
> > client.close()
> > ```
>
> > [tab] MCP
> >
> > > Use the Harbor MCP server at [http://127.0.0.1:8390/mcp](http://127.0.0.1:8390/mcp) to run hello-world/hello-world with Codex (openai/gpt-6.1-sol) on Daytona, setting OPENAI\_API\_KEY=your-openai-key in the agent env. Show the job ID, job, and rollouts.

## Check progress and results

**Recommended: use the Harbor viewer** to monitor live progress and browse results and artifacts:

```bash theme={"system"}
harbor view --server http://127.0.0.1:8390
```

![Rollouts move from queued to running to finished. Queued rollouts can pause or cancel; running rollouts can cancel through cancelling. Retry creates a new rollout.](https://mintcdn.com/harborframework/vzYWOwnch5t54m5e/images/rollout-lifecycle.png?fit=max\&auto=format\&n=vzYWOwnch5t54m5e\&q=85\&s=3641a8234e7a9280ad02de14bdf11c55)

Rollout lifecycle.

> [accordion] Check through the API
>
> Use the returned ID with [Get job](https://docs.harborframework.com/api-sdk/jobs/get-job.md) and [List rollouts](https://docs.harborframework.com/api-sdk/rollouts/list-rollouts.md):
>
> ```bash theme={"system"}
> JOB_ID="paste-job-id"
> curl "http://127.0.0.1:8390/jobs/$JOB_ID"
> curl "http://127.0.0.1:8390/rollouts?job_id=$JOB_ID"
> ```
>
> Poll until `finished` or `cancelled`. `finished` means execution ended; inspect rollout `error_type` and `rewards` for the outcome.
>
> [Get rollout](https://docs.harborframework.com/api-sdk/rollouts/get-rollout.md) returns its full result and artifact URI map. Pass a file URI to [Download](https://docs.harborframework.com/api-sdk/files/download.md), or use `client.download(uri, destination)` to save it locally.

> [accordion] Request behavior
>
> * Submit a `JobConfig`; the server resolves it before accepting work.
> * Set `dry_run: true` to return `{ "n_trials": 2 }` without creating a job or reserving an idempotency key. In the SDK, use `client.jobs.submit(config, dry_run=True).n_trials`; in the CLI, add `--dry-run`. Resolution may download tasks.
> * Lists return `items`, `total`, and `next_page_token`. Pass the token as `page_token` with unchanged filters and sort; an empty token ends traversal. `page_size` defaults to 50 (also for zero) and is capped at 100. Detail endpoints return full records.
> * Actions accept `job_ids` or `rollout_ids` arrays; SDK methods also accept a single ID.
> * Pause affects queued rollouts. Cancel stops queued and running rollouts; `force: false` lets running rollouts finish. Retry creates a new rollout ID in the same job.
> * Submission and retry accept an optional `Idempotency-Key`. The SDK and CLI generate one per call, except dry runs; supply `idempotency_key` in the SDK to reuse it after a lost response. Duplicates return `409` with the original job ID or retry rollout IDs. The SDK does not retry requests automatically.
> * HTTP errors return `{"error": {"code": "...", "message": "..."}}`; the SDK raises `harbor.client.APIError`. `Harbor-API-Version` is optional; the SDK supplies it.

## Server settings

| Setting | Behavior |
| - | - |
| `--host`, `--port` | Default: `127.0.0.1:8390`. |
| `--data-dir` | Defaults to `.harbor`; persists jobs, results, and artifacts. Use one server process per directory. |
| `--max-concurrent` | Default: `8092` running rollouts across all jobs. Each job also has `n_concurrent_trials`. |
| `HARBOR_API_KEY` | Optional locally; required for non-local binding or relay. When set, clients send `Authorization: Bearer <key>`. The CLI and SDK read it from their environment. |

Use HTTPS for remote deployments. Set sandbox-provider credentials, such as `DAYTONA_API_KEY`, on the server; send model credentials per job through `agents[].env`. `environment.env` sets variables **inside** the sandbox, not provider credentials.

The **API & SDK** tab has the [endpoint reference](https://docs.harborframework.com/api-sdk/jobs/submit-job.md), and your server exposes its OpenAPI schema at `/openapi.json`.

## Discover agents and sandboxes

List the server's built-in providers and inspect their configuration options:

```bash theme={"system"}
curl http://127.0.0.1:8390/agents
curl http://127.0.0.1:8390/agents/codex/options
curl http://127.0.0.1:8390/sandboxes
curl http://127.0.0.1:8390/sandboxes/docker/options
```

Or use the Python SDK:

```python theme={"system"}
from harbor import Harbor

with Harbor() as client:
    print(client.agents.list().agents)
    print(client.agents.options("codex"))
    print(client.sandboxes.list().sandboxes)
    print(client.sandboxes.options("docker"))
```

Options are returned as JSON Schema. See the [agents](https://docs.harborframework.com/api-sdk/agents/list-agents.md)
and [sandboxes](https://docs.harborframework.com/api-sdk/sandboxes/list-sandboxes.md) reference for details.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.