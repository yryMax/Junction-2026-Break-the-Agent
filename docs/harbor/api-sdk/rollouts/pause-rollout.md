> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Pause Rollout

> Pause selected queued rollouts. Running rollouts cannot be paused.



## OpenAPI

````yaml /api-sdk/openapi.json post /rollouts/pause
openapi: 3.1.0
info:
  title: Harbor API
  version: '2026-10-30'
  description: The harbor serve API.
servers:
  - url: http://127.0.0.1:{port}
    description: Local server
    variables:
      port:
        default: '8390'
  - url: https://{host}
    description: Remote server
    variables:
      host:
        default: your-server.example.com
security:
  - {}
  - BearerAuth: []
tags:
  - name: Jobs
  - name: Rollouts
  - name: Files
  - name: Queries
  - name: Agents
  - name: Sandboxes
  - name: Hub
paths:
  /rollouts/pause:
    post:
      tags:
        - Rollouts
      summary: Pause Rollout
      description: Pause selected queued rollouts. Running rollouts cannot be paused.
      operationId: pause_rollout_rollouts_pause_post
      parameters:
        - name: Harbor-API-Version
          in: header
          required: false
          schema:
            type: string
            default: '2026-10-30'
            title: Harbor-Api-Version
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/RolloutActionRequest'
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ActionResponse_RolloutDetail_'
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.rollouts.pause("ROLLOUT_ID")
            print(result)
            client.close()
