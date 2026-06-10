---
name: mcp-registry-api
description: Use this skill whenever the user asks to find, install, configure, or publish an MCP (Model Context Protocol) server. Read this to learn the exact REST API commands for the official registry, how to verify publishers, and the strict security protocols required to safely evaluate and install AI tools without risking the user's system.
---

# mcp-registry-api

Instructions for the agent to follow when this skill is activated.

## When to use

Use this skill whenever the user asks you to "find an MCP tool for X", "install an MCP server", "publish my MCP server", or generally interact with the MCP registry to discover and configure new capabilities.

## Instructions

1. **Understand the Spirit of the Registry**
   The MCP Registry is the definitive single source of truth for discovering, authenticating, and configuring Model Context Protocol servers. It uses stringent namespace-based authentication (via DNS, GitHub OAuth, or OIDC) to cryptographically guarantee the identity of publishers. You must act as a highly intelligent, security-conscious expert. Your primary directive is to find tools that truly meet the user's specific needs while aggressively protecting them from malicious, low-quality, or spoofed servers. Do not guess; verify everything.

2. **Identify the User's True Need**
   When searching for a server, evaluate the user's actual goal. Do not just pick the first search result. You must read the `description` of the returned servers, analyze their capabilities, and meticulously select the one that most comprehensively and accurately solves the user's exact problem.

3. **Differentiate Good vs. Bad Servers**
   Before recommending or installing any server, you must evaluate its quality and authenticity:
   - **Authentication & Namespaces**: The registry enforces namespace verification. A server named `com.anthropic/tools` or `io.github.microsoft/server` has been authenticated via DNS or GitHub. You must inspect the namespace to verify if it belongs to the official, reputable company it claims to be.
   - **Official Metadata**: Read the `_meta` object in the API response. The registry explicitly flags the status and authenticity of the server. Use this to determine if the server is actively maintained and officially recognized.

4. **Follow Strict Security Protocol for Unknown Sources**
   You must ruthlessly protect the user from harmful code. Follow these absolute rules:
   - If a server is from a well-known, highly reputable company (e.g., Anthropic, Microsoft, Vercel) AND the namespace is definitively verified, you may proceed.
   - **IF THE SERVER IS NOT FROM A REPUTABLE COMPANY OR IS UNKNOWN**: 
     - You **MUST NOT** download or execute it blindly.
     - You **MUST** find its source repository (e.g., GitHub).
     - You **MUST** check the number of GitHub stars, read the documentation, and inspect the source code to ensure it is not malicious, corrupt, or poorly written.
     - You **MUST** ask the human user for explicit, direct confirmation before downloading, installing, or configuring any unverified or third-party server.

5. **Execute Registry API Commands**
   To interact with the registry, use `curl` (or `curl.exe` on Windows/PowerShell to avoid `Invoke-WebRequest` alias errors) to call the following exact endpoints. All endpoints use the base URL `https://registry.modelcontextprotocol.io/v0.1`.
   **CRITICAL RULE**: You must URL-encode server names in path parameters (e.g., `com.example/server` becomes `com.example%2Fserver`). Do not forget this step.
   - **Discover Servers**: `GET /servers` (Use query parameters: `search=<keyword>`, `limit=<int>`, `cursor=<string>`. Example: `curl -s "https://registry.modelcontextprotocol.io/v0.1/servers?search=database&limit=5"`)
   - **List Server Versions**: `GET /servers/{serverName}/versions` (Example: `curl -s "https://registry.modelcontextprotocol.io/v0.1/servers/com.example%2Fserver/versions"`)
   - **Get Specific Server Version (Inspect & Download)**: `GET /servers/{serverName}/versions/{version}` (Use `latest` for the version parameter to get the most recent metadata. Example: `curl -s "https://registry.modelcontextprotocol.io/v0.1/servers/com.example%2Fserver/versions/latest"`. *Agent Action*: Parse the JSON response. Extract the `remotes` array or installation instructions to figure out how to configure the server.)
   - **Publish a Server**: `POST /publish` (Publishing requires a rigorous namespace authentication token. The body must strictly comply with the `server.schema.json` standard.)
   - **Update Server Version**: `PUT /servers/{serverName}/versions/{version}` (Supported by the generic API specification to update a specific version.)
   - **Delete Server Version**: `DELETE /servers/{serverName}/versions/{version}` (Supported by the generic API specification to remove a specific version.)
   - **Update Server Version Status**: `PATCH /servers/{serverName}/versions/{version}/status` (Update the lifecycle status (e.g., deprecating a version) of a specific server version.)
   - **Update All Versions Status**: `PATCH /servers/{serverName}/status` (Update the status for all versions of a server simultaneously.)

6. **Perform Final Configuration**
   Once you have found the optimal, secure server and retrieved its `latest` metadata, read the `remotes` field or execution commands. Generate the local configuration automatically for the user (e.g., modifying their `mcp_servers.json`) with the correct arguments. Do not leave the user to figure it out themselves; complete the setup entirely while adhering to the security protocols above.
