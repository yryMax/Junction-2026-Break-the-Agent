> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Web UI

> Add secrets and launch a remote rollout from Harbor Hub

## Adding Secrets

Unless you are running the `oracle` or `nop` agents which do not require API model inference, the first step will be adding your API secrets.
On the Harbor Hub, everything is owned by organizations. This includes jobs, trials, packages, and secrets.
When running remote rollouts, secrets will be selected from the organization chosen to own the job.

To add secrets to your personal org on the Web UI, first go to [The Hub](https://hub.harborframework.com)

Click on your profile in the top right corner:

![The Harbor Hub home page, listing published datasets](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/hub-home.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=f86605d6cc0529b7a5f16d339a442abb)

Then click on your settings tab:

![Profile Page](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/hub-settings.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=5feff5526a2797eeb0bc015a7e7d1b31)

Scroll down to the secrets section, and click "Add secret":

![Secrets Tab](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/add-secrets.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=96a8b20a0d141460ac1e6863a4ae6ecb)

Add a registry secret or an environment variable secret:

![Secrets Modal](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/secret-modal.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=4acc892a2410a0d6e6b4bc32952bd49d)

Environment secrets can be injected
into the agent runtime during a rollout, registry secrets are never injected, and only used to resolve private image repositories referenced by tasks.

## Adding Secrets To Organizations

To add a secret to an organization you own, click the "Organization" button on the top of the page:

![Org Button](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/org-button.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=1b850c3920a4aa30adf9aea2efdf8bb6)

Select an organization that you own, or create a new one.

Then select the settings tab of the organization to view the saved secrets there. Note that only organization owners may modify secrets.

![Add Org Secrets](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/org-settings.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=6cc0cb9ab529b75521d5d10511c02ebb)

Finally, add a secret:

![Add Org Secrets](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/org-add-secret.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=d9ee70aa8882fcf365016930d7c32881)

## Launching a Job

After adding your secrets, go to the [job launcher](https://hub.harborframework.com/jobs/launch).

First, select the organization that should own this job.

![Select Organization](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/select-organization.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=7b05a41f0be5c45dcab5ef6f1e93a9cf)

Then, select the dataset or task that you would like to evaluate. You can select tasks and
datasets that have been uploaded to the hub, or to GitHub. For private GitHub tasks, you must
first connect your private GitHub repositories from your profile settings.

![Select Sources](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/select-source.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=0974476e14df30752f26d9f8a2bb30db)

### Adding Agents

Next, select the agent/model combinations that you would like to evaluate. Harbor Hub defaults to
**direct credential mode**, with **Disable credential proxying** checked. This injects selected
credentials into the task sandbox, where agent code can read them. Gateway policy and accounting
do not apply in direct mode.

To opt into **gateway mode**, uncheck **Disable credential proxying**. Supported inference provider
keys are then replaced with scoped proxy credentials. Selected non-provider secrets still arrive
with their real values; see [Agent Secrets](https://docs.harborframework.com/hosted-harbor/submitting-jobs.md#agent-secrets)
for provider support and credential handling.

![Select Agent](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/select-agent.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=d7c3215ec5994273accae1a1aae54966)

Custom agents must be stored on GitHub and compatible with ACP. Read `HOSTED_INFERENCE_TOKEN`
for the selected model credential. Gateway mode also sets `HOSTED_INFERENCE_URL`; direct mode
leaves it unset, so use the provider's normal endpoint. See
[Custom Agents](https://docs.harborframework.com/hosted-harbor/custom-agents.md#inference-credentials) for the full contract.

After selecting your agent, you may add additional environment variables that you would like set.
THIS FIELD IS NOT FOR SECRETS. Environment variables placed in these fields are persisted plaintext
in the job config. Open **Agent Configuration** to configure the selected agent's options.
Agents with a declared options schema show typed controls and descriptions for their supported
kwargs. You can also inspect a deployed agent's options with
`harbor agent schema claude-code --hub`, replacing `claude-code` with the agent name.

### Choosing Secrets to Inject

Once you have finished configuring the agents, choose which credentials each
one receives. The `Secrets` section lists the names of every secret uploaded to the
owning organization. You can select any number of them per
agent, including more than one inference provider key.

![Select Secrets](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/select-secret-settings.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=ffffe47a6f479cdabe030cd84b46a9c7)

Every selected secret is injected. One of them is also renamed: the key whose
provider matches that agent's selected model arrives under the name the agent
expects, so you do not have to store a second copy of it under a different
name. Everything else you selected is injected under its own name.

For example, running `claude-code` against `openrouter/zai/glm-5.2`, you would
select your organization's `OPENROUTER_API_KEY`. It matches the model's
`openrouter/` provider, and `claude-code` reads its credential from
`ANTHROPIC_API_KEY`, so that is the name it is injected under. Any other
secrets you selected, an `OPENAI_API_KEY`, a token a task needs, arrive
unchanged.

If none of the selected secrets match the model's provider, nothing is renamed
and the agent starts without a credential, so the trial fails with a
credential error. Selecting only `OPENAI_API_KEY` for that OpenRouter model
does exactly this: the key is injected under its own name, but it is not the
one the model needs.

### Overall Job Settings

The overall job settings include the name, retries, attempts, concurrency, and timeout multiplier.

![Select Job Settings](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/select-job-settings.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=84c6150abc296067262566de98a176cf)

**Name**

Naming the job is recommended as the default timestamp can be difficult to identify later.

**Retries**

Retries set the maximum number of additional executions for each trial after an eligible exception.
The default is `0`, which disables retries. Eligibility depends on the exception include/exclude
settings and the remaining retry budget; a low score alone does not trigger a retry. See
[Retry Settings](https://docs.harborframework.com/hosted-harbor/submitting-jobs.md#retry-settings).

**Attempts**

Attempts per task set the number of independent trials for each task and agent combination.
For example, 4 attempts with 3 retries allow up to `4 × (1 + 3) = 16` executions per task per agent:
each of the 4 trials can run once and retry up to 3 times. Retries replace a failed execution;
they do not add scored samples. There are still 4 trial slots, and a trial that exhausts its retries
may finish with an error instead of a score.

**Concurrent Trials**

Concurrency can be set based on your api key rate limits. Harbor Hub will monitor concurrency
and back off if rate limits are causing failures, but it is recommended to set concurrency
such that rate limit failures do not occur.

**Timeout Multiplier**

Finally, the timeout multiplier refers to the time limit an agent has to complete a task.
The time limit is declared in the task, but if you would like to modify this timeout without
modifying the task, you can set the timeout multiplier to something other than `1.0`.
e.g. `0.5` gives half of the time limit, `2.0` gives twice as much time.

### Advanced Job Settings

![Select Advanced Settings](https://mintcdn.com/harborframework/Gs5DwVkRMDXhj7Hx/images/hosted-harbor/advanced-options.png?fit=max\&auto=format\&n=Gs5DwVkRMDXhj7Hx\&q=85\&s=f6302b5b17990472135b21f05050e6ed)


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.