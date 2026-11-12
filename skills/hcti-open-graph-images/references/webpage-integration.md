# Webpage integration for dynamic Open Graph images

Read this reference when adding or reviewing webpage metadata for an HCTI Open Graph configuration.

## Build the public image URL

The `create_og_config` response contains both `id` and `domain_id`. Use `domain_id` in public image URLs:

```text
https://hcti.io/v1/og/{domain_id}{source_page_pathname}
```

The pathname after `domain_id` is loaded from the configuration's `base_url`:

```text
base_url:    https://example.com
source page: https://example.com/products/widget
image URL:   https://hcti.io/v1/og/DOMAIN_ID/products/widget
```

Use the same pathname as the source page. Do not include the source origin again. Query strings do not produce separate images; use `hcti:content_version` when a page's render inputs need an explicit version change.

## Screenshot configuration example

For an `html_css` configuration with `extract_values: true`, place standard preview metadata and HCTI rendering metadata in the initial HTML head section. Create one `meta` element for each row:

| Attribute | Key | Content |
| --- | --- | --- |
| `property` | `og:title` | `Product launch` |
| `property` | `og:description` | `See what is new in this release.` |
| `property` | `og:image` | `https://hcti.io/v1/og/DOMAIN_ID/articles/product-launch` |
| `property` | `og:image:alt` | `Product launch announcement` |
| `property` | `hcti:selector` | `#social-card` |
| `property` | `hcti:viewport_width` | `1200` |
| `property` | `hcti:viewport_height` | `630` |

The matching page must contain a `section` element whose `id` is `social-card`, with the content HCTI should capture inside it.

Use the `property` attribute for the examples below. HCTI also accepts the `name` attribute for `hcti:*` fields. Keep these elements inside the HTML head section and render them into the server response; HCTI does not rely on client-side JavaScript to discover them.

## Template configuration example

For a `templated` configuration, publish template values using `html:tv:{template_key}`. Create one `meta` element for each row:

| Attribute | Key | Content |
| --- | --- | --- |
| `property` | `og:title` | `Product launch` |
| `property` | `og:description` | `See what is new in this release.` |
| `property` | `og:image` | `https://hcti.io/v1/og/DOMAIN_ID/articles/product-launch` |
| `property` | `og:image:alt` | `Product launch announcement` |
| `property` | `html:tv:headline` | `Product launch` |
| `property` | `html:tv:author.name` | `Ada Lovelace` |

Dots in the template key create nested values, so `html:tv:author.name` supplies `author.name`. A content value is parsed as JSON when it is valid JSON; otherwise it remains a string.

A configuration mapping can reuse existing metadata instead of requiring an `html:tv:*` tag. Examples:

- Map `headline` with `fallback: titles` to use the document title element, `og:title`, or `twitter:title` in that order.
- Map `summary` with `fallback: descriptions` to use `description`, `og:description`, or `twitter:description` in that order.
- Map `price` with `meta_key: product:price` and publish a `meta` element whose `property` is `product:price` and whose content is `$19.00`.

When both a mapping and `html:tv:*` provide the same template key, the direct `html:tv:*` value wins.

## Twitter Card metadata

Use the same HCTI endpoint for Twitter/X previews. Standard Twitter fields conventionally use the `name` attribute. Create one `meta` element for each row:

| Name | Content |
| --- | --- |
| `twitter:card` | `summary_large_image` |
| `twitter:title` | `Product launch` |
| `twitter:description` | `See what is new in this release.` |
| `twitter:image` | `https://hcti.io/v1/og/DOMAIN_ID/articles/product-launch` |
| `twitter:image:alt` | `Product launch announcement` |

The configuration's `optimization_mode` controls whether HCTI preserves one size, post-processes one render, or renders with crawler-specific viewports.

## Webpage `hcti:*` fields

Page-level values override matching defaults from an `html_css` configuration. Boolean values use `true` or `false`.

### Capture and content

