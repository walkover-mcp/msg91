<p align="center"><img src="assets/msg91-logo.png" alt="MSG91" width="120"></p>

# MSG91 MCP

Lets AI assistants work with [MSG91](https://msg91.com): find the right MSG91 API for SMS, OTP, WhatsApp, Email, Voice and more, read its parameters, and call it correctly.

This is a remote MCP server (Streamable HTTP) at `https://mcp.msg91.com/mcp`. Nothing to install and no API key for the server itself.

## Tools
| Tool | What it does |
|---|---|
| `search_msg91_docs` | Search the MSG91 documentation with a natural-language question |
| `get_msg91_api_docs` | Browse the MSG91 API docs module by module, down to one endpoint as an OpenAPI file |
| `quickstart-msg91` | How to run any MSG91 API from the terminal |

## Connect from Cline
Install it from the Cline MCP Marketplace, or open **MCP Servers → Configure MCP Servers** and add this to `cline_mcp_settings.json`:
```json
{
  "mcpServers": {
    "msg91": {
      "type": "streamableHttp",
      "url": "https://mcp.msg91.com/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```
Agent-driven install steps are in [llms-install.md](llms-install.md).

## Connect from Claude Code
```bash
claude mcp add --transport http msg91 https://mcp.msg91.com/mcp
```

## Calling the MSG91 API
Run any MSG91 endpoint from your terminal (Node.js 18+):
```bash
npx docstar-cli msg91 <module> <endpoint> --<flag> "<value>"
# e.g.
npx docstar-cli msg91 whatsapp send-message-in-text --integrated_number "91XXXXXXXXXX" \
  --recipient_number "91YYYYYYYYYY" --content_type "text" --text "Hello"
```
The CLI asks for your MSG91 authkey in the terminal and can save it. Do not paste your authkey into an AI chat.

## MCP Registry
[server.json](server.json) is the listing for the official MCP Registry, as `com.msg91/mcp`. The `com.msg91` name is proven by owning the `msg91.com` domain.

One-time setup: create a signing key and publish its public half on `msg91.com` (Git Bash or any shell with OpenSSL):
```bash
openssl genpkey -algorithm Ed25519 -out mcp-registry-key.pem
echo "v=MCPv1; k=ed25519; p=$(openssl pkey -in mcp-registry-key.pem -pubout -outform DER | tail -c 32 | base64)"
```
Add the printed line as a TXT record on `msg91.com`. Keep `mcp-registry-key.pem` private and out of this repo.

Publish (bump `version` first for any later change):
```bash
mcp-publisher login dns --domain msg91.com --private-key "$(openssl pkey -in mcp-registry-key.pem -noout -text | grep -A3 'priv:' | tail -n +2 | tr -d ' :\n')"
mcp-publisher publish
```

## Links
- Website: https://msg91.com
