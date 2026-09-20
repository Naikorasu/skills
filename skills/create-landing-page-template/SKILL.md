---
name: create-landing-page-template
description: Recreates an authorized website URL as a self-contained PHP landing-page template. Use when the user provides a page URL and wants its visible design, content, images, CSS, and UI behavior reproduced locally with index.php, setTitle.php, setSeoText.php, local assets, an AMP page, Chrome DevTools MCP inspection, and a local PHP preview.
compatibility: Requires Chrome, Node.js with npx, and PHP CLI. Uses the Chrome DevTools MCP server from chrome-devtools-mcp.
---

# Create Landing Page Template

Act as a UI designer, frontend developer, and PHP programmer. Convert the main page at a user-provided URL into a maintainable, self-contained PHP landing-page template while preserving its visual hierarchy, responsive behavior, and essential UI interactions.

## Required Inputs

Obtain the following before implementation:

- The source page URL.
- The destination directory. Use the current working directory only when the user does not specify one.
- The brand, domain, and target URL when they cannot be derived safely from the source page.
- Confirmation that the user owns the page or is authorized to reproduce it.

Treat `$target_url` as the destination used by calls to action and outbound conversion links. Treat the provided page URL as `$source_url`. Do not assume that these URLs are identical.

Do not copy login pages, checkout pages, private dashboards, personal data, credentials, tracking identifiers, or content outside the authorized source page. Do not bypass authentication, bot protection, paywalls, or access controls. Do not navigate to unrelated pages unless an asset required by the source page is hosted there.

## Required Output

Create this exact structure:

```text
<destination>/
├── index.php
├── setTitle.php
├── setSeoText.php
├── assets/
│   ├── img/
│   ├── css/
│   └── js/
└── amp/
    └── index.php
```

Do not add a framework, build system, package manifest, database, router, templating engine, or dependency unless the source page cannot be reproduced without it and the user explicitly approves the addition.

## Dependency Setup

Use existing tools before installing anything.

1. Check whether Chrome, `npx`, PHP CLI, and Chrome DevTools MCP capabilities are available.
2. Use the host's existing Chrome DevTools MCP integration when it is available. In Pi, load the relevant Chrome DevTools tools before browser work when they are not already active.
3. When the MCP server is missing, configure or run the official package with this command:

   ```sh
   npx -y chrome-devtools-mcp@latest
   ```

   A standard MCP client configuration uses `npx` with `-y` and `chrome-devtools-mcp@latest` as its arguments.
4. When PHP CLI is missing, identify the operating system and available package manager. Ask for confirmation before a system-wide installation or any command requiring elevated privileges. Start an approved installation as a background process, write its output to a log file, record its process ID, and wait for successful completion before serving the template.
5. Do not install a separate PHP development-server package. The PHP CLI already provides the required server through `php -S`.

Do not claim that a background installation succeeded until its process has exited successfully and the executable passes a version check.

## Workflow

### 1. Inspect the Source Page

Use Chrome DevTools MCP for all browser inspection and visual review.

1. Open the exact source URL in a new page.
2. Record the final URL after redirects, HTTP status, viewport title, meta description, canonical URL, favicon, logo, banner or hero image, primary calls to action, and visible text.
3. Capture desktop and mobile screenshots as visual references.
4. Inspect the rendered DOM, computed styles, responsive breakpoints, fonts, colors, spacing, layout, hover and focus states, and UI interactions.
5. Inspect the Network panel and classify requests as documents, stylesheets, images, fonts, UI scripts, data requests, analytics, advertising, or third-party embeds.
6. Wait for lazy-loaded visible assets to finish loading before recording their final URLs.
7. Note console errors before implementation so that source-page errors are not mistaken for template regressions.

The rendered page is the visual source of truth. Raw HTML alone is insufficient when the page is client-rendered.

### 2. Create a Local Content Snapshot

Reproduce only the main page and assets needed by that page.

- Preserve semantic HTML, heading order, navigation landmarks, labels, alt text, keyboard access, focus visibility, and responsive layout.
- Convert rendered dynamic content into static HTML when it is needed for the visible landing page.
- Remove analytics, trackers, advertising tags, service workers, push notifications, fingerprinting, consent SDKs that are not required for layout, and unrelated third-party scripts.
- Do not copy source maps, build caches, API credentials, authorization headers, cookies, local storage, session data, or inline secrets.
- Preserve attribution, copyright notices, and license notices when required.

Do not create a generic redesign when the user requested a faithful template. Use the source screenshots to match visual proportions and behavior, but keep the implementation smaller than the source application whenever static HTML, CSS, and minimal JavaScript produce the same result.

### 3. Download and Rewrite Assets

Download visible images, icons, backgrounds, fonts when licensing permits, stylesheets, and UI-only scripts into the required local folders.

- Store images and fonts referenced by CSS under `assets/img/` unless a font-specific folder is explicitly requested.
- Store CSS under `assets/css/`.
- Store UI JavaScript under `assets/js/`.
- Use deterministic, descriptive filenames and remove query strings from local filenames.
- Verify each response status, MIME type, and file size before saving it.
- Reject unexpected executable content disguised as an image or stylesheet.
- Rewrite HTML `src`, `srcset`, poster, icon, preload, stylesheet, and script references to local paths.
- Rewrite CSS `url(...)` and `@import` references to local paths. Prefer merging small imported stylesheets into one local stylesheet instead of preserving an import chain.
- Remove duplicate, unused, and page-irrelevant assets.
- Do not hotlink assets from the source website when a local copy is authorized and available.
- Keep outbound business links in `$target_url` or another clearly named variable rather than scattering hard-coded URLs through `index.php`.

