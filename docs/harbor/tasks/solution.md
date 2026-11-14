> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Solution

> Task solutions.

The **`solution/`** folder is **optional**. It holds a reference script used by the oracle agent to sanity-check that a task is solvable. Solutions often help when debugging tasks or sandbox integrations.

## Required script

| OS | File |
| - | - |
| Linux | `solution/solve.sh` |
| Windows | `solution/solve.bat` |

Harbor chooses the extension from `[environment].os` in [task.toml](https://docs.harborframework.com/tasks/configuration.md).

Additional files (scripts, data) are allowed. At runtime Harbor copies the folder to `/solution` and runs the solve script from the task working directory.

## When to include a solution

* **Recommended** while authoring tasks — run `harbor run` with the Oracle agent to verify the task is solvable.
* **Optional** for published benchmarks — omit if you do not want a public reference implementation.

Without `solution/`, the Oracle agent cannot run.

## Configuration

```toml title="task.toml" theme={"system"}
[solution]
env = { API_KEY = "sk-test-123" }
```

Variables are applied when the Oracle executes `solve.sh`.

## Multiple solutions

Use `--ae` to pass an environment variable, then choose an implementation in `solution/solve.sh`. **You choose the variable name and write the selection logic in `solve.sh`, so you can support as many solutions as you need.** It can run `solve2.sh`, `solution2/solve.sh`, a Python script, or any other implementation. Harbor copies the entire `solution/` directory, so implementations can share helpers or use separate supporting files.

The [hello-world-multiple-solutions example](https://github.com/harbor-framework/harbor/tree/main/examples/tasks/hello-world-multiple-solutions) uses `SOLUTION` to select a second correct solution. From the repository root:

```bash theme={"system"}
uv run harbor run -p examples/tasks/hello-world-multiple-solutions -a oracle \
  --ae SOLUTION=2
```

Omit `--ae` to run the original solution. Both should receive reward `1`. `SOLUTION` is just this example's choice, not a special Harbor variable. Avoid setting the same variable in `[solution.env]`, which takes precedence over `--ae`.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.