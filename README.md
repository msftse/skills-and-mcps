# Skills and MCPs Collection

A curated collection of AI skills and MCP (Model Context Protocol) server configurations for Copilot and AI agent workflows.

## Repository Structure

```
.
├── skills/                          # AI agent skills
│   ├── agent-framework-azure-ai-py/ # Azure AI Foundry agent skill
│   │   ├── SKILL.md                 # Skill definition and usage
│   │   └── references/              # Detailed reference docs
│   │       ├── tools.md             # Hosted tool patterns
│   │       ├── mcp.md               # MCP integration guide
│   │       ├── threads.md           # Thread & conversation management
│   │       ├── advanced.md          # Structured outputs, OpenAPI, etc.
│   │       └── acceptance-criteria.md # Code validation criteria
│   └── azure-mgmt-fabric-py/       # Azure Fabric Management skill
│       ├── SKILL.md                 # Skill definition and usage
│       └── references/
│           └── acceptance-criteria.md # Code validation criteria
├── prds/                            # Product Requirements Documents
│   └── link-in-bio-page-builder/    # Link-in-Bio page builder PRD
│       └── PRD.md
└── README.md
```

## Skills

### [Agent Framework Azure AI (Python)](skills/agent-framework-azure-ai-py/SKILL.md)

Build Azure AI Foundry agents using the Microsoft Agent Framework Python SDK (`agent-framework-azure-ai`). Covers:

- Creating persistent agents with `AzureAIAgentsProvider`
- Hosted tools (code interpreter, file search, web search)
- MCP server integration (hosted and client-managed)
- Conversation thread management
- Streaming responses
- Function tools and structured outputs

**Source:** [microsoft/skills](https://github.com/microsoft/skills/tree/main/.github/plugins/azure-sdk-python/skills/agent-framework-azure-ai-py)

### [Azure Fabric Management (Python)](skills/azure-mgmt-fabric-py/SKILL.md)

Manage Microsoft Fabric capacities and resources programmatically using `azure-mgmt-fabric`. Covers:

- Creating, updating, and deleting Fabric capacities
- Suspending and resuming capacities for cost control
- SKU management (F2 through F2048)
- Name availability checks
- Long-running operation (LRO) patterns
- Async client usage

**Source:** [microsoft/skills](https://github.com/microsoft/skills/tree/main/.github/plugins/azure-sdk-python/skills/azure-mgmt-fabric-py)

## PRDs

### [Link-in-Bio Page Builder](prds/link-in-bio-page-builder/PRD.md)

A self-hosted, multi-user Linktree alternative built with Next.js, Neon Postgres, and Vercel. Features profile editing, theme system, public URLs with SEO, and click analytics.

**Source:** [coleam00/link-in-bio-page-builder](https://github.com/coleam00/link-in-bio-page-builder/blob/main/.claude/PRD.md)

## Contributing

To add a new skill or PRD:

1. **Skills** -- Create a new folder under `skills/` with a `SKILL.md` and optional `references/` directory
2. **PRDs** -- Create a new folder under `prds/` with a `PRD.md`
3. Update this README with a summary and link

## License

Internal use -- Microsoft SE.