Use `curl` or `wget` for asset downloads after Chrome DevTools MCP has identified the final asset URLs. Send no source-page cookies or authorization headers during downloads unless the user explicitly authorizes them and the assets are private resources they own.

### 4. Disable Data Fetching Without Breaking UI

Inspect every retained script for `fetch`, `XMLHttpRequest`, Axios, GraphQL, WebSocket, EventSource, JSONP, and equivalent network calls.

- Disable requests used only for remote data, analytics, telemetry, personalization, form submission, or background synchronization.
- Replace visible remote data with the static rendered snapshot captured from the page.
- Preserve scripts required only for local UI behavior, such as menus, tabs, accordions, sliders, modal dialogs, and form validation.
- Remove network code instead of adding a global `fetch` override. A global override can silently break legitimate UI behavior.
- Prevent forms and calls to action from submitting to the source website. Route intended calls to action through the configured `$target_url`, or keep forms inert when no destination is supplied.
- Confirm through the Network panel that the local template makes no unintended requests to the source domain or third-party data APIs.

### 5. Implement `setTitle.php`

Define configuration variables in `setTitle.php`. It must contain at least:

```php
<?php
$brand = 'Example Brand';
$domain = 'example.test';
$source_url = 'https://source.example/';
$target_url = 'https://example.test/';
$meta_title = 'Example Page Title';
$meta_description = 'Example page description.';
$image_logo = 'assets/img/logo.webp';
$image_banner = 'assets/img/banner.webp';
$amp_url = 'https://example.test/amp/';
```

Add only variables used by `index.php` or `amp/index.php`, such as `$canonical_url`, `$favicon`, or `$theme_color`. Keep content and configuration out of `index.php` when they belong in these variables. Do not place secrets in this file.

Use `require __DIR__ . '/setTitle.php';` from the main page and `require dirname(__DIR__) . '/setTitle.php';` from the AMP page. Escape variable output according to its HTML context with `htmlspecialchars(..., ENT_QUOTES, 'UTF-8')`.

### 6. Implement `setSeoText.php`

Write native semantic HTML containing the SEO text. Use real paragraph tags and an optional containing section, for example:

```html
<section class="seo-text" aria-labelledby="seo-text-heading">
  <h2 id="seo-text-heading">About Example Brand</h2>
  <p>Useful, accurate, human-readable information about the page topic.</p>
  <p>Additional information that helps visitors understand the offer.</p>
</section>
```

Do not include `html`, `head`, or `body` wrappers. Do not use hidden text, keyword stuffing, doorway text, generated claims, copied reviews, or facts not supported by the source page. Include this file in `index.php` with `include __DIR__ . '/setSeoText.php';` at a visually appropriate location.

### 7. Implement `index.php`

The main page must:

- Enable strict types when compatible with the implementation.
- Load `setTitle.php` before output.
- Output escaped title, description, canonical URL, favicon, and `rel="amphtml"` metadata.
- Load only local CSS, images, and UI JavaScript.
- Use semantic, accessible HTML and preserve the source page's responsive behavior.
- Include `setSeoText.php` exactly once.
- Use `$target_url` for intended conversion links.
- Avoid remote data dependencies and unnecessary PHP logic.

Use root-relative or correctly computed local paths consistently. Verify the template from both `/` and `/index.php`.

### 8. Implement `amp/index.php`

Create a valid, simplified AMP counterpart when the page content can be represented in AMP.

- Load `setTitle.php` from the parent directory.
- Set its canonical URL to the main page.
- Use AMP-compatible markup such as `amp-img` with explicit dimensions.
- Place permitted CSS in `style amp-custom` and omit ordinary custom JavaScript.
- Reuse the same factual content and configured target URL.
- Do not label a page as AMP unless it passes AMP validation.

If a valid AMP page cannot represent an essential interaction, explain the limitation and create the closest valid static version rather than shipping invalid AMP markup.

### 9. Validate Before Preview

Perform the smallest reliable checks:

1. Run PHP syntax checks on `index.php`, `setTitle.php`, and `amp/index.php`.
2. Confirm that all local asset references resolve and that no required file is empty.
3. Search generated HTML, CSS, and JavaScript for stale source-domain asset URLs, data API URLs, trackers, and hard-coded conversion links.
4. Start the local PHP server.
5. Open the local page with Chrome DevTools MCP at desktop and mobile viewport sizes.
6. Compare screenshots with the source references and correct material differences in layout, typography, spacing, color, imagery, overflow, and responsive behavior.
7. Check the browser console, failed network requests, keyboard navigation, focus states, image alt text, heading order, and common accessibility issues.
8. Confirm that only intended local resources and explicitly approved outbound links are requested.
9. Validate the AMP page when an AMP validator is available.

Do not declare completion while PHP syntax errors, missing local assets, unintended remote data requests, broken primary UI interactions, or serious console errors remain.

## Preview

Use PHP's built-in development server. Choose an available loopback port and bind only to loopback unless the user explicitly requests network access.

```sh
php -S 127.0.0.1:8080 -t <destination>
```

Run the server as a background process with logs redirected to a file. Record its process ID so it can be stopped later. Then use Chrome DevTools MCP to open:

```text
http://127.0.0.1:8080/
http://127.0.0.1:8080/amp/
```

If port `8080` is occupied, select another available port and report the actual URL.

## Completion Report

When the template is ready, provide:

- The destination directory.
- The local preview URL and AMP preview URL.
- The PHP server process ID and log path.
- A concise list of downloaded asset types.
- A concise list of disabled data requests, trackers, and third-party integrations.
- Any deliberate visual or functional differences from the source.
- Validation results, including PHP syntax, browser console, missing requests, responsive review, accessibility basics, and AMP validation status.

Keep the preview server running for the user unless they ask to stop it. Never report an exact visual match without reviewing the served local page through Chrome DevTools MCP.
