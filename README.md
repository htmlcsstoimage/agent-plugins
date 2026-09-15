# HTML/CSS to Image API for AI agents

Let AI agents capture live website screenshots, render HTML/CSS, and populate reusable templates as images or PDFs—without managing a browser.

This repository is a portable [Agent Plugin](https://agent-plugins.org/) package with client-specific manifests for the hosted HCTI MCP server at `https://mcp.hcti.io`. Authentication uses HCTI's browser-based OAuth flow; no API key or access token is stored in this repository.

## Repository layout

| Path | Purpose |
| --- | --- |
| `plugin.json` | Portable Agent Plugins manifest. |
| `mcp.json` | Portable MCP configuration. |
| `.mcp.json` | Claude Code, GitHub Copilot, and Grok-compatible MCP configuration. |
| `llms-install.md` | Agent-readable installation guidance for Cline and other MCP clients. |
| `.codex-plugin/plugin.json` | Native ChatGPT and Codex plugin manifest. |
| `.app.json` | Maps the package to the official published OpenAI plugin. |
| `.claude-plugin/plugin.json` | Claude Code plugin manifest. |
| `.cursor-plugin/plugin.json` | Cursor Marketplace manifest. |
| `.grok-plugin/plugin.json` | Grok Build marketplace manifest. |
| `gemini-extension.json` | Gemini CLI extension manifest. |
| `kimi.plugin.json` | Kimi Code plugin manifest. |
| `skills/hcti-image-generation/` | Image generation and template workflow guidance. |
| `skills/hcti-open-graph-images/` | Dynamic Open Graph image configuration guidance. |

## ChatGPT and Codex

Install the official [HTML/CSS to Image plugin](https://chatgpt.com/plugins/plugin_asdk_app_6a4d168031448191abcd6540497efb7b) from the Plugins Directory in ChatGPT or Codex. This is the preferred OpenAI integration and uses HCTI's registered MCP connection with browser-based OAuth.

The native `.codex-plugin/plugin.json` manifest packages the shared HCTI skill, while `.app.json` maps the package to the published OpenAI plugin.

## Kimi Code

Install the plugin directly from GitHub in Kimi Code:

```text
/plugins install https://github.com/htmlcsstoimage/agent-plugins
/reload
```

Run `/mcp-config login hcti` and complete authorization in your browser. Then use `/mcp` to confirm that the HCTI tools are connected.

To test a local checkout, use `/plugins install /path/to/agent-plugins`. Kimi copies installed plugins into its managed plugin directory, so reinstall after changing the local source.

## Gemini CLI

Install the extension directly from GitHub:

```bash
gemini extensions install https://github.com/htmlcsstoimage/agent-plugins
```

Restart Gemini CLI, run `/extensions list` to confirm the extension is enabled, then run `/mcp auth hcti` and complete authorization in your browser. Use `/mcp list` to confirm the HCTI tools are connected. Gemini CLI automatically discovers the included HCTI Agent Skills.

To test a local checkout during development, run `gemini extensions link /path/to/agent-plugins`, restart Gemini CLI, and inspect it with `/extensions list`.

## GitHub Copilot

### Install a local checkout

Install this repository directly:

```bash
copilot plugin install /path/to/agent-plugins
```

Start Copilot CLI and use `/plugin list`, `/skills list`, and `/mcp` to confirm that the plugin, skill, and HCTI server are available. Connect `hcti` from `/mcp` and complete authorization in your browser.

## Cline

Give Cline the repository's [`llms-install.md`](llms-install.md) to configure the hosted HCTI server, or add it manually as a remote **Streamable HTTP** server at `https://mcp.hcti.io`. Complete the OAuth authorization in your browser; no API key or local server installation is required.

## Claude Code

### Install from the Claude community marketplace

After the plugin is published, add Anthropic's community marketplace and install the plugin:

```text
/plugin marketplace add anthropics/claude-plugins-community
/plugin install html-css-to-image@claude-community
/reload-plugins
```

Run `/mcp`, select `hcti`, and complete authorization in your browser. Claude can invoke the HCTI skill automatically when relevant; it is also available as `/html-css-to-image:hcti-image-generation`.

### Test a local checkout

Validate and load this repository directly:

```bash
claude plugin validate . --strict
claude --plugin-dir .
```

In the test session, run `/mcp` to connect `hcti`, then try an image request. Use `/reload-plugins` after changing the manifest, skill, or MCP configuration.

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

The core rendering and template tools are:

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

## Managing resources with MCP

Beyond generating images, the HCTI MCP server can manage supporting resources in the connected organization:

| Resource | Tools | Purpose |
| --- | --- | --- |
| Usage | `check_usage` | Check the organization's current image usage and limits. |
| Open Graph configurations | `create_og_config`, `update_og_config`, `get_og_config`, `list_og_configs`, `delete_og_config` | Manage dynamic social-preview images for pages on a website. |
| Proxies | `create_proxy`, `update_proxy`, `get_proxy`, `list_proxies`, `delete_proxy` | Manage proxies used when HCTI accesses source websites. |
| Storage destinations | `create_storage_destination`, `update_storage_destination`, `get_storage_destination`, `list_storage_destinations`, `delete_storage_destination`, `get_aws_storage_external_id` | Manage external destinations for generated images and retrieve the external ID needed for AWS setup. |

[Dynamic Open Graph image configurations](https://htmlcsstoimage.com/features/og-images) are an especially useful integration point. An agent can resolve an existing HCTI template, map page metadata into its variables, optionally select a configured proxy or storage destination, and create a persistent configuration for serving cached social images.

Resource tools operate on the HCTI organization selected during OAuth authorization, and available tools depend on the permissions granted to that connection. Listing and retrieval are read-only; create, update, and delete operations change the account. In particular, `update_og_config` replaces the complete configuration, so agents should retrieve the current configuration and preserve unchanged settings before updating it.

## Example requests

- "Create a 1200×630 Open Graph image with this title and color palette."
- "Capture the pricing table on `https://example.com/pricing`."
- "List my HCTI templates and render the social-card template with this headline."
- "Create five product-card variations with different accent colors."
- "Create a dynamic Open Graph configuration for `https://example.com` using my social-card template and map its title and description from page metadata."

## Data handling

Requests sent through the plugin are processed by the hosted HTML/CSS to Image service. HTML, CSS, template values, target URLs, rendering options, and resource configuration supplied to HCTI are sent to that service; persistent resources are stored in the connected HCTI organization. Do not provide cookies, authorization headers, private URLs, or other secrets unless intentionally configuring an authorized rendering workflow and you understand that HCTI will process that data.

## License and service terms

The plugin files in this repository are available under the [MIT License](LICENSE). The license applies to this repository's source code, configuration, and documentation; it does not license the hosted HTML/CSS to Image service. Use of the service is governed separately by the [Terms of Use](https://htmlcsstoimage.com/terms) and [Privacy Policy](https://htmlcsstoimage.com/privacy).

## Validate

Run the repository checks:

```bash
node scripts/validate-template.mjs
```

Validate the Claude adapter with Claude Code's official strict validator:

```bash
claude plugin validate . --strict
```

### Verified compatibility

Tested with Claude Code 2.1.258. The strict plugin validator passes, Claude discovers the shared HCTI skill and MCP server, and the browser-based OAuth flow connects successfully.

The Grok adapter is validated by the xAI marketplace's catalog and component-index checks against the pinned commit.

## HOL Registry

Our AI agent plugins are registered in the [HOL Plugin Registry](https://hol.org/registry/plugins/html-css-to-image%2Fhtml-css-to-image), where you can review installation details, security findings, provenance, and the current trust score.

[![HTML/CSS to Image API trust badge](https://img.shields.io/endpoint?url=https%3A%2F%2Fhol.org%2Fapi%2Fregistry%2Fbadges%2Fplugin%3Fslug%3Dhtml-css-to-image%252Fhtml-css-to-image%26metric%3Dtrust%26style%3Dflat)](https://hol.org/registry/plugins/html-css-to-image%2Fhtml-css-to-image)

## Documentation and support

- [HTML/CSS to Image MCP documentation](https://docs.htmlcsstoimage.com/integrations/mcp/)
- [Official ChatGPT and Codex plugin](https://chatgpt.com/plugins/plugin_asdk_app_6a4d168031448191abcd6540497efb7b)
- [OpenAI plugin packaging documentation](https://developers.openai.com/codex/plugins/build)
- [Kimi Code plugin documentation](https://github.com/MoonshotAI/kimi-code/blob/main/docs/en/customization/plugins.md)
- [Gemini CLI extension documentation](https://geminicli.com/docs/extensions/)
- [GitHub Copilot plugin documentation](https://docs.github.com/en/copilot/concepts/agents/about-plugins)
- [Cline MCP installation instructions](llms-install.md)
- [Cursor plugin documentation](https://cursor.com/docs/plugins)
- [Cursor plugin reference](https://cursor.com/docs/reference/plugins)
- [Claude Code plugin documentation](https://code.claude.com/docs/en/plugins)
- [Claude Code plugin reference](https://code.claude.com/docs/en/plugins-reference)
- Support: [support@htmlcsstoimage.com](mailto:support@htmlcsstoimage.com)
