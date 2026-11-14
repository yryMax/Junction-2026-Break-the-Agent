> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Core concepts

> Core concepts for understanding Harbor.

Harbor has the following core concepts:

## Task

A task defines one or more instructions, a sandbox environment, and a verifier. Tasks are used to evaluate agents and models and are implemented as directories in the [Harbor task format](https://docs.harborframework.com/tasks/overview.md). See [Multi-step](https://docs.harborframework.com/tasks/multi-step.md) for tasks with multiple instructions.

## Dataset

A [dataset](https://docs.harborframework.com/datasets/datasets.md) is a collection of tasks. Datasets are used to evaluate agents and models. Usually, a dataset corresponds to a benchmark (e.g. Terminal-Bench or SWE-Bench Verified). Datasets can optionally be distributed through [Harbor Hub](https://hub.harborframework.com).

## Agent

An agent is a program that completes tasks. Harbor includes [pre-integrated agents](https://docs.harborframework.com/agents/pre-integrated-agents.md) and supports [custom agents](https://docs.harborframework.com/agents/custom-agents.md) implemented with the `BaseAgent` interface.

## Sandbox

A sandbox is an isolated environment for running tasks. Harbor includes [pre-integrated sandboxes](https://docs.harborframework.com/sandboxes/pre-integrated-sandboxes.md) such as [Daytona](https://www.daytona.io/) and [Modal](https://modal.com/products/sandboxes). Other runtimes can be integrated as [custom sandboxes](https://docs.harborframework.com/sandboxes/custom-sandboxes.md) by implementing `BaseEnvironment`.

## Verifier

A [verifier](https://docs.harborframework.com/tasks/verifier.md) evaluates the agent's work and produces the task's reward.

## Trial

A trial is one agent's attempt at completing one task.

## Job

A [job](https://docs.harborframework.com/jobs/run-a-job.md) is a collection of trials used to evaluate agents and models. A job can combine datasets, agents, tasks, and models.

Under the hood, a job generates trials and runs them in parallel.

## Trajectory

A trajectory is the conversation and action history of the agent and user while completing a task. Trajectories are one of the most useful tools for understanding and debugging agent behavior.

Harbor's standard trajectory format is [ATIF](https://docs.harborframework.com/agents/atif.md). Trajectories can also be [loaded into a later agent session](https://docs.harborframework.com/jobs/loading-trajectories.md).


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.