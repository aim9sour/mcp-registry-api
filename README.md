# 🛡️ MCP Registry API Skill

> A security-first Master Skill for AI Agents to safely interact with the Official Model Context Protocol (MCP) Registry.

![Security Shield](https://img.shields.io/badge/Security-Strict-red.svg) ![MCP Ready](https://img.shields.io/badge/MCP-Ready-blue.svg) ![Agent Skill](https://img.shields.io/badge/Skill-Agent-brightgreen.svg)

## 🌟 What is this?
**Disclaimer:** *This is an unofficial, community-made skill designed to safely interact with the Official MCP Registry.*

This is a highly specialized **Agent Skill** designed to teach AI agents (like Claude, Gemini, or any custom agent) how to safely and professionally interact with the official MCP Registry (`registry.modelcontextprotocol.io`). 

Instead of letting agents blindly run commands or guess API endpoints, this skill embeds deep registry mechanics, authentication protocols, and **strict security boundaries** to ensure agents only download verified, safe tools for their users.

## 🚀 How to Install (For Agents)
Agents can install this skill globally using the skills CLI:

```bash
npx skills add aim9sour/mcp-registry-api
```

## 🔒 Security First
This skill is built with extreme prejudice against malicious code. It explicitly instructs the agent to:
1. **Verify Namespaces**: Ensure the publisher is a verified entity via DNS/OIDC.
2. **Check Official Metadata**: Rely on registry meta-flags to validate authenticity.
3. **Never Blindly Trust**: If a tool is from an unknown publisher, the agent is hardcoded to halt, inspect the GitHub stars/code, and **demand explicit human confirmation** before proceeding.

## 📚 Features
- **Full API Reference**: Contains all exact REST endpoints for discovering, inspecting, and publishing servers.
- **URL Encoding Handling**: Pre-warns the agent about complex `serverName` encoding (e.g., `com.example%2Fmy-tool`).
- **Contextual Awareness**: Teaches the agent to behave like a smart librarian, analyzing tool descriptions to perfectly match the user's need.

## 🛠️ Usage Example
Once the skill is installed, users can simply tell their agent:
> "Find and install an official MCP server for interacting with PostgreSQL."

The agent will automatically use this skill's knowledge to:
1. Search the official registry safely.
2. Verify the publisher's namespace.
3. Fetch the `latest` remote configuration.
4. Auto-configure the `mcp_servers.json` file for you.

---
*Created with ❤️ by [@aim9sour](https://github.com/aim9sour)*
