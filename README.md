# MIP Analyser

**SIG-MIP-001 · v1.0**

A browser-based form for the Media Integrity Protocol. Walks an investigator field-by-field through 15 fields per source (across up to 3 sources), then produces a structured, copy-paste-ready MIP report for dossier inclusion.

Embedded at: https://signalandshadow.io/mip-analyser
Standalone: https://signalandshadow.github.io/mip-analyser/

## What it does

The Analyser does not perform forensic analysis itself. Run your ELA, FFT, metadata, codec, and C2PA checks in your specialist tools first (Forensically, FotoForensics, ExifTool, MediaInfo, c2patool, etc.), then enter the findings here. The Analyser structures the inputs into a SIG-MIP-001-conformant report with consistent labelling, integrity classification, and citation anchor.

## Features

- **15 fields per source.** Source provenance, platform, licence, file metadata, ELA variance, FFT inconsistencies, codec fingerprinting, C2PA Content Credentials, SHA-256 hard binding, pHash soft binding, chronological markers, and overall integrity assessment.
- **Up to 3 sources per dossier.** Switch between sources without losing entered data.
- **Keyboard-driven.** `Enter` to advance, `A-E` to pick a radio option. Auto-focus on each new field.
- **Three output paths.** Copy to clipboard, download as `.txt` (filename includes the case ID), or print (print stylesheet strips chrome).
- **Embed-aware.** Detects iframe context, hides page chrome automatically, and posts height to the parent window for auto-resize.
- **Client-side only.** No data leaves the browser. Form state is held in memory for the session.

## Local development

Single-file static site. No build step.

```
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Deployment

Hosted on GitHub Pages from the `main` branch root. Any push to `main` triggers a redeploy (typically 30 to 90 seconds).

## Embed mode

The page detects iframe context automatically. When loaded inside an iframe (or with `?embed=1` for local testing), only the site-wide page footer is hidden. The status bar, hero, and form all stay visible, since the hero is the tool's actual header. The status bar becomes static (rather than sticky) so it scrolls naturally inside the parent page. The page background is set to transparent so the iframe blends with the parent.

The page posts its height to the parent window via `postMessage` with type `ss-mip-analyser-height`, allowing the parent to auto-resize the iframe.

### Iframe snippet for the parent page

```html
<iframe
  src="https://signalandshadow.github.io/mip-analyser/?embed=1"
  style="width: 100%; border: 0; min-height: 800px;"
  id="mip-analyser-frame"
  title="MIP Analyser">
</iframe>
<script>
  window.addEventListener('message', (e) => {
    if (e.data && e.data.type === 'ss-mip-analyser-height') {
      document.getElementById('mip-analyser-frame').style.height = e.data.height + 'px';
    }
  });
</script>
```

## Standards

Built and verified under **LST-001 v1.0.3**. House editorial standard governs every line of output, including the dossier export strings.

Output schema: **SIG-MIP-001 v1.0**.

## Licence

MIT. See `LICENSE`.

## Maintainer

Derek Bowler · Signal &amp; Shadow · Versoix, Geneva
