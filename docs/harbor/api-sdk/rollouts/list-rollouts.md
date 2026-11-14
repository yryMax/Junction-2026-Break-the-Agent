> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# List Rollouts

> List rollouts, optionally within a job, with filters and pagination.



## OpenAPI

````yaml /api-sdk/openapi.json get /rollouts
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
  /rollouts:
    get:
      tags:
        - Rollouts
      summary: List Rollouts
      description: List rollouts, optionally within a job, with filters and pagination.
      operationId: list_rollouts_rollouts_get
      parameters:
        - name: page_size
          in: query
          required: false
          schema:
            type: integer
            minimum: 0
            description: >-
              Number of items; zero uses the default (50). Values above 100 are
              capped.
            default: 50
            title: Page Size
          description: >-
            Number of items; zero uses the default (50). Values above 100 are
            capped.
        - name: page_token
          in: query
          required: false
          schema:
            type: string
            maxLength: 4096
            default: ''
            title: Page Token
        - name: task
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
              minLength: 1
              maxLength: 4096
            maxItems: 100
            description: >-
              Task names across versions, or name@sha256:<full digest> pins.
              Repeated values use OR.
            title: Task
          description: >-
            Task names across versions, or name@sha256:<full digest> pins.
            Repeated values use OR.
        - name: state
          in: query
          required: false
          schema:
            anyOf:
              - enum:
                  - queued
                  - running
                  - paused
                  - cancelling
                  - finished
                  - cancelled
                type: string
              - type: 'null'
            title: State
        - name: name
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                maxLength: 200
              - type: 'null'
            title: Name
        - name: q
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                maxLength: 200
              - type: 'null'
            title: Q
        - name: agent_name
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Agent Name
        - name: agent_version
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Agent Version
        - name: model_name
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Model Name
        - name: model_provider
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Model Provider
        - name: reasoning_effort
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Reasoning Effort
        - name: source
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Source
        - name: created_after
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: date-time
              - type: 'null'
            title: Created After
        - name: created_before
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: date-time
              - type: 'null'
            title: Created Before
        - name: job_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid
              - type: 'null'
            title: Job Id
        - name: sort
          in: query
          required: false
          schema:
            type: string
            maxLength: 100
            description: Sort field; prefix '-' for descending. Nulls sort last.
            enum:
              - id
              - name
              - state
              - created_at
              - updated_at
              - started_at
              - finished_at
              - task_name
              - agent_name
              - agent_version
              - model_name
              - model_provider
              - reasoning_effort
              - source
              - n_input_tokens
              - n_cached_input_tokens
              - n_output_tokens
              - cost_usd
              - '-id'
              - '-name'
              - '-state'
              - '-created_at'
              - '-updated_at'
              - '-started_at'
              - '-finished_at'
              - '-task_name'
              - '-agent_name'
              - '-agent_version'
              - '-model_name'
              - '-model_provider'
              - '-reasoning_effort'
              - '-source'
              - '-n_input_tokens'
              - '-n_cached_input_tokens'
              - '-n_output_tokens'
              - '-cost_usd'
            default: created_at
            title: Sort
          description: Sort field; prefix '-' for descending. Nulls sort last.
        - name: Harbor-API-Version
          in: header
          required: false
          schema:
            type: string
            default: '2026-10-30'
            title: Harbor-Api-Version
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RolloutsResponse'
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.rollouts.list(job_id="JOB_ID", page_size=10)
            print(result)
            client.close()
components:
  schemas:
    RolloutsResponse:
      properties:
        items:
          items:
            $ref: '#/components/schemas/RolloutSummary'
          type: array
          title: Items
        total:
          type: integer
          minimum: 0
          title: Total
          description: Total matching items, before pagination.
        next_page_token:
          type: string
          title: Next Page Token
          default: ''
      type: object
      required:
        - items
        - total
        - next_page_token
      title: RolloutsResponse
    RolloutSummary:
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
      title: RolloutSummary
      description: |-
        One attempt. Recorded rewards are null until finalized and valid.

        Live execution fields can be newer than the last persisted updated_at.
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