# Install HTML/CSS to Image MCP in Cline

Configure the hosted HTML/CSS to Image MCP server as a remote Streamable HTTP server. Do not clone, build, or run a local server, and do not ask the user for an API key, access token, environment variable, or custom authorization header.

## Connection details

- Server name: `hcti`
- Transport: Streamable HTTP
- URL: `https://mcp.hcti.io`
- Authentication: browser-based OAuth
- Documentation: `https://docs.htmlcsstoimage.com/integrations/mcp/`

## Install in Cline

Preserve every existing MCP server and user setting. Add or update only the `hcti` server.

### Cline CLI

When the `cline` command is available, use its MCP installer so Cline writes the correct settings shape for the installed CLI version:

```bash
cline mcp install hcti --transport http --yes https://mcp.hcti.io
```

The command should report an installed server with a `streamableHttp` transport and no headers or warnings.

### Cline IDE extension

In Cline's MCP Servers panel, add a remote server named `hcti`, use the URL `https://mcp.hcti.io`, and select **Streamable HTTP**. If editing the extension's MCP settings JSON directly, use:

```json
{
  "mcpServers": {
    "hcti": {
      "type": "streamableHttp",
      "url": "https://mcp.hcti.io",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

The Cline extension's transport value must be `streamableHttp` in camelCase. Do not omit `type`, because an untyped remote entry may be interpreted as legacy SSE. Cline CLI may store the same connection in a version-specific nested `transport` object; let its installer manage that shape instead of rewriting it to match the extension example.

After saving the configuration:

1. Connect or restart the `hcti` server in Cline's MCP Servers panel.
2. When Cline reports that authentication is required, start the authorization flow and have the user complete it in their browser.
3. Confirm that HCTI tools are listed for the connected server. `check_usage`, `create_image`, and `create_url_image` are useful tools to look for.
4. Do not create a test image or change an HCTI resource unless the user asks. Rendering consumes account credits, and create, update, and delete tools modify the connected organization.

An HTML/CSS to Image account is required. Users can sign up for free during authorization. Available tools depend on the permissions granted to the OAuth connection.

## Troubleshooting

- If Cline tries to use SSE, confirm that the entry contains `"type": "streamableHttp"`.
- If authentication is pending, use the server's authentication action in the MCP Servers panel and finish the browser flow.
- If the server is still disconnected after authorization, restart only the `hcti` server and check the exact URL.
- Do not store OAuth credentials in this repository or paste them into the MCP configuration.

The plugin files in this repository are MIT licensed. Use of the hosted service is governed separately by the [HTML/CSS to Image Terms of Use](https://htmlcsstoimage.com/terms) and [Privacy Policy](https://htmlcsstoimage.com/privacy).
