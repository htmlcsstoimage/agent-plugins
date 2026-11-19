# A structured, capture-synchronized composition

This is a six-second landscape example, not a required design or an API default. Replace the placeholder brand, claims, and call to action with the user's content. Keep claims grounded in supplied facts.

The opening and proof scenes each occupy two seconds. The call to action appears in the last two seconds and stays visible through the final frame. CSS animations start only when the first recording action adds `body.recording`. Fixed duration keeps capture running for all six seconds without an additional waiting action.

Arguments for `create_htmlcss_recording`:

```json
{
  "content": {
    "html": "<main><p class='brand'>Your brand</p><section class='scene opening'><p class='eyebrow'>New release</p><h1>One clear benefit.</h1></section><section class='scene proof'><p class='eyebrow'>Show the product</p><h2>A feature worth seeing.</h2></section><section class='scene ending'><p class='eyebrow'>The next step</p><h2>Try it today.</h2><p>example.com</p></section></main>",
    "css": "*{box-sizing:border-box}body{margin:0;background:#10251c;color:#fff;font-family:system-ui}main{position:relative;width:960px;height:540px;overflow:hidden;padding:48px 64px}.brand{margin:0;font-size:24px;font-weight:700}.scene{position:absolute;inset:140px 64px 64px;opacity:0;display:flex;flex-direction:column;justify-content:center;gap:18px}h1,h2,p{margin:0}h1,h2{max-width:800px;font-size:64px;line-height:1.08}.eyebrow{color:#80efb0;font-size:22px}.ending>p:last-child{font-size:28px}.recording .opening,.recording .proof{animation:scene 2s both}.recording .proof{animation-delay:2s}.recording .ending{animation:ending 2s 4s both}@keyframes scene{0%{opacity:0;transform:translateY(20px)}15%,85%{opacity:1;transform:translateY(0)}100%{opacity:0;transform:translateY(-12px)}}@keyframes ending{0%{opacity:0;transform:translateY(20px)}15%,100%{opacity:1;transform:translateY(0)}}",
    "viewport_width": 960,
    "viewport_height": 540,
    "device_scale": 1,
    "duration_ms": 6000,
    "duration_mode": "fixed",
    "actions": [
      {"type": "execute_javascript", "script": "document.body.classList.add('recording');"}
    ],
    "output": {"format": "mp4"}
  }
}
```

For custom fonts or remote images, load them before starting the visual timeline. If an action waits for readiness, give it an appropriate timeout and account for that waiting time within the recording, or coordinate preparation with a supported trigger. Avoid waiting indefinitely on an asset that failed to load.

For campaign variants, change the content and design tokens while preserving the scene structure and timing. Keep the number of created recordings within the requested variation count; do not assume that image templates or image batch tools can create recordings.