| Field | Purpose |
| --- | --- |
| `hcti:selector` | Crop to the element matching a CSS selector; prefer a stable unique ID. |
| `hcti:css` | Inject page-specific CSS after the page loads. |
| `hcti:full_screen` | Capture the entire scrollable page instead of the initial viewport. |
| `hcti:transparent_background` | Render without a background; use a format that supports alpha. |
| `hcti:disable_twemoji` | Use native Linux emoji rather than HCTI's Twemoji fallback. |
| `hcti:content_version` | Integer version included in the content identity; increment it to force changed inputs at the same pathname. |

### Viewport and output scale

| Field | Purpose and constraints |
| --- | --- |
| `hcti:viewport_width` | Viewport width from 1 to 6000 pixels; set with `viewport_height`. |
| `hcti:viewport_height` | Viewport height from 1 to 6000 pixels; set with `viewport_width`. |
| `hcti:device_scale` | Pixel ratio from 0.1 to 3; higher values increase output dimensions, file size, and render time. |
| `hcti:viewport_mobile` | Enable mobile viewport behavior. |
| `hcti:viewport_landscape` | Enable landscape viewport orientation. |
| `hcti:viewport_touch` | Report touch capability to the page. |
| `hcti:jumbo_max_width` | Advanced jumbo width; must be supplied with `jumbo_max_height` and consumes additional renders. |
| `hcti:jumbo_max_height` | Advanced jumbo height; must be supplied with `jumbo_max_width` and consumes additional renders. |

For jumbo mode, both dimensions must be greater than zero and no more than 80,000, at least one must exceed 8,000, and their total area cannot exceed 400,000,000 pixels.

### Timing and lifecycle

| Field | Purpose and constraints |
| --- | --- |
| `hcti:ms_delay` | Extra delay before capture, from 0 to 10,000 milliseconds. |
| `hcti:max_wait_ms` | Limit for otherwise-irrelevant page loading activity, from 500 to 10,000 milliseconds and subject to the plan. |
| `hcti:render_when_ready` | Wait for the page's explicit HCTI readiness signal; use only when the page reliably sends it. |

### Browser context

| Field | Purpose and accepted values |
| --- | --- |
| `hcti:color_scheme` | Emulate `light` or `dark` color preference. |
| `hcti:timezone` | Use an IANA timezone such as `America/New_York` or `UTC`. |
| `hcti:media_type` | Render with `screen` or `print` CSS media rules. |
| `hcti:block_consent_banners` | Attempt to dismiss cookie or consent banners. |
| `hcti:identify_as_hcti` | Send `X-HCTI-SCREENSHOT: 1` on the top-level page request. |

## Standard social metadata

Keep ordinary preview metadata alongside the HCTI image URL. Common fields include:

- `og:title`, `og:type`, `og:description`, `og:url`, and `og:site_name`
- `og:image` and `og:image:alt`
- `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`, and `twitter:image:alt`

Only publish `og:image:width`, `og:image:height`, and `og:image:type` when those values accurately describe every response from the configured endpoint. Crawler-specific optimization can make fixed dimensions misleading.

## Verification checklist

- Fetch the source page and confirm the tags appear in the initial HTML head section.
- Confirm the HCTI URL uses `domain_id`, not the management `id`.
- Confirm its pathname matches the source page pathname.
- Open the HCTI image URL directly and inspect the rendered result.
- If the result is stale, review the refresh interval, source cache headers or ETag, and `hcti:content_version`.
- Test the final public page in the relevant platform's link-preview debugger when available; platform caches are separate from HCTI's cache.

Authoritative guides:

- [Dynamic Open Graph image setup](https://docs.htmlcsstoimage.com/getting-started/og-images/)
- [Supported page metadata](https://docs.htmlcsstoimage.com/getting-started/og-images/supported-parameters/)
- [Caching and refresh behavior](https://docs.htmlcsstoimage.com/guides/debugging/og-image-caching/)
- [Platform and framework guides](https://docs.htmlcsstoimage.com/guides/og-images/)
