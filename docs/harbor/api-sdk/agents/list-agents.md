> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# List Agents

> List built-in agents without loading their optional dependencies.



## OpenAPI

````yaml /api-sdk/openapi.json get /agents
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
  /agents:
    get:
      tags:
        - Agents
      summary: List Agents
      description: List built-in agents without loading their optional dependencies.
      operationId: list_agents_agents_get
      parameters:
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
                $ref: '#/components/schemas/AgentsResponse'
              example:
                agents:
                  - acp
                  - aider
                  - antigravity-cli
                  - antigravity-sdk
                  - claude-code
                  - cline-cli
                  - code-puppy
                  - codex
                  - computer-1
                  - copilot-cli
                  - cortex-code
                  - cursor-cli
                  - deerflow
                  - devin
                  - dspy-rlm
                  - eve
                  - fabric
                  - fx
                  - fx-dev
                  - gemini-cli
                  - goose
                  - grok-build
                  - hermes
                  - junie
                  - kimi-cli
                  - kimi-code
                  - langgraph
                  - mcode
                  - mimo
                  - mini-swe-agent
                  - muse-code
                  - nemo-agent
                  - nop
                  - openclaw
                  - opencode
                  - opencode-v2
                  - openhands
                  - openhands-sdk
                  - oracle
                  - pi
                  - qwen-coder
                  - rovodev-cli
                  - strands
                  - swe-agent
                  - terminus-2
                  - trae-agent
                  - vibe
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.agents.list()
            print(result)
            client.close()
components:
  schemas:
    AgentsResponse:
      properties:
        agents:
          items:
            type: string
          type: array
          title: Agents
      type: object
      required:
        - agents
      title: AgentsResponse
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