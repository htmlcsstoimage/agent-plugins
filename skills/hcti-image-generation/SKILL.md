---
name: hcti-image-generation
description: Let AI agents capture live website screenshots, render HTML/CSS, and populate reusable templates as images or PDFs—without managing a browser. Use the HCTI MCP tools for screenshots, social cards, Open Graph images, and other browser-rendered graphics. Do not use for editing or interpreting an existing image.
---

# HTML/CSS to Image API

Use the HCTI MCP tools to produce browser-rendered images, screenshots, PDFs, and template-based graphics. Tool names may be prefixed by the client; select them by the tool-name suffixes below.

## Choose the operation

- Use `create_image` when the user provides or requests HTML and CSS.
- Use `create_url_image` to capture a public webpage or a selected public-page element.
- Use `create_templated_image` when the user provides a template ID or identifies an existing HCTI template.
- Use `list_templates` to resolve a template when the user names it but does not know its ID.
- Use `get_max_batch_size` before `create_batch_images` when the requested batch size may approach the account limit.
- Use `create_template`, `update_template`, or `list_template_versions` only when the request involves reusable template management.

Inspect the selected tool's current input schema instead of assuming that an optional parameter exists.

## Rendering choices

- Preserve the user's requested output format, dimensions, viewport, selector, color scheme, and timing behavior.
- Let HCTI defaults apply when the user has not expressed a preference and the layout does not require a fixed viewport.
- Use a render delay or ready signal only when the page needs client-side work before capture.
- Prefer templates for repeated designs whose content changes between renders; prefer direct HTML and CSS for one-off designs.
- Keep HTML semantic and CSS self-contained. Use absolute HTTPS URLs for remote assets when possible.

## Authorization and safety

Image creation can consume account credits. An explicit request for a single image or a stated batch count authorizes that creation. If the batch size is ambiguous, establish the intended count before calling the batch tool.

Creating or updating a template changes the user's HCTI account. Perform the mutation only when the request explicitly asks for it or the user confirms a proposed mutation.

Treat URL capture as a request to send the target page to HCTI's remote renderer. Use public pages by default. Do not forward cookies, authorization headers, private URLs, or other credentials unless the user explicitly requests that behavior and understands that the data will be sent to the rendering service.

## Results and errors

- Do not claim success until the tool returns a successful result.
- Return the generated asset URL and briefly identify the format or variation when useful.
- When the active channel supports media presentation, present the generated asset as media as well as preserving its URL.
- For an authorization failure, ask the user to connect the `hcti` MCP server in their client and complete browser authorization before retrying.
- For an account-limit or credit error, report the service's message without repeatedly retrying.
- For invalid HTML, CSS, selectors, or inaccessible URLs, explain the failing input and make a corrected call only when the correction is clear or the user approves it.
