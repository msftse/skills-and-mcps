# Skills

This directory contains AI agent skills -- reusable instruction sets and reference documentation that teach AI coding assistants how to use specific SDKs, frameworks, and tools.

## Available Skills

| Skill | Description | Language |
|-------|-------------|----------|
| [agent-framework-azure-ai-py](agent-framework-azure-ai-py/SKILL.md) | Build Azure AI Foundry agents with the Microsoft Agent Framework | Python |
| [azure-mgmt-fabric-py](azure-mgmt-fabric-py/SKILL.md) | Manage Microsoft Fabric capacities and resources | Python |
| [drawio-mcp-diagramming](drawio-mcp-diagramming/SKILL.md) | Create and edit architecture diagrams using Draw.io MCP with Azure icon support | MCP |

## Skill Structure

Each skill follows this structure:

```
skill-name/
├── SKILL.md          # Main skill definition (entry point)
└── references/       # Detailed reference documentation
    ├── tools.md
    ├── mcp.md
    └── ...
```

- **SKILL.md** -- The primary file loaded by the AI assistant. Contains architecture overview, installation, core workflows, and quick references.
- **references/** -- Deep-dive docs on specific topics. Referenced from SKILL.md for when the assistant needs more detail.
