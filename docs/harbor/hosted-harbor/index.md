> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Hosted Harbor

> Launching Jobs On The Hub

The [Harbor Hub](https://hub.harborframework.com) can now be used to run remote harbor rollouts.
This guide provides instructions for launching remote jobs through the CLI, web UI, and API.

First, visit [the job launcher](https://hub.harborframework.com/jobs/launch).
Click the link in the banner to fill out a google form for access to remote rollouts.

Then follow the [API key creation guide](https://docs.harborframework.com/hosted-harbor/api-key.md) to create an API key.

## Interfaces

| Page | Use it for |
| - | - |
| [Web UI](https://docs.harborframework.com/hosted-harbor/web-ui.md) | Adding secrets and launching a job from the browser |
| [CLI](https://docs.harborframework.com/hosted-harbor/cli.md) | Launching with `harbor run --launch`, then browsing jobs and trials |
| [API overview](https://docs.harborframework.com/hosted-harbor/api.md) | Base URL, authentication, and error shapes |

## API Reference

| Page | Endpoints |
| - | - |
| [Submitting jobs](https://docs.harborframework.com/hosted-harbor/submitting-jobs.md) | `POST /job-submit`, `GET /job-status` |
| [Custom agents](https://docs.harborframework.com/hosted-harbor/custom-agents.md) | Running an ACP agent from a GitHub repository |
| [Managing secrets](https://docs.harborframework.com/hosted-harbor/secrets.md) | `/secrets`, `/secrets/preflight` |
| [Registry credentials](https://docs.harborframework.com/hosted-harbor/registry-credentials.md) | `/registry-credentials` |
| [Leaderboards](https://docs.harborframework.com/harbor-hub/leaderboards.md) | Curated leaderboards from the CLI |


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.