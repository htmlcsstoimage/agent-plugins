# HTML/CSS to Image agent plugins

Let AI agents capture live website screenshots, render HTML/CSS, and populate reusable templates as images or PDFs—without managing a browser.

This repository is a portable [Agent Plugin](https://agent-plugins.org/) package with client-specific manifests for the hosted HCTI MCP server at `https://mcp.hcti.io`. Authentication uses HCTI's browser-based OAuth flow; no API key or access token is stored in this repository.

## Repository layout

| Path | Purpose |
| --- | --- |
| `plugin.json` | Portable Agent Plugins manifest. |
| `mcp.json` | Portable MCP configuration. |
| `.mcp.json` | Grok-compatible MCP configuration. |
| `.cursor-plugin/plugin.json` | Cursor Marketplace manifest. |
| `.grok-plugin/plugin.json` | Grok Build marketplace manifest. |
| `skills/hcti-image-generation/` | Shared HCTI workflow and safety guidance. |

## Cursor

### Install from the Cursor Marketplace

After the plugin is published, open **Customize** in Cursor, search for **HTML/CSS to Image API**, select **Install**, and choose the desired scope.

Open Cursor's MCP settings, find `hcti`, select **Connect**, and complete authorization in your browser. Start a new Agent chat after connecting so the HCTI tools are available.

### Test a local checkout

Copy this repository into Cursor's local plugin directory:

```bash
mkdir -p ~/.cursor/plugins/local
cp -R /path/to/agent-plugins ~/.cursor/plugins/local/html-css-to-image
```

Restart Cursor or run **Developer: Reload Window**, then confirm that **HTML/CSS to Image API** appears in **Customize**. Connect the `hcti` MCP server from Cursor's MCP settings before testing an image request.

For another test cycle, replace the copied `html-css-to-image` directory with the current checkout and reload Cursor.

## Grok Build

The xAI plugin marketplace points to this repository at a pinned commit. Its Grok manifest reuses the root skill, MCP endpoint, and assets.

After the marketplace entry is available, install **HTML/CSS to Image** in Grok Build, connect the `hcti` MCP server when prompted, and complete authorization in your browser.

## Included capabilities

| Tool | Purpose |
| --- | --- |
| `create_image` | Render HTML and CSS as an image or PDF. |
| `create_url_image` | Capture a public URL or selected page element. |
| `create_templated_image` | Render a saved HCTI template with variable values. |
| `create_batch_images` | Generate multiple image variations in one request. |
| `get_max_batch_size` | Check the account's current batch limit. |
| `create_template` | Save a reusable HTML/CSS template. |
| `update_template` | Update an existing template. |
| `list_templates` | List templates in the authorized account. |
| `list_template_versions` | Inspect a template's version history. |

## Example requests

- "Create a 1200×630 Open Graph image with this title and color palette."
- "Capture the pricing table on `https://example.com/pricing`."
- "List my HCTI templates and render the social-card template with this headline."
- "Create five product-card variations with different accent colors."

## Data handling

Requests sent through the plugin are processed by the hosted HTML/CSS to Image service. HTML, CSS, template values, target public URLs, and rendering options supplied to HCTI are sent to that service to produce the requested output. Do not send cookies, authorization headers, private URLs, or other secrets for webpage capture.

## Validate

Validate the Cursor adapter from this repository:

```bash
node scripts/validate-template.mjs
```

The Grok adapter is validated by the xAI marketplace's catalog and component-index checks against the pinned commit.

## Documentation and support

- [HTML/CSS to Image MCP documentation](https://docs.htmlcsstoimage.com/integrations/mcp/)
- [Cursor plugin documentation](https://cursor.com/docs/plugins)
- [Cursor plugin reference](https://cursor.com/docs/reference/plugins)
- Support: [support@htmlcsstoimage.com](mailto:support@htmlcsstoimage.com)
