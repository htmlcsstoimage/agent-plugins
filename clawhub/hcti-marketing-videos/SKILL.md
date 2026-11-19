---
name: hcti-marketing-videos
description: Create marketing videos and animated GIFs from structured HTML/CSS with HCTI. Use for product launches, feature teasers, animated social ads, branded announcements, and motion graphics with precise text and layout. For recording an existing website, use the website recording workflow instead.
metadata:
  openclaw:
    homepage: https://htmlcsstoimage.com
    requires:
      bins:
        - openclaw
---

# HTML/CSS marketing videos

Turn a brief into a browser-rendered composition and record it with HCTI. Keep text, layout, assets, and timing editable in HTML/CSS. Recordings have no audio; do not promise voiceover, music, or sound effects.

## Connect with OAuth

Requires an HCTI account and available rendering credits. Rendering consumes the user's allowance and may incur service costs under their plan. Use the hosted MCP server at `https://mcp.hcti.io` with Streamable HTTP and browser-based OAuth. No HCTI API key environment variables are needed.

If HCTI tools are already available in the current client, use that connection. Otherwise, when setting up the integration in OpenClaw, inspect existing server entries with `openclaw mcp list`. For a new connection:

```bash
openclaw mcp add hcti --url https://mcp.hcti.io --transport streamable-http --auth oauth
openclaw mcp login hcti
```

Have the user complete browser authorization and choose their HCTI organization. Do not overwrite an existing `hcti` entry pointing elsewhere. For an existing correct connection, run `openclaw mcp login hcti` to authorize it. Check connectivity with `openclaw mcp doctor hcti --probe`, then ensure the current agent session exposes the HCTI tools before rendering. If it does not, reload the MCP connection or start a new session as supported by the client.

Installing this skill supplies instructions; it does not automatically register or authorize the MCP server. Use the client's OAuth credential storage, and never copy access tokens into skill files, shell commands, or chat. If the installed OpenClaw version lacks these MCP commands, consult its supported connection setup or update it before proceeding.

HTML, CSS, JavaScript, recording actions, target URLs, and rendering options are processed by the hosted HCTI service. See [HCTI MCP documentation](https://docs.htmlcsstoimage.com/integrations/mcp/) and [OpenClaw MCP setup](https://docs.openclaw.ai/cli/mcp/registry).

## Choose the composition

- Use the supplied brand, audience, message, assets, destination, and dimensions. Infer reasonable choices from the brief; ask only when a missing detail materially changes the result.
- Build around one main message and a clear ending. Product launches, metric reveals, feature comparisons, and animated typography suit this workflow.
- Use semantic sections for scenes, CSS variables for design tokens, and separate content from animation rules. Keep variable text editable rather than baking it into images.
- Choose a viewport that fits the destination: landscape for demos, square for feed graphics, or portrait for vertical placements. Set both viewport dimensions; recordings capture the viewport rather than automatically cropping to an element.
- Prefer MP4 or WebM for video. Choose GIF when the requested placement needs an animated image, keeping its dimensions and duration economical.

For a concrete starting point, read [references/composition.md](references/composition.md). Adapt the design and timing to the brief rather than repeating the example's styling.

## Make timing predictable

Keep animations stopped during page preparation. Start them with the first recorded `execute_javascript` action, for example by adding a `recording` class to the document body. Gate CSS animations on that class, or use it to start a controlled JavaScript timeline.

Use `duration_mode: fixed` for a composition with a planned runtime. It keeps capture running for the requested duration after the start action completes, so no trailing `stall` action is needed. Make the entire sequence fit that runtime and leave readable holds for the opening, key claim, and final call to action.

Use an `element_visible` trigger when a real page element establishes readiness. Use `function_call` only when the page actually calls `window.RecordingReady()`. Neither replaces starting the visual timeline at capture time. Honor font and asset readiness before revealing dependent content.

Use transforms and opacity for motion where possible. Check overflow, long copy, contrast, and safe margins at the intended output size. Keep the final scene visible through the end; for a requested loop, design the boundary deliberately.

## Create the recording

Tool names may have client prefixes. Select by suffix and inspect the active tool schema. Read the server's `hcti://docs/recording-authoring` resource when available for current settings and limits.

Call `create_htmlcss_recording` with a top-level `content` object containing the HTML, CSS, requested duration, viewport, actions, and output settings. Supply actual objects and arrays, not JSON-encoded strings. Leave optional settings omitted or null to use API defaults unless the composition requires a particular value.

`device_scale` changes capture resolution without changing layout; `output.scale` resizes the resulting media. Do not shrink the layout viewport simply to make a smaller file if that would change line wrapping or responsive behavior.

Creation returns `item.id`, `item.status_url`, and `item.recording_url` before capture finishes. Preserve those values. Use `get_recording_status` with the returned ID to check progress, spacing checks a few seconds apart; never create another recording just to poll.

- `processing` or `pending_retry`: keep the existing recording; an error from an earlier attempt does not necessarily mean terminal failure.
- `awaiting_render`: deferred rendering has not started. Use `store_recording` when starting it is within the user's requested workflow; status checks alone do not start it.
- `ready`: the recording succeeded.
- `failed` or `cancelled`: stop and explain the returned error, including `action_path` when relevant. Correct a clear cause before attempting another render.

Keep polling bounded to the active task; if it must stop before completion, report the ID, URL, and current status honestly. If HCTI storage is disabled, use the configured customer storage destination rather than promising that the HCTI media URL will serve the file.

## Review and deliver

Inspect the video or representative frames using available media tools before claiming visual verification. Check the opening, scene transitions, text readability, clipping, asset loading, and ending. Sample extra frames around transitions; a few still frames do not establish that every moment is correct. If media inspection is unavailable, share the result for review and state that limitation.

Return the recording URL, format, dimensions, and intended runtime. Present the video or animation inline when the host supports it, and preserve a clickable URL. Provide the editable HTML/CSS when requested or useful for the user's implementation.

An explicit request authorizes the requested recording or stated variation count. Creating variants or rerendering can consume additional credits; keep iteration tied to concrete defects or the requested scope. Do not create or update saved templates, storage destinations, or other account resources unless requested. On authorization or account-limit errors, explain the service response instead of repeatedly retrying.
