# Scoped website capture and evidence

Inspect the current MCP tool schema before using these examples. Settings here are explicit choices for the examples, not service defaults.

## General scrolling walkthrough

Arguments for `create_url_recording`:

```json
{
  "content": {
    "url": "https://example.com/",
    "viewport_width": 1280,
    "viewport_height": 720,
    "device_scale": 1,
    "duration_ms": 10000,
    "duration_mode": "at_most",
    "actions": [{"type": "simple_scroll"}],
    "output": {"format": "mp4"}
  }
}
```

The scroll can end at the bottom or at the recording deadline. Verify the ending before calling this a complete page review. For long pages, scope the review to known sections or choose a longer supported recording rather than promising full-page coverage from a short clip.

## Known sections and reading pauses

Once the page has been inspected and these selectors are known to exist, an action sequence can include:

```json
[
  {"type": "stall", "duration_ms": 1000},
  {
    "type": "scroll",
    "steps": [{"to": {"type": "selector", "selector": "#pricing", "if_not_found": [{"type": "error"}]}, "duration_ms": 1800}]
  },
  {"type": "stall", "duration_ms": 1500},
  {
    "type": "scroll",
    "steps": [{"to": {"type": "position", "position": "bottom"}, "duration_ms": 1800}]
  },
  {"type": "stall", "duration_ms": 1500}
]
```

This sequence takes approximately 7.6 seconds, excluding preparation and any additional waiting. Choose a compatible recording duration. `fixed` holds or cuts at the requested runtime; `at_most` ends when actions finish or the deadline is reached. Use the same viewport and explicit action timings for requested before/after captures so differences are easier to interpret.

For a lazy-loading review, add `skip_scroll_preload: true` to the recording content. This disables the preparation scroll; it does not disable the recorded scroll actions. For mobile layouts, set both viewport dimensions and the requested mobile/touch behavior explicitly rather than only resizing the finished video.

## Evidence collection

Prefer direct playback when available. If the environment provides a downloader and FFmpeg, a local copy of the returned media can be sampled without creating more HCTI recordings:

```bash
mkdir -p review-frames
ffmpeg -i recording.mp4 -vf fps=1/2 review-frames/frame-%03d.png
```

This samples approximately every two seconds. Inspect the generated frames using available image tools and extract additional frames around suspected problems. A sampling pass can miss brief glitches; do not describe it as continuous playback verification. If these tools are unavailable, use the host's supported media viewing and state the resulting limits.

Useful findings distinguish evidence from interpretation:

> At approximately 4 seconds in the 1280×720 recording, the sticky header covers the first pricing-card heading after scrolling to the section. The attached frame shows the overlap. Check the section's scroll offset and header height.

Do not present examples as actual findings. If no defect is observed, say what sections and moments were inspected and what remains untested.
