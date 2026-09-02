# HTML/CSS to Image API for Cursor

Let Cursor agents capture live website screenshots, render HTML/CSS, and populate reusable templates as images or PDFs—without managing a browser.

The plugin contains one focused Agent Skill and connects Cursor to the hosted HCTI MCP server at `https://mcp.hcti.io`. Authentication uses HCTI's browser-based OAuth flow; no API key or access token is stored in this repository.

## Install from the Cursor Marketplace

After the plugin is published, open **Customize** in Cursor, search for **HTML/CSS to Image API**, select **Install**, and choose the desired scope.

Open Cursor's MCP settings, find `hcti`, select **Connect**, and complete authorization in your browser. Start a new Agent chat after connecting so the HCTI tools are available.

## Test a local checkout

Copy this repository into Cursor's local plugin directory:

```bash
mkdir -p ~/.cursor/plugins/local
cp -R /path/to/cursor-plugin ~/.cursor/plugins/local/html-css-to-image
```

Restart Cursor or run **Developer: Reload Window**, then confirm that **HTML/CSS to Image API** appears in **Customize**. Connect the `hcti` MCP server from Cursor's MCP settings before testing an image request.

For another test cycle, replace the copied `html-css-to-image` directory with the current checkout and reload Cursor.

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

## Validate before submission

```bash
node scripts/validate-template.mjs
```

Then test the plugin locally, push this repository publicly, and submit its repository URL at [Cursor Marketplace publishing](https://cursor.com/marketplace/publish). Cursor reviews marketplace plugins and their updates before listing them.

## Documentation and support

- [HTML/CSS to Image MCP documentation](https://docs.htmlcsstoimage.com/integrations/mcp/)
- [Cursor plugin documentation](https://cursor.com/docs/plugins)
- [Cursor plugin reference](https://cursor.com/docs/reference/plugins)
- Support: [support@htmlcsstoimage.com](mailto:support@htmlcsstoimage.com)
