# Installing the MSG91 MCP server

These steps are for AI agents (such as Cline) setting up this server for a user.

MSG91 MCP is a remote server. There is nothing to clone, build or run locally: the client connects to `https://mcp.msg91.com/mcp` over Streamable HTTP. The server itself needs no API key.

## 1. Add the server to the MCP settings
Add this entry to the `mcpServers` object in the MCP settings file (for Cline: `cline_mcp_settings.json`). Keep any servers already in the file.

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

The `type` must be `streamableHttp`.

## 2. Verify
Call `search_msg91_docs` with a question such as "how do I send an OTP". It should return passages from the MSG91 docs with their links.

## Calling the MSG91 API
The server helps find the right endpoint and its parameters. To actually call an endpoint, run the MSG91 CLI in the user's own terminal (Node.js 18+, nothing to install):

```bash
npx docstar-cli msg91 <module> <endpoint> --<flag> "<value>"
```

The CLI asks for the user's MSG91 authkey in the terminal (hidden) and can save it. Never ask the user to paste the authkey into the chat. Confirm the recipient and the message content with the user before sending anything.

## Tools
- `search_msg91_docs`: search the MSG91 documentation with a natural-language question.
- `get_msg91_api_docs`: browse the MSG91 API docs module by module, down to one endpoint as an OpenAPI file.
- `quickstart-msg91`: how to run any MSG91 API from the terminal.
