> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Agent Options

> Get the JSON Schema for a built-in agent's user-facing kwargs.



## OpenAPI

````yaml /api-sdk/openapi.json get /agents/{name}/options
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
  /agents/{name}/options:
    get:
      tags:
        - Agents
      summary: Get Agent Options
      description: Get the JSON Schema for a built-in agent's user-facing kwargs.
      operationId: get_agent_options_agents__name__options_get
      parameters:
        - name: name
          in: path
          required: true
          schema:
            type: string
            title: Name
          example: codex
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
                type: object
                additionalProperties: true
                title: Response Get Agent Options Agents  Name  Options Get
              example:
                additionalProperties: false
                properties:
                  base_url:
                    anyOf:
                      - type: string
                      - type: 'null'
                    default: null
                    description: Inference endpoint override (--base-url).
                    title: Base Url
                  disable_web_search:
                    default: false
                    description: Disable native web search and fetch tools when supported.
                    title: Disable Web Search
                    type: boolean
                  reasoning_effort:
                    anyOf:
                      - enum:
                          - none
                          - minimal
                          - low
                          - medium
                          - high
                          - xhigh
                          - max
                        type: string
                      - type: 'null'
                    default: null
                    description: Model reasoning effort.
                    title: Reasoning Effort
                  version:
                    anyOf:
                      - type: string
                      - type: 'null'
                    default: null
                    description: >-
                      Agent CLI version to install. Omit for latest, or to use
                      the version the image already has for agents that check
                      for one.
                    title: Version
                  skip_install:
                    default: false
                    description: >-
                      Skip installing the agent; the environment must already
                      provide it and its dependencies.
                    title: Skip Install
                    type: boolean
                  prompt_template_path:
                    anyOf:
                      - type: string
                      - type: 'null'
                    default: null
                    description: Jinja2 template path; must include {{ instruction }}.
                    title: Prompt Template Path
                  config:
                    default: null
                    description: Native config file path or inline JSON.
                    title: Config
                  reasoning_summary:
                    anyOf:
                      - enum:
                          - auto
                          - concise
                          - detailed
                          - none
                        type: string
                      - type: 'null'
                    default: null
                    description: How model reasoning is summarized.
                    title: Reasoning Summary
                  web_search:
                    anyOf:
                      - enum:
                          - disabled
                          - cached
                          - live
                        type: string
                      - type: 'null'
                    default: null
                    description: Web search mode.
                    title: Web Search
                  dangerously_bypass_hook_trust:
                    default: false
                    description: >-
                      Run enabled Codex hooks without persisted trust; use only
                      with vetted hook sources.
                    title: Dangerously Bypass Hook Trust
                    type: boolean
                title: CodexOptions
                type: object
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.agents.options("codex")
            print(result)
            client.close()
components:
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