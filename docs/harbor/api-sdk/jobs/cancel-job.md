> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Cancel Job

> Cancel selected jobs. Set force=false to let running rollouts finish.



## OpenAPI

````yaml /api-sdk/openapi.json post /jobs/cancel
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
  /jobs/cancel:
    post:
      tags:
        - Jobs
      summary: Cancel Job
      description: Cancel selected jobs. Set force=false to let running rollouts finish.
      operationId: cancel_job_jobs_cancel_post
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
              $ref: '#/components/schemas/CancelJobsRequest'
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ActionResponse_JobDetail_'
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.jobs.cancel("JOB_ID", force=False)
            print(result)
            client.close()
components:
  schemas:
    CancelJobsRequest:
      properties:
        force:
          type: boolean
          title: Force
          default: true
        job_ids:
          items:
            type: string
            format: uuid
          type: array
          minItems: 1
          title: Job Ids
      additionalProperties: false
      type: object
      required:
        - job_ids
      title: CancelJobsRequest
    ActionResponse_JobDetail_:
      properties:
        items:
          items:
            $ref: '#/components/schemas/JobDetail'
          type: array
          title: Items
      type: object
      required:
        - items
      title: ActionResponse[JobDetail]
    JobDetail:
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
        rollouts:
          $ref: '#/components/schemas/RolloutCounts'
        n_retries:
          type: integer
          minimum: 0
          title: N Retries
          default: 0
        n_errors:
          type: integer
          minimum: 0
          title: N Errors
          default: 0
        agent_names:
          items:
            type: string
          type: array
          title: Agent Names
        model_names:
          items:
            type: string
          type: array
          title: Model Names
        model_providers:
          items:
            type: string
          type: array
          title: Model Providers
        reasoning_efforts:
          items:
            type: string
          type: array
          title: Reasoning Efforts
        sources:
          items:
            type: string
          type: array
          title: Sources
        rewards:
          additionalProperties:
            $ref: '#/components/schemas/MeanAggregate'
          type: object
          title: Rewards
        usage:
          $ref: '#/components/schemas/UsageAggregates'
        lock_uri:
          type: string
          title: Lock Uri
        config:
          $ref: '#/components/schemas/JobConfig-Output'
        session_id:
          anyOf:
            - type: string
              maxLength: 200
              minLength: 1
              pattern: ^[A-Za-z0-9_-]+$
            - type: 'null'
          title: Session Id
      type: object
      required:
        - created_at
        - updated_at
        - started_at
        - finished_at
        - id
        - name
        - state
        - rollouts
        - n_retries
        - n_errors
        - agent_names
        - model_names
        - model_providers
        - reasoning_efforts
        - sources
        - rewards
        - usage
        - lock_uri
        - config
        - session_id
      title: JobDetail
    RolloutCounts:
      properties:
        n_total:
          type: integer
          minimum: 0
          title: N Total
        n_queued:
          type: integer
          minimum: 0
          title: N Queued
          default: 0
        n_running:
          type: integer
          minimum: 0
          title: N Running
          default: 0
        n_paused:
          type: integer
          minimum: 0
          title: N Paused
          default: 0
        n_cancelling:
          type: integer
          minimum: 0
          title: N Cancelling
          default: 0
        n_cancelled:
          type: integer
          minimum: 0
          title: N Cancelled
          default: 0
        n_finished:
          type: integer
          minimum: 0
          title: N Finished
          default: 0
      type: object
      required:
        - n_total
        - n_queued
        - n_running
        - n_paused
        - n_cancelling
        - n_cancelled
        - n_finished
      title: RolloutCounts
    MeanAggregate:
      properties:
        mean:
          anyOf:
            - type: number
            - type: 'null'
          title: Mean
        count:
          type: integer
          minimum: 0
          title: Count
          default: 0
      type: object
      required:
        - mean
        - count
      title: MeanAggregate
    UsageAggregates:
      properties:
        n_input_tokens:
          $ref: '#/components/schemas/SumAggregate'
        n_cached_input_tokens:
          $ref: '#/components/schemas/SumAggregate'
        n_output_tokens:
          $ref: '#/components/schemas/SumAggregate'
        cost_usd:
          $ref: '#/components/schemas/SumAggregate'
      type: object
      required:
        - n_input_tokens
        - n_cached_input_tokens
        - n_output_tokens
        - cost_usd
      title: UsageAggregates
    JobConfig-Output:
      properties:
        job_name:
          type: string
          title: Job Name
        jobs_dir:
          type: string
          format: path
          title: Jobs Dir
          default: jobs
        n_attempts:
          type: integer
          minimum: 1
          title: N Attempts
          default: 1
        install_only:
          type: boolean
          title: Install Only
          description: >-
            Only run agent setup/install, then exit (skips agent run +
            verification).
          default: false
        skip_credential_checks:
          type: boolean
          title: Skip Credential Checks
          description: >-
            Defer agent credential checks to runtime; other preflight checks
            still run.
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
        debug:
          type: boolean
          title: Debug
          description: Enable debug logging
          default: false
        n_concurrent_trials:
          type: integer
          minimum: 1
          title: N Concurrent Trials
          description: >-
            Maximum concurrent trials to run. Agent n_concurrent is a per-agent
            sub-limit under this value.
          default: 4
        quiet:
          type: boolean
          title: Quiet
          description: Suppress trial progress displays
          default: false
        retry:
          $ref: '#/components/schemas/RetryConfig'
        override_error_policy:
          additionalProperties:
            $ref: '#/components/schemas/ErrorPolicyOverride'
          type: object
          title: Override Error Policy
        environment:
          $ref: '#/components/schemas/EnvironmentConfig-Output'
        verifier:
          $ref: '#/components/schemas/VerifierConfig'
        metrics:
          items:
            $ref: '#/components/schemas/MetricConfig'
          type: array
          title: Metrics
        agents:
          items:
            $ref: '#/components/schemas/AgentConfig'
          type: array
          title: Agents
        user_agent:
          anyOf:
            - $ref: '#/components/schemas/UserAgentConfig'
            - type: 'null'
          description: Optional simulated-user agent and bridge applied to every trial.
        datasets:
          items:
            $ref: '#/components/schemas/DatasetConfig'
          type: array
          title: Datasets
        tasks:
          items:
            $ref: '#/components/schemas/TaskConfig'
          type: array
          title: Tasks
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
            Paths to extra instruction files appended to each task instruction.
            File contents are appended before ``extra_instructions``.
        extra_instructions:
          items:
            type: string
          type: array
          title: Extra Instructions
          description: >-
            Inline extra instructions appended to each task instruction.
            Appended after any ``extra_instruction_paths`` contents.
        source_jobs:
          items:
            $ref: '#/components/schemas/SourceJobConfig'
          type: array
          title: Source Jobs
          description: >-
            Source jobs this job derives from. 'regrade' creates one regrade
            trial per matching source trial. 'diff' compares the normal target
            expansion against the sources and creates only required regrades and
            reruns. All source jobs must use the same action.
      type: object
      title: JobConfig
    SumAggregate:
      properties:
        sum:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Sum
        count:
          type: integer
          minimum: 0
          title: Count
          default: 0
      type: object
      required:
        - sum
        - count
      title: SumAggregate
    RetryConfig:
      properties:
        max_retries:
          type: integer
          minimum: 0
          title: Max Retries
          description: Maximum number of retry attempts
          default: 0
        include_exceptions:
          anyOf:
            - items:
                type: string
              type: array
              uniqueItems: true
            - type: 'null'
          title: Include Exceptions
          description: Only retry these exception types, subject to exclude_exceptions.
        exclude_exceptions:
          anyOf:
            - items:
                type: string
              type: array
              uniqueItems: true
            - type: 'null'
          title: Exclude Exceptions
          description: >-
            Exception types to NOT retry on. Takes precedence over
            include_exceptions.
        wait_multiplier:
          type: number
          title: Wait Multiplier
          description: Multiplier for exponential backoff wait time
          default: 1
        min_wait_sec:
          type: number
          title: Min Wait Sec
          description: Minimum wait time in seconds between retries
          default: 1
        max_wait_sec:
          type: number
          title: Max Wait Sec
          description: Maximum wait time in seconds between retries
          default: 60
      type: object
      title: RetryConfig
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
    MetricConfig:
      properties:
        type:
          $ref: '#/components/schemas/MetricType'
          default: mean
        kwargs:
          additionalProperties: true
          type: object
          title: Kwargs
      type: object
      title: MetricConfig
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
    DatasetConfig:
      properties:
        path:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: Path
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
        ref:
          anyOf:
            - type: string
            - type: 'null'
          title: Ref
        registry_url:
          anyOf:
            - type: string
            - type: 'null'
          title: Registry Url
        registry_path:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: Registry Path
        repo:
          anyOf:
            - type: string
            - type: 'null'
          title: Repo
        overwrite:
          type: boolean
          title: Overwrite
          description: Whether to overwrite cached remote tasks.
          default: false
        download_dir:
          anyOf:
            - type: string
              format: path
            - type: 'null'
          title: Download Dir
          description: The directory to cache remote tasks to.
        task_names:
          anyOf:
            - items:
                type: string
              type: array
            - type: 'null'
          title: Task Names
          description: Tasks to include from the dataset. Name can be a glob pattern.
        exclude_task_names:
          anyOf:
            - items:
                type: string
              type: array
            - type: 'null'
          title: Exclude Task Names
          description: Tasks to exclude from the dataset. Name can be a glob pattern.
        n_tasks:
          anyOf:
            - type: integer
            - type: 'null'
          title: N Tasks
          description: >-
            Maximum number of tasks to include from this dataset. Applied after
            task_names/exclude_task_names filtering.
      type: object
      title: DatasetConfig
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
    SourceJobConfig:
      properties:
        action:
          type: string
          enum:
            - regrade
            - diff
          title: Action
        type:
          type: string
          enum:
            - local
            - hub
          title: Type
        job_id:
          anyOf:
            - type: string
              format: uuid
            - type: 'null'
          title: Job Id
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
      title: SourceJobConfig
      description: >-
        A source job this job derives from.


        ``action`` names the derivation and must be stated explicitly.
        ``regrade``

        derives one regrade trial per matching source trial. ``diff`` compares
        the

        source against the target job and derives only the required regrades and

        reruns. A ``hub`` source records only the hub job UUID; a ``local``
        source

        records the job directory path plus its UUID when known.
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
    MetricType:
      type: string
      enum:
        - sum
        - min
        - max
        - mean
        - uv-script
      title: MetricType
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
    BridgeKind:
      type: string
      enum:
        - acp
      title: BridgeKind
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