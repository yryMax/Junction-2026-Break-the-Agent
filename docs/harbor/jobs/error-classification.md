> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Error classification

> The single reference for how Harbor treats every trial error: retry, grade, and whether the reward counts.

[`error_policy.py`](https://github.com/harbor-framework/harbor/blob/main/src/harbor/models/trial/error_policy.py) is the **single source of truth** for trial errors. Each exception type maps directly to an `ErrorPolicy` with three independent fields: `should_retry`, `should_grade`, and `has_valid_reward`.

![Decision tree: when verification runs and whether its reward is valid](https://mintcdn.com/harborframework/M26tNatDkzs-cxdi/images/error-classification.png?fit=max\&auto=format\&n=M26tNatDkzs-cxdi\&q=85\&s=4d79707fce542663aa638547e1d5929f)

Policy fields include any `override_error_policy`. **Retries are decided after the attempt finishes:** retry settings determine whether to start another attempt through the same flow. **Disabling retries does not disable grading.**

`result.reward_valid` tells you whether the trial's reward can count in **benchmarks or RL training**. Once the verifier returns a reward:

* **No errors:** `result.reward_valid=True`.
* **Errors occurred:** `result.reward_valid=True` only if `policy.has_valid_reward=True`.

> [!NOTE]
>
> **Both 0 and 1 can be valid reward signals.** `reward_valid` indicates whether a reward can be used for RL and job reward aggregation, not whether the task succeeded. Invalid rewards contribute to **neither the numerator nor the denominator** of the mean. For example, rewards `0` (valid), `1` (valid), and `1` (invalid) produce `(0 + 1) / 2 = 0.5`.

If verification is skipped or returns no reward, `result.reward_valid=False`. Running the verifier alone does not guarantee reward validity.

## Exception types

Defaults from `_POLICY_GROUPS`. Unlisted exception types use `should_retry=True`, `should_grade=True`, `has_valid_reward=True`.

| Retry | Grade | Reward valid | Exception types |
| - | - | - | - |
| No | Yes | Yes | `NonZeroAgentExitCodeError`, `AgentTimeoutError`, `ContextWindowExceededError`, `OutputTokenExceededError`, `ContextLengthExceededError`, `OutputLengthExceededError`, `AgentSafetyRefusalError`, `ContentPolicyViolationError` |
| Yes | No | No | `EnvironmentStartTimeoutError`, `SandboxBuildFailedError`, `AgentSetupTimeoutError`, `HealthcheckError`, `SandboxLikelyOutOfMemoryError`, `MemoryLimitExceededError`, `SSHAccessError`, `GKEExecStreamClosedError`, `GKEPodNotFoundError`, `RuntimeRequestError`, `AddTestsDirError`, `DownloadVerifierDirError` |
| Yes | Yes | No | `ApiError`, `ApiRateLimitError`, `ApiInternalServerError`, `ApiOverloadedError`, `ApiConnectionClosedError`, `ApiResponseStalledError`, `UnknownApiError`, `NetworkConnectionError`, `ApiConnectionError`, `ApiUsageLimitError`, `RateLimitError`, `InternalServerError`, `ServiceUnavailableError`, `APIConnectionError`, `APIError`, `Timeout` |
| No | Yes | No | `UnsupportedModelParametersError`, `BadRequestError`, `UnprocessableEntityError`, `UnsupportedParamsError`, `APIResponseValidationError`, `BudgetExceededError`, `VerifierTimeoutError`, `RewardFileNotFoundError`, `RewardFileEmptyError`, `VerifierOutputParseError` |
| No | No | No | `AgentAuthenticationError`, `ModelNotFoundError`, `ApiProviderResourceNotFoundError`, `AuthenticationError`, `PermissionDeniedError`, `NotFoundError`, `ServiceOperationsUnsupportedError`, `VercelScopeError`, `DspyImportError`, `MissingExtraError`, `RegradeError`, `CancelledError` |

## Overriding the policy

Set `override_error_policy` in `JobConfig` (applied to its trials) or `TrialConfig`. Keys are exact exception type names; omitted fields keep their defaults. For example, add this to a job config:

```yaml theme={"system"}
retry:
  max_retries: 2
override_error_policy:
  NonZeroAgentExitCodeError:
    should_retry: true
  ApiRateLimitError:
    should_retry: false
    should_grade: true
    has_valid_reward: false
```

[Retry settings](https://docs.harborframework.com/jobs/configs.md#retry) still apply: explicit retry filters take precedence over `policy.should_retry`, with exclusions winning over inclusions. Set `retry.max_retries: 0` (the default) to disable automatic retries, for example when the training loop owns them.

## Reading the classification

```python theme={"system"}
result = TrialResult.model_validate_json(path.read_text())
error = result.exception_info  # ApiRateLimitError with no overrides.

print(error.exception_type)          # "ApiRateLimitError"
print(error.policy.should_retry)     # True
print(error.policy.should_grade)     # True
print(error.policy.has_valid_reward) # False
print(result.reward_valid)           # False
```

`ExceptionInfo.policy` and `TrialResult.reward_valid` are computed and included in serialized JSON. Each exception stores its applicable `policy_override`, so overrides survive saving and loading results.


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.