components:
  schemas:
    RolloutActionRequest:
      properties:
        rollout_ids:
          items:
            type: string
            format: uuid
          type: array
          minItems: 1
          title: Rollout Ids
      additionalProperties: false
      type: object
      required:
        - rollout_ids
      title: RolloutActionRequest
    ActionResponse_RolloutDetail_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/harbor__server__models__RolloutDetail'
          type: array
          title: Items
      type: object
      required:
        - items
      title: ActionResponse[RolloutDetail]
    harbor__server__models__RolloutDetail:
      properties:
        created_at:
          type: string
          format: date-time
          title: Created At
        updated_at:
          type: string
          format: date-time
          title: Updated At
        started_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Started At
        finished_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Finished At
        id:
          type: string
          format: uuid
          title: Id
        job_id:
          type: string
          format: uuid
          title: Job Id
        name:
          type: string
          title: Name
        state:
          type: string
          enum:
            - queued
            - running
            - paused
            - cancelling
            - finished
            - cancelled
          title: State
        retry_ids:
          items:
            type: string
            format: uuid
          type: array
          title: Retry Ids
          description: >-
            All rollout IDs in the retry group, including the original and
            current rollout, newest first. A rollout without retries returns
            [id].
        phase:
          anyOf:
            - type: string
              enum:
                - environment_setup
                - agent_setup
                - agent_execution
                - verifier
                - collection
                - cleanup
            - type: 'null'
          title: Phase
        task:
          $ref: '#/components/schemas/TaskIdentity'
        agent:
          $ref: '#/components/schemas/AgentIdentity'
        model:
          $ref: '#/components/schemas/ModelIdentity'
        rewards:
          anyOf:
            - additionalProperties:
                anyOf:
                  - type: number
                  - type: integer
                  - type: 'null'
              type: object
            - type: 'null'
          title: Rewards
        usage:
          $ref: '#/components/schemas/Usage'
        error_type:
          anyOf:
            - type: string
            - type: 'null'
          title: Error Type
        lock:
          $ref: '#/components/schemas/TrialLock'
        artifacts:
          additionalProperties:
            type: string
          type: object
          title: Artifacts
        storage_error:
          anyOf:
            - $ref: '#/components/schemas/ExceptionInfo'
            - type: 'null'
        result:
          anyOf:
            - $ref: '#/components/schemas/TrialResult'
            - type: 'null'
      type: object
      required:
        - created_at
        - updated_at
        - started_at
        - finished_at
        - id
        - job_id
        - name
        - state
        - retry_ids
        - phase
        - task
        - agent
        - model
        - rewards
        - usage
        - error_type
        - lock
        - artifacts
        - storage_error
        - result
      title: RolloutDetail
    TaskIdentity:
      properties:
        name:
          type: string
          title: Name
        digest:
          type: string
          title: Digest
        source:
          anyOf:
            - type: string
            - type: 'null'
          title: Source
      type: object
      required:
        - name
        - digest
        - source
      title: TaskIdentity
      description: Resolved task content; source is a label, not a registry identity.
    AgentIdentity:
      properties:
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
        version:
          anyOf:
            - type: string
            - type: 'null'
          title: Version
      type: object
      required:
        - name
        - version
      title: AgentIdentity
      description: Configured implementation name/import path and observed runtime version.
    ModelIdentity:
      properties:
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
        provider:
          anyOf:
            - type: string
            - type: 'null'
          title: Provider
        reasoning_effort:
          anyOf:
            - type: string
            - type: 'null'
          title: Reasoning Effort
      type: object
      required:
        - name
        - provider
        - reasoning_effort
      title: ModelIdentity
      description: Configured model string and reasoning effort, with observed provider.
    Usage:
      properties:
        n_input_tokens:
          anyOf:
            - type: integer
            - type: 'null'
          title: N Input Tokens
        n_cached_input_tokens:
          anyOf:
            - type: integer
            - type: 'null'
          title: N Cached Input Tokens
        n_output_tokens:
          anyOf:
            - type: integer
            - type: 'null'
          title: N Output Tokens
        cost_usd:
          anyOf:
            - type: number
            - type: 'null'
          title: Cost Usd
      type: object
      required:
        - n_input_tokens
        - n_cached_input_tokens
        - n_output_tokens
        - cost_usd
      title: Usage
      description: >-
        Recorded usage, with null for unknown values; input includes cached
        input.
    TrialLock:
      properties:
        schema_version:
          type: integer
          title: Schema Version
          default: 2
        task:
          $ref: '#/components/schemas/TaskLock'
        install_only:
          type: boolean
          title: Install Only
          default: false
        timeout_multiplier:
          type: number
          title: Timeout Multiplier
          default: 1
        agent_timeout_multiplier:
          anyOf:
            - type: number
            - type: 'null'
          title: Agent Timeout Multiplier
        verifier_timeout_multiplier:
          anyOf:
            - type: number
            - type: 'null'
          title: Verifier Timeout Multiplier
        agent_setup_timeout_multiplier:
          anyOf:
            - type: number
            - type: 'null'
          title: Agent Setup Timeout Multiplier
        environment_build_timeout_multiplier:
          anyOf:
            - type: number
            - type: 'null'
          title: Environment Build Timeout Multiplier
        extra_instructions:
          anyOf:
            - items:
                $ref: '#/components/schemas/ExtraInstructionLock'
              type: array
            - type: 'null'
          title: Extra Instructions
        agent:
          $ref: '#/components/schemas/AgentConfig'
        user_agent:
          anyOf:
            - $ref: '#/components/schemas/UserAgentConfig'
            - type: 'null'
        user_persona:
          anyOf:
            - $ref: '#/components/schemas/ExtraInstructionLock'
            - type: 'null'
        user_prompt_template:
          anyOf:
            - $ref: '#/components/schemas/ExtraInstructionLock'
            - type: 'null'
        bridge_prompt:
          anyOf:
            - $ref: '#/components/schemas/ExtraInstructionLock'
            - type: 'null'
        bridge_inputs:
          additionalProperties:
            $ref: '#/components/schemas/ExtraInstructionLock'
          type: object
          title: Bridge Inputs
        skills:
          items:
            $ref: '#/components/schemas/AgentSkillLock'
          type: array
          title: Skills
        environment:
          $ref: '#/components/schemas/EnvironmentConfig-Output'
        extra_docker_compose:
          anyOf:
            - items:
                $ref: '#/components/schemas/ExtraDockerComposeLock'
              type: array
            - type: 'null'
          title: Extra Docker Compose
        verifier:
          $ref: '#/components/schemas/VerifierLock'
        artifacts:
          items:
            anyOf:
              - type: string
              - $ref: '#/components/schemas/ArtifactConfig'
          type: array
          title: Artifacts
        override_error_policy:
          additionalProperties:
            $ref: '#/components/schemas/ErrorPolicyOverride'
          type: object
          title: Override Error Policy
        source_trial:
          anyOf:
            - $ref: '#/components/schemas/SourceTrialLock'
            - type: 'null'
      type: object
      required:
        - task
        - agent
        - environment
        - verifier
      title: TrialLock
    ExceptionInfo:
      properties:
        exception_type:
          type: string
          title: Exception Type
        exception_message:
          type: string
          title: Exception Message
        exception_traceback:
          type: string
          title: Exception Traceback
        occurred_at:
          type: string
          format: date-time
          title: Occurred At
        policy_override:
          anyOf:
            - $ref: '#/components/schemas/ErrorPolicyOverride'
            - type: 'null'
        policy:
          $ref: '#/components/schemas/ErrorPolicy'
          readOnly: true
      type: object
      required:
        - exception_type
        - exception_message
        - exception_traceback
        - occurred_at
        - policy
      title: ExceptionInfo
      description: Information about an exception that occurred during trial execution.
    TrialResult:
      properties:
        id:
          type: string
          format: uuid
          title: Id
        task_name:
          type: string
          title: Task Name
        trial_name:
          type: string
          title: Trial Name
        trial_uri:
          type: string
          title: Trial Uri
        task_id:
          anyOf:
            - $ref: '#/components/schemas/LocalTaskId'
            - $ref: '#/components/schemas/GitTaskId'
            - $ref: '#/components/schemas/PackageTaskId'
          title: Task Id
        source:
          anyOf:
            - type: string
            - type: 'null'
          title: Source
        task_checksum:
          type: string
          title: Task Checksum
        config:
          $ref: '#/components/schemas/TrialConfig'
        agent_info:
          $ref: '#/components/schemas/AgentInfo'
        agent_result:
          anyOf:
            - $ref: '#/components/schemas/AgentContext'
            - type: 'null'
        verifier_result:
          anyOf:
            - $ref: '#/components/schemas/VerifierResult'
            - type: 'null'
        verifier_environment_mode:
          anyOf:
            - $ref: '#/components/schemas/VerifierEnvironmentMode'
            - type: 'null'
          description: >-
            Resolved verifier environment mode ('shared' or 'separate') the
            trial-level verify ran in. None for multi-step trials: the mode is
            resolved per step there and is not recorded yet.
        exception_info:
          anyOf:
            - $ref: '#/components/schemas/ExceptionInfo'
            - type: 'null'
        started_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Started At
        finished_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Finished At
        environment_setup:
          anyOf:
            - $ref: '#/components/schemas/TimingInfo'
            - type: 'null'
        agent_setup:
          anyOf:
            - $ref: '#/components/schemas/TimingInfo'
            - type: 'null'
        agent_execution:
          anyOf:
            - $ref: '#/components/schemas/TimingInfo'
            - type: 'null'
        verifier:
          anyOf:
            - $ref: '#/components/schemas/TimingInfo'
            - type: 'null'
        step_results:
          anyOf:
            - items:
                $ref: '#/components/schemas/StepResult'
              type: array
            - type: 'null'
          title: Step Results
        reward_valid:
          type: boolean
          title: Reward Valid
          readOnly: true
      type: object
      required:
        - task_name
        - trial_name
        - trial_uri
        - task_id
        - task_checksum
        - config
        - agent_info
        - reward_valid
      title: TrialResult
    TaskLock:
      properties:
        name:
          type: string
          title: Name
        version:
          anyOf:
            - type: string
            - type: 'null'
          title: Version
        type:
          type: string
          enum:
            - local
            - git
            - package
          title: Type
        digest:
          type: string
          title: Digest
        source:
          anyOf:
            - type: string
            - type: 'null'
          title: Source
        path:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: Path
        git_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Git Url
        git_commit_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Git Commit Id
      type: object
      required:
        - name
        - type
        - digest
      title: TaskLock
    ExtraInstructionLock:
      properties:
        path:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: Path
        digest:
          type: string
          title: Digest
      type: object
      required:
        - digest
      title: ExtraInstructionLock
    AgentConfig:
      properties:
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
        import_path:
          anyOf:
            - type: string
            - type: 'null'
          title: Import Path
        model_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Model Name
        base_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Base Url
          description: Override the inference endpoint used by the agent.
        model_api_key_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Model Api Key Name
          description: Name of the environment variable containing the inference API key.
        model_api_format:
          anyOf:
            - $ref: '#/components/schemas/ModelApiFormat'
            - type: 'null'
          description: Inference API format supported by the agent.
        skip_model_parsing:
          type: boolean
          title: Skip Model Parsing
          description: Pass the model string unchanged without resolving its provider.
          default: false
        n_concurrent:
          anyOf:
            - type: integer
              minimum: 1
            - type: 'null'
          title: N Concurrent
          description: >-
            Per-agent cap on concurrent agent.run() phases. Must not exceed the
            job's n_concurrent_trials and is usually useful only when lower.
            When omitted, agent execution is only limited by
            n_concurrent_trials.
        concurrency_group:
          anyOf:
            - type: string
            - type: 'null'
          title: Concurrency Group
          description: >-
            Optional shared concurrency pool name for agent configs that should
            use the same n_concurrent limit.
        skills:
          items:
            anyOf:
              - type: string
              - type: string
                format: path
          type: array
          title: Skills
          description: >-
            Skill directories or source strings (git URLs, org/name[@ref]
            shorthand, or local paths). Job.create() / Trial.create() resolve
            any non-local entries to cached directories in-place.
        override_timeout_sec:
          anyOf:
            - type: number
            - type: 'null'
          title: Override Timeout Sec
        override_setup_timeout_sec:
          anyOf:
            - type: number
            - type: 'null'
          title: Override Setup Timeout Sec
        max_timeout_sec:
          anyOf:
            - type: number
            - type: 'null'
          title: Max Timeout Sec
        resume_trajectory:
          type: boolean
          title: Resume Trajectory
          description: >-
            For multi-step tasks, resume the agent's native session from the
            previous step instead of starting a fresh conversation on each step.
            Requires an agent with native resume support (capabilities.resume);
            the trial fails fast otherwise. No effect on single-step tasks.
          default: false
        load_trajectory:
          anyOf:
            - type: string
            - type: 'null'
          title: Load Trajectory
          description: >-
            Path to a trajectory to load as the agent's session before the first
            step, which then resumes it instead of starting fresh. A .jsonl file
            is the agent's native session format (lossless, same agent only;
            capabilities.load_native_trajectory); a .json file is an ATIF
            trajectory converted to the agent's native format (portable across
            supported agents; capabilities.load_atif_trajectory). The trial
            fails fast if the agent lacks the needed support. Composes with
            resume_trajectory.
        extra_allowed_hosts:
          items:
            type: string
          type: array
          title: Extra Allowed Hosts
          description: >-
            Run-specific hostnames or IP addresses merged into the effective
            agent phase allowlist during agent.run() only.
        include_logs:
          items:
            type: string
          type: array
          title: Include Logs
          description: >-
            Glob patterns of agent log files to download, relative to the agent
            logs directory. When set, only matching files are kept.
        exclude_logs:
          items:
            type: string
          type: array
          title: Exclude Logs
          description: >-
            Glob patterns of agent log files to skip when downloading. Applied
            after include_logs, so exclude wins on overlap.
        kwargs:
          additionalProperties: true
          type: object
          title: Kwargs
        env:
          additionalProperties:
            type: string
          type: object
          title: Env
        mcp_servers:
          items:
            $ref: '#/components/schemas/MCPServerConfig'
          type: array
          title: Mcp Servers
      type: object
      title: AgentConfig
    UserAgentConfig:
      properties:
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
        import_path:
          anyOf:
            - type: string
            - type: 'null'
          title: Import Path
        model_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Model Name
        base_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Base Url
          description: Override the inference endpoint used by the agent.
        model_api_key_name:
          anyOf:
            - type: string
            - type: 'null'
          title: Model Api Key Name
          description: Name of the environment variable containing the inference API key.
        model_api_format:
          anyOf:
            - $ref: '#/components/schemas/ModelApiFormat'
            - type: 'null'
          description: Inference API format supported by the agent.
        skip_model_parsing:
          type: boolean
          title: Skip Model Parsing
          description: Pass the model string unchanged without resolving its provider.
          default: false
        n_concurrent:
          anyOf:
            - type: integer
              minimum: 1
            - type: 'null'
          title: N Concurrent
          description: >-
            Per-agent cap on concurrent agent.run() phases. Must not exceed the
            job's n_concurrent_trials and is usually useful only when lower.
            When omitted, agent execution is only limited by
            n_concurrent_trials.
        concurrency_group:
          anyOf:
            - type: string
            - type: 'null'
          title: Concurrency Group
          description: >-
            Optional shared concurrency pool name for agent configs that should
            use the same n_concurrent limit.
        skills:
          items:
            anyOf:
              - type: string
              - type: string
                format: path
          type: array
          title: Skills
          description: >-
            Skill directories or source strings (git URLs, org/name[@ref]
            shorthand, or local paths). Job.create() / Trial.create() resolve
            any non-local entries to cached directories in-place.
        override_timeout_sec:
          anyOf:
            - type: number
            - type: 'null'
          title: Override Timeout Sec
        override_setup_timeout_sec:
          anyOf:
            - type: number
            - type: 'null'
          title: Override Setup Timeout Sec
        max_timeout_sec:
          anyOf:
            - type: number
            - type: 'null'
          title: Max Timeout Sec
        resume_trajectory:
          type: boolean
          title: Resume Trajectory
          description: >-
            For multi-step tasks, resume the agent's native session from the
            previous step instead of starting a fresh conversation on each step.
            Requires an agent with native resume support (capabilities.resume);
            the trial fails fast otherwise. No effect on single-step tasks.
          default: false
        load_trajectory:
          anyOf:
            - type: string
            - type: 'null'
          title: Load Trajectory
          description: >-
            Path to a trajectory to load as the agent's session before the first
            step, which then resumes it instead of starting fresh. A .jsonl file
            is the agent's native session format (lossless, same agent only;
            capabilities.load_native_trajectory); a .json file is an ATIF
            trajectory converted to the agent's native format (portable across
            supported agents; capabilities.load_atif_trajectory). The trial
            fails fast if the agent lacks the needed support. Composes with
            resume_trajectory.
        extra_allowed_hosts:
          items:
            type: string
          type: array
          title: Extra Allowed Hosts
          description: >-
            Run-specific hostnames or IP addresses merged into the effective
            agent phase allowlist during agent.run() only.
        include_logs:
          items:
            type: string
          type: array
          title: Include Logs
          description: >-
            Glob patterns of agent log files to download, relative to the agent
            logs directory. When set, only matching files are kept.
        exclude_logs:
          items:
            type: string
          type: array
          title: Exclude Logs
          description: >-
            Glob patterns of agent log files to skip when downloading. Applied
            after include_logs, so exclude wins on overlap.
        kwargs:
          additionalProperties: true
          type: object
          title: Kwargs
        env:
          additionalProperties:
            type: string
          type: object
          title: Env
        mcp_servers:
          items:
            $ref: '#/components/schemas/MCPServerConfig'
          type: array
          title: Mcp Servers
        user_persona_path:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: User Persona Path
        user_prompt_template_path:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: User Prompt Template Path
        bridge:
          $ref: '#/components/schemas/BridgeConfig'
      type: object
      required:
        - bridge
      title: UserAgentConfig
    AgentSkillLock:
      properties:
        name:
          type: string
          title: Name
        source:
          type: string
          format: path
          title: Source
        digest:
          type: string
          title: Digest
        git_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Git Url
        git_commit_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Git Commit Id
      type: object
      required:
        - name
        - source
        - digest
      title: AgentSkillLock
    EnvironmentConfig-Output:
      properties:
        type:
          anyOf:
            - $ref: '#/components/schemas/EnvironmentType'
            - type: 'null'
        import_path:
          anyOf:
            - type: string
            - type: 'null'
          title: Import Path
        force_build:
          type: boolean
          title: Force Build
          default: false
        delete:
          type: boolean
          title: Delete
          default: true
        stream:
          type: boolean
          title: Stream
          default: false
        cpu_enforcement_policy:
          $ref: '#/components/schemas/ResourceMode'
          default: auto
        memory_enforcement_policy:
          $ref: '#/components/schemas/ResourceMode'
          default: auto
        override_cpus:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Override Cpus
        override_memory_mb:
          anyOf:
            - type: integer
            - type: 'null'
          title: Override Memory Mb
        override_storage_mb:
          anyOf:
            - type: integer
            - type: 'null'
          title: Override Storage Mb
        override_gpus:
          anyOf:
            - type: integer
            - type: 'null'
          title: Override Gpus
        override_tpu:
          anyOf:
            - $ref: '#/components/schemas/TpuSpec'
            - type: 'null'
        mounts:
          anyOf:
            - items:
                $ref: '#/components/schemas/ServiceVolumeConfig'
              type: array
            - type: 'null'
          title: Mounts
          description: Additional mounts for the agent environment only.
        extra_docker_compose:
          items:
            type: string
            format: path
          type: array
          title: Extra Docker Compose
          description: Additional Docker Compose overlays for the agent environment only.
        env:
          additionalProperties:
            type: string
          type: object
          title: Env
        kwargs:
          additionalProperties: true
          type: object
          title: Kwargs
        extra_allowed_hosts:
          items:
            type: string
          type: array
          title: Extra Allowed Hosts
          description: >-
            Run-specific hostnames or IP addresses merged into the [environment]
            network baseline at agent env start.
      type: object
      title: EnvironmentConfig
    ExtraDockerComposeLock:
      properties:
        path:
          type: string
          format: path
          title: Path
        digest:
          type: string
          title: Digest
      type: object
      required:
        - path
        - digest
      title: ExtraDockerComposeLock
    VerifierLock:
      properties:
        override_timeout_sec:
          anyOf:
            - type: number
            - type: 'null'
          title: Override Timeout Sec
        max_timeout_sec:
          anyOf:
            - type: number
            - type: 'null'
          title: Max Timeout Sec
        include_logs:
          items:
            type: string
          type: array
          title: Include Logs
          description: >-
            Glob patterns of verifier log files to download, relative to the
            verifier logs directory. When set, only matching files are kept; the
            reward file is always downloaded.
        exclude_logs:
          items:
            type: string
          type: array
          title: Exclude Logs
          description: >-
            Glob patterns of verifier log files to skip when downloading.
            Applied after include_logs, so exclude wins on overlap.
        env:
          additionalProperties:
            type: string
          type: object
          title: Env
        import_path:
          anyOf:
            - type: string
            - type: 'null'
          title: Import Path
        kwargs:
          additionalProperties: true
          type: object
          title: Kwargs
        disable:
          type: boolean
          title: Disable
          default: false
        extra_allowed_hosts:
          items:
            type: string
          type: array
          title: Extra Allowed Hosts
          description: >-
            Run-specific hostnames or IP addresses merged into the effective
            verifier phase allowlist during verify(), and into the verifier
            environment baseline when the verifier runs separately.
        environment_mode:
          anyOf:
            - $ref: '#/components/schemas/VerifierEnvironmentMode'
            - type: 'null'
      type: object
      title: VerifierLock
      description: |-
        The trial's verifier config plus the resolved environment mode.

        ``environment_mode`` is None for multi-step tasks, where the mode is
        resolved per step.
    ArtifactConfig:
      properties:
        source:
          type: string
          title: Source
        destination:
          anyOf:
            - type: string
            - type: 'null'
          title: Destination
        exclude:
          items:
            type: string
          type: array
          title: Exclude
          description: >-
            Patterns to exclude when downloading a directory artifact (passed as
            tar --exclude flags).
        service:
          anyOf:
            - type: string
            - type: 'null'
          title: Service
          description: >-
            Docker Compose service to collect this artifact from. None or 'main'
            targets the agent's container. Any other value requires a
            compose-capable environment provider and an absolute source path.
      type: object
      required:
        - source
      title: ArtifactConfig
    ErrorPolicyOverride:
      properties:
        should_retry:
          anyOf:
            - type: boolean
            - type: 'null'
          title: Should Retry
        should_grade:
          anyOf:
            - type: boolean
            - type: 'null'
          title: Should Grade
        has_valid_reward:
          anyOf:
            - type: boolean
            - type: 'null'
          title: Has Valid Reward
      additionalProperties: false
      type: object
      title: ErrorPolicyOverride
    SourceTrialLock:
      properties:
        action:
          type: string
          const: regrade
          title: Action
        type:
          type: string
          enum:
            - local
            - hub
          title: Type
        trial_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Trial Id
        path:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: Path
        task:
          anyOf:
            - $ref: '#/components/schemas/TaskLock'
            - type: 'null'
      type: object
      required:
        - action
        - type
      title: SourceTrialLock
      description: |-
        Resolved source: the recorded trial this trial derives from.

        The same shape as the config's ``source_trial`` plus ``task``: the
        source trial's own task lock, copied verbatim from its lock.json (None
        when the source has none). Resolution also fills in ``trial_id`` for
        local sources that did not record it. Equality is content-anchored:
        the trial UUID (path only as a fallback for id-less sources) and the
        source task digest. ``path`` is otherwise recorded for humans and
        resume, not compared.
    ErrorPolicy:
      properties:
        should_retry:
          type: boolean
          title: Should Retry
        should_grade:
          type: boolean
          title: Should Grade
        has_valid_reward:
          type: boolean
          title: Has Valid Reward
      type: object
      required:
        - should_retry
        - should_grade
        - has_valid_reward
      title: ErrorPolicy
    LocalTaskId:
      properties:
        path:
          type: string
          format: path
          title: Path
      type: object
      required:
        - path
      title: LocalTaskId
    GitTaskId:
      properties:
        git_url:
          type: string
          title: Git Url
        git_commit_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Git Commit Id
        path:
          type: string
          format: path
          title: Path
      type: object
      required:
        - git_url
        - path
      title: GitTaskId
    PackageTaskId:
      properties:
        org:
          type: string
          title: Org
        name:
          type: string
          title: Name
        ref:
          anyOf:
            - type: string
            - type: 'null'
          title: Ref
      type: object
      required:
        - org
        - name
      title: PackageTaskId
    TrialConfig:
      properties:
        task:
          $ref: '#/components/schemas/TaskConfig'
        trial_name:
          type: string
          title: Trial Name
          default: ''
        trials_dir:
          type: string
          format: path
          title: Trials Dir
          default: trials
        install_only:
          type: boolean
          title: Install Only
          description: >-
            Only run agent setup/install, then exit (skips agent run +
            verification).
          default: false
        timeout_multiplier:
          type: number
          title: Timeout Multiplier
          default: 1
        agent_timeout_multiplier:
          anyOf:
            - type: number
            - type: 'null'
          title: Agent Timeout Multiplier
        verifier_timeout_multiplier:
          anyOf:
            - type: number
            - type: 'null'
          title: Verifier Timeout Multiplier
        agent_setup_timeout_multiplier:
          anyOf:
            - type: number
            - type: 'null'
          title: Agent Setup Timeout Multiplier
        environment_build_timeout_multiplier:
          anyOf:
            - type: number
            - type: 'null'
          title: Environment Build Timeout Multiplier
        agent:
          $ref: '#/components/schemas/AgentConfig'
        user_agent:
          anyOf:
            - $ref: '#/components/schemas/UserAgentConfig'
            - type: 'null'
          description: >-
            Optional simulated-user agent that communicates with the primary
            agent through its configured bridge.
        environment:
          $ref: '#/components/schemas/EnvironmentConfig-Output'
        verifier:
          $ref: '#/components/schemas/VerifierConfig'
        override_error_policy:
          additionalProperties:
            $ref: '#/components/schemas/ErrorPolicyOverride'
          type: object
          title: Override Error Policy
        artifacts:
          items:
            anyOf:
              - type: string
              - $ref: '#/components/schemas/ArtifactConfig'
          type: array
          title: Artifacts
        extra_instruction_paths:
          items:
            type: string
            format: path
          type: array
          title: Extra Instruction Paths
          description: >-
            Paths to extra instruction files appended to the task instruction.
            File contents are appended before ``extra_instructions``.
        extra_instructions:
          items:
            type: string
          type: array
          title: Extra Instructions
          description: >-
            Inline extra instructions appended to the task instruction. Appended
            after any ``extra_instruction_paths`` contents.
        job_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Job Id
        source_trial:
          anyOf:
            - $ref: '#/components/schemas/SourceTrialConfig'
            - type: 'null'
          description: >-
            Source trial this trial derives from. When set with
            action='regrade', the trial skips the agent phase: it seeds its
            agent logs and artifacts from the source trial, then re-runs
            verification against them in a separate verifier environment. The
            source trial is never modified.
      type: object
      required:
        - task
      title: TrialConfig
    AgentInfo:
      properties:
        name:
          type: string
          title: Name
        version:
          type: string
          title: Version
        model_info:
          anyOf:
            - $ref: '#/components/schemas/ModelInfo'
            - type: 'null'
      type: object
      required:
        - name
        - version
      title: AgentInfo
      description: Information about an agent that participated in a trial.
    AgentContext:
      properties:
        n_input_tokens:
          anyOf:
            - type: integer
            - type: 'null'
          title: N Input Tokens
          description: The number of input tokens used including cache.
        n_cache_tokens:
          anyOf:
            - type: integer
            - type: 'null'
          title: N Cache Tokens
          description: The number of cache tokens used.
        n_output_tokens:
          anyOf:
            - type: integer
            - type: 'null'
          title: N Output Tokens
          description: The number of output tokens used.
        cost_usd:
          anyOf:
            - type: number
            - type: 'null'
          title: Cost Usd
          description: The cost in USD for the agent execution.
        model_usage:
          anyOf:
            - additionalProperties:
                $ref: '#/components/schemas/ModelUsage'
              type: object
            - type: 'null'
          title: Model Usage
          description: >-
            Token usage and cost broken down by model name as reported by the
            agent, including subagents. Backfilled from the ATIF trajectory when
            the agent does not populate it.
        rollout_details:
          anyOf:
            - items:
                $ref: >-
                  #/components/schemas/harbor__models__agent__rollout_detail__RolloutDetail
              type: array
            - type: 'null'
          title: Rollout Details
          description: >-
            Detailed information about each rollout trajectory including token
            IDs, loss masks, and logprobs. Each element represents one
            trajectory. For a linear chat history, there is only one rollout
            trajectory.
        metadata:
          anyOf:
            - additionalProperties: true
              type: object
            - type: 'null'
          title: Metadata
          description: Additional metadata about the agent execution.
      type: object
      title: AgentContext
    VerifierResult:
      properties:
        rewards:
          anyOf:
            - additionalProperties:
                anyOf:
                  - type: number
                  - type: integer
              type: object
            - type: 'null'
          title: Rewards
      type: object
      title: VerifierResult
    VerifierEnvironmentMode:
      type: string
      enum:
        - shared
        - separate
      title: VerifierEnvironmentMode
      description: Whether the verifier runs in the agent's environment or its own.
    TimingInfo:
      properties:
        started_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Started At
        finished_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Finished At
      type: object
      title: TimingInfo
      description: Timing information for a phase of trial execution.
    StepResult:
      properties:
        step_name:
          type: string
          title: Step Name
        agent_result:
          anyOf:
            - $ref: '#/components/schemas/AgentContext'
            - type: 'null'
        verifier_result:
          anyOf:
            - $ref: '#/components/schemas/VerifierResult'
            - type: 'null'
        exception_info:
          anyOf:
            - $ref: '#/components/schemas/ExceptionInfo'
            - type: 'null'
        agent_execution:
          anyOf:
            - $ref: '#/components/schemas/TimingInfo'
            - type: 'null'
        verifier:
          anyOf:
            - $ref: '#/components/schemas/TimingInfo'
            - type: 'null'
      type: object
      required:
        - step_name
      title: StepResult
    ModelApiFormat:
      type: string
      enum:
        - openai_chat_completions
        - openai_responses
        - anthropic_messages
        - google_generate_content
        - bedrock_converse
      title: ModelApiFormat
      description: Canonical inference protocols; agents declare their supported overrides.
    MCPServerConfig:
      properties:
        name:
          type: string
          title: Name
        transport:
          type: string
          enum:
            - stdio
            - sse
            - streamable-http
          title: Transport
          default: sse
        url:
          anyOf:
            - type: string
            - type: 'null'
          title: Url
        command:
          anyOf:
            - type: string
            - type: 'null'
          title: Command
        args:
          items:
            type: string
          type: array
          title: Args
        headers:
          additionalProperties:
            type: string
          type: object
          title: Headers
      type: object
      required:
        - name
      title: MCPServerConfig
      description: Configuration for an MCP server available to the agent.
    BridgeConfig:
      properties:
        kind:
          $ref: '#/components/schemas/BridgeKind'
        prompt_path:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: Prompt Path
        kwargs:
          additionalProperties: true
          type: object
          title: Kwargs
      type: object
      required:
        - kind
      title: BridgeConfig
    EnvironmentType:
      type: string
      enum:
        - docker
        - sbx
        - podman
        - daytona
        - e2b
        - modal
        - runloop
        - langsmith
        - ec2
        - gke
        - ack
        - openshift
        - novita
        - apple-container
        - singularity
        - islo
        - tensorlake
        - cwsandbox
        - use-computer
        - cua-cloud
        - blaxel
        - opensandbox
        - beam
        - skypilot
        - hf-sandbox
        - hyperbrowser
        - vercel
        - runta
        - prime
        - kata
        - mosaic
        - smol
        - sail
        - kernel
        - snowflake
        - boxd
        - superserve
        - aca-sandbox
      title: EnvironmentType
    ResourceMode:
      type: string
      enum:
        - auto
        - limit
        - request
        - guarantee
        - ignore
      title: ResourceMode
    TpuSpec:
      properties:
        type:
          type: string
          minLength: 1
          title: Type
          description: >-
            TPU accelerator type. Accepts either a user-friendly alias (e.g.,
            'v6e', 'trillium', 'v4') or a canonical GKE label (e.g.,
            'tpu-v6e-slice', 'tpu7x').
        topology:
          type: string
          title: Topology
          description: TPU topology as 'NxM' or 'NxMxK' (e.g., '2x4', '2x2x1').
      type: object
      required:
        - type
        - topology
      title: TpuSpec
      description: |-
        Specification for a TPU slice attached to an environment.

        The (type, topology) pair fully determines the GKE node pool the pod
        lands on *and* the per-pod TPU chip count, so there is no separate
        user-facing chip-count field — it is derived via chip_count.
    ServiceVolumeConfig:
      properties:
        type:
          type: string
          enum:
            - bind
            - volume
            - image
          title: Type
        source:
          type: string
          title: Source
        target:
          type: string
          title: Target
        read_only:
          type: boolean
          const: true
          title: Read Only
        bind:
          $ref: '#/components/schemas/ServiceVolumeBind'
        volume:
          $ref: '#/components/schemas/ServiceVolumeVolume'
        image:
          $ref: '#/components/schemas/ServiceVolumeImage'
      type: object
      required:
        - type
        - source
        - target
      title: ServiceVolumeConfig
    TaskConfig:
      properties:
        path:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: Path
        git_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Git Url
        git_commit_id:
          anyOf:
            - type: string
            - type: 'null'
          title: Git Commit Id
        name:
          anyOf:
            - type: string
            - type: 'null'
          title: Name
        ref:
          anyOf:
            - type: string
            - type: 'null'
          title: Ref
        overwrite:
          type: boolean
          title: Overwrite
          default: false
        download_dir:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: Download Dir
        source:
          anyOf:
            - type: string
            - type: 'null'
          title: Source
      type: object
      title: TaskConfig
    VerifierConfig:
      properties:
        override_timeout_sec:
          anyOf:
            - type: number
            - type: 'null'
          title: Override Timeout Sec
        max_timeout_sec:
          anyOf:
            - type: number
            - type: 'null'
          title: Max Timeout Sec
        include_logs:
          items:
            type: string
          type: array
          title: Include Logs
          description: >-
            Glob patterns of verifier log files to download, relative to the
            verifier logs directory. When set, only matching files are kept; the
            reward file is always downloaded.
        exclude_logs:
          items:
            type: string
          type: array
          title: Exclude Logs
          description: >-
            Glob patterns of verifier log files to skip when downloading.
            Applied after include_logs, so exclude wins on overlap.
        env:
          additionalProperties:
            type: string
          type: object
          title: Env
        import_path:
          anyOf:
            - type: string
            - type: 'null'
          title: Import Path
        kwargs:
          additionalProperties: true
          type: object
          title: Kwargs
        disable:
          type: boolean
          title: Disable
          default: false
        extra_allowed_hosts:
          items:
            type: string
          type: array
          title: Extra Allowed Hosts
          description: >-
            Run-specific hostnames or IP addresses merged into the effective
            verifier phase allowlist during verify(), and into the verifier
            environment baseline when the verifier runs separately.
      type: object
      title: VerifierConfig
    SourceTrialConfig:
      properties:
        action:
          type: string
          const: regrade
          title: Action
        type:
          type: string
          enum:
            - local
            - hub
          title: Type
        trial_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Trial Id
        path:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: Path
      type: object
      required:
        - action
        - type
      title: SourceTrialConfig
      description: |-
        A source trial this trial derives from.

        ``action`` names the derivation and must be stated explicitly; only
        ``regrade`` exists today. A ``hub`` source records only the hub trial
        UUID (the bytes are downloaded or read from the local cache); a
        ``local`` source records the trial directory path plus its UUID when
        known.
    ModelInfo:
      properties:
        name:
          type: string
          title: Name
        provider:
          anyOf:
            - type: string
            - type: 'null'
          title: Provider
      type: object
      required:
        - name
      title: ModelInfo
      description: |-
        Information about a model that participated in a trial.

        ``provider`` is optional: when the user runs e.g. ``-m gpt-5.4`` with no
        ``<provider>/`` prefix, the CLI records the model name without a
        provider. Downstream writes to the ``model`` table omit the column so
        the DB default (``'unknown'``) takes over, keeping both sides honest
        about "not specified" vs "explicitly unknown".
    ModelUsage:
      properties:
        n_input_tokens:
          type: integer
          title: N Input Tokens
          default: 0
        n_cache_tokens:
          type: integer
          title: N Cache Tokens
          default: 0
        n_output_tokens:
          type: integer
          title: N Output Tokens
          default: 0
        cost_usd:
          anyOf:
            - type: number
            - type: 'null'
          title: Cost Usd
      type: object
      title: ModelUsage
      description: >-
        Token usage and cost attributed to a single model.


        Field semantics mirror the totals on ``AgentContext``:
        ``n_input_tokens``

        includes cached tokens.
    harbor__models__agent__rollout_detail__RolloutDetail:
      properties:
        prompt_token_ids:
          items:
            items:
              type: integer
            type: array
          type: array
          title: Prompt Token Ids
        completion_token_ids:
          items:
            items:
              type: integer
            type: array
          type: array
          title: Completion Token Ids
        logprobs:
          items:
            items:
              type: number
            type: array
          type: array
          title: Logprobs
        extra:
          additionalProperties:
            items: {}
            type: array
          type: object
          title: Extra
      type: object
      title: RolloutDetail
      description: >-
        Details about a complete chat history.


        This represents the full conversation sequence, allowing downstream

        applications to recover the entire conversation using token IDs and
        logprobs.


        For agents with subagents, summarization, or other non-linear chat
        histories,

        multiple RolloutDetail objects can be stored in the list to represent

        different segments. By convention, the first RolloutDetail object should
        contain

        the main agent's conversation history.
    BridgeKind:
      type: string
      enum:
        - acp
      title: BridgeKind
    ServiceVolumeBind:
      properties:
        create_host_path:
          type: boolean
          const: false
          title: Create Host Path
        selinux:
          type: string
          enum:
            - z
            - Z
          title: Selinux
      type: object
      title: ServiceVolumeBind
    ServiceVolumeVolume:
      properties:
        subpath:
          type: string
          title: Subpath
      type: object
      title: ServiceVolumeVolume
    ServiceVolumeImage:
      properties:
        subpath:
          type: string
          title: Subpath
      type: object
      title: ServiceVolumeImage
  responses:
    ProblemDetails:
      description: Request failed (RFC 9457).
      content:
        application/problem+json:
          schema:
            additionalProperties: true
            description: >-
              RFC 9457 response, with Hub's error tag and optional extension
              fields.
            properties:
              type:
                const: about:blank
                default: about:blank
                title: Type
                type: string
              title:
                title: Title
                type: string
              status:
                title: Status
                type: integer
              detail:
                title: Detail
                type: string
              _tag:
                title: Tag
                type: string
            required:
              - title
              - status
              - detail
              - _tag
            title: ProblemDetails
            type: object
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      description: Required only when the server has HARBOR_API_KEY configured.

````

This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.