> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Registries

> Create custom registries of datasets.

Harbor supports running and downloading datasets from custom registries via `--registry-url` or `--registry-path`.

> [!NOTE]
>
> This document describes how anyone can create a custom registry. By default, Harbor uses Harbor Hub as the registry.

## Schema

`registry.json` is a JSON array. Each entry defines one version of a dataset and must include `name`, `version`, `description`, and `tasks`. The `metrics` field is optional.

```json title="registry.json" theme={"system"}
[
  {
    "name": "<dataset>",
    "version": "<version>",
    "description": "<description>",
    "tasks": [
      {
        "name": "<task>",
        "git_url": "https://github.com/<org>/<repo>.git",
        "git_commit_id": "<commit-sha>",
        "path": "<path/to/task>"
      }
    ],
    "metrics": [
      {
        "type": "mean",
        "kwargs": {}
      }
    ]
  }
]
```

### Dataset fields

**name** `string`

Dataset name used with `--dataset`.

**version** `string`

Dataset version. Define another top-level entry to publish another version of the same dataset.

**description** `string`

Human-readable description of the dataset.

**tasks** `list[Task]`

Tasks included in the dataset.

**metrics** `list[Metric]` (default: \[])

Optional reward aggregations. See [Metrics](https://docs.harborframework.com/datasets/metrics.md).

### Task fields

Every task must include `name` and `path`. The Git fields are optional.

**tasks\[].name** `string`

Task name.

**tasks\[].git\_url** `string | null` (default: null)

Git repository containing the task. Without `--repo`, omitting this field makes `path` a local path.

**tasks\[].git\_commit\_id** `string | null` (default: null)

Commit containing the task. If omitted, Harbor resolves the repository's default branch.

**tasks\[].path** `string`

Path to the task directory, relative to the Git repository root when `git_url` is set.

### Metric fields

**metrics\[].type** `"sum" | "min" | "max" | "mean" | "uv-script"` (default: "mean")

Metric implementation used to aggregate task rewards.

**metrics\[].kwargs** `object` (default: \{})

Keyword arguments passed to the metric implementation.

> [!NOTE]
>
> When loading the registry with [`--repo`](https://docs.harborframework.com/datasets/git-repos), Harbor fills omitted `git_url` and `git_commit_id` values from the selected repository and resolved commit.

> [!TIP]
>
> Use unique dataset name and version pairs, and pin remote tasks to full commit SHAs for reproducible runs.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.