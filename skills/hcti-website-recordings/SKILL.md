---
name: hcti-website-recordings
description: Record scrolling website walkthroughs and visually review public pages with HCTI video recordings and screenshots. Use to inspect sticky headers, clipping, lazy loading, responsive layouts, or transitions while scrolling, and to capture website demos. For authored HTML/CSS marketing animations, use the marketing-video workflow instead.
---

# Website recordings and visual reviews

Use HCTI to capture an existing webpage as video or animated GIF. For a review request, inspect the resulting media and report visible evidence. A successful recording establishes capture success, not website correctness, accessibility conformance, or a passing functional test.

## Set the review scope

Use the supplied URL, target sections, viewport, and behaviors. For a walkthrough, capture the requested journey without adding an audit. For a visual review, identify what should be assessed: header behavior, section transitions, clipping, readability, mobile layout, or loading states. Infer a sensible viewport from the request; record extra sizes only when they are in scope.

HCTI renders remotely. Use a public URL reachable by that service; a local `localhost` development server is not remotely reachable. Use an existing authorized preview URL when available, or explain what is needed to make the page reachable. Do not silently publish a local site or forward private credentials.

Inspect page source or use available browsing tools to identify real selectors when the workflow needs exact sections, containers, or controls. If page inspection is unavailable, use general scrolling rather than inventing selectors.

## Plan the recording

- Use `simple_scroll` for a general walkthrough. Leave scroll distance and easing omitted or null for API defaults unless the brief calls for particular behavior. The duration limit does not guarantee reaching the bottom of a long page.
- Use `scroll` steps for known sections, exact positions, or a scrollable container. Set `container_selector` on the scroll action when appropriate, and supply each step's `duration_ms`.
- Include pauses where the viewer needs to read or where a dynamic state needs time to appear. Keep the sequence within the chosen duration mode and account limits.
- Keep preparation scroll preload enabled for an ordinary polished walkthrough. For a review specifically intended to observe lazy-loading behavior, consider `skip_scroll_preload: true`; otherwise preparation may initialize the content before the recording starts. State which behavior was tested.
- Preserve the actual page appearance for reviews. Do not inject CSS to hide a defect or suppress banners that are part of the requested experience. If a modified capture is requested, label the modification in the result.
- Use clicks only for controls in the requested journey. For a required selector, use an explicit failure branch rather than silently skipping a missing element. Submit forms, make purchases, or trigger account changes only when those specific actions are explicitly authorized.

For scoped capture examples and evidence collection, read [references/review-recipes.md](references/review-recipes.md).

## Create and track the recording

Tool names may have client prefixes. Inspect the active schema and read `hcti://docs/recording-authoring` when available. Call `create_url_recording` with a top-level `content` object containing the URL, duration, both viewport dimensions when fixed sizing is needed, actions, and output settings. Pass objects and arrays as JSON values, not encoded strings.

Creation returns `item.id`, `item.status_url`, and `item.recording_url` before capture completes. Poll `get_recording_status` with that ID a few seconds apart, keeping checks bounded to the active task. Do not create duplicate recordings to check progress.

`processing` and `pending_retry` are ongoing states. For `awaiting_render`, use `store_recording` if starting the deferred capture is part of the request; reading status does not start it. Stop on `ready`, `failed`, or `cancelled`. On failure, report the returned error and `action_path`; a failure may come from the capture environment rather than the website itself. If polling must end early, report the current state rather than calling the media ready.

When HCTI storage is disabled, use the configured customer storage destination for the media rather than promising the HCTI media URL will serve it.

## Inspect and report evidence

For a visual review, inspect the video or sampled frames with available media tools. Include the start, relevant scroll transitions, target sections, and ending. Sample around the time of a suspected issue rather than relying only on evenly spaced frames. Use `create_url_image` for a high-resolution still or a known element when it helps diagnose a visible problem; a separate screenshot does not establish the state of a particular recorded frame.

Record findings with the viewport, approximate timestamp or section, what is visibly wrong, and the supporting frame or screenshot. Separate observed defects from hypotheses and recommendations. Verify that the requested sections are visible before claiming full coverage. An absent problem in a sampled frame is not proof that it never occurs.

If video inspection is unavailable, return the recording and offer the supported still-image evidence. Say clearly that the motion or scrolling behavior was not visually verified. Do not infer DOM semantics, keyboard behavior, backend correctness, or automated regression results from appearance alone.

Return a clickable recording URL, output format, tested viewport, covered sections, findings, and relevant coverage limits. For a capture-only request, a concise result with the media URL is sufficient. Present media inline when supported by the host.

Creating recordings and supplementary screenshots consumes account credits. Stay within the requested captures and variants. Reuse a suitable existing recording when the user provides its ID; use `get_recording` for saved settings and `get_recording_status` for current progress. Delete recordings only when the user requests deletion. Explain permission or account-limit failures without repeatedly retrying.
