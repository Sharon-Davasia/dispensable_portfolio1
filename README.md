# Sharon K Davasia — Portfolio

A static, single-page portfolio. There is no build step, package install, or backend.

## What’s in this folder

| Path | Purpose |
| --- | --- |
| `index (1).html` | The page to open. Fonts and styles are already inlined. |
| `site/index.template.html` | Source template. The `__FONTS_CSS__` placeholder is where font CSS is inserted. |
| `fonts/` | Local font stylesheets (`Inter`, `Instrument Serif`, `JetBrains Mono`). |
| `SHARON_RESUME_LATEST.pdf` | Resume linked from the page. |

## Start the site

### Option 1 — open the file

Double-click `index (1).html`, or open it from your browser with **File → Open**.

That is enough to view the page. No install is required.

### Option 2 — local server

A local server is useful when you want normal `http://` links, including the résumé download.

**Python** (from this folder):

```bash
python -m http.server 8080
```

Then open [http://localhost:8080/index%20(1).html](http://localhost:8080/index%20(1).html).

Stop the server with `Ctrl+C`. On some machines the command is `py -m http.server 8080` or `python3 -m http.server 8080`.

**Node.js:**

```bash
npx --yes serve .
```

Open the URL it prints, then go to `index (1).html`.

**Windows PowerShell** (no Python or Node required). From this folder:

```powershell
$root = (Get-Location).Path
$listener = New-Object System.Net.HttpListener
$listener.Prefixes.Add('http://127.0.0.1:8080/')
$listener.Start()
Write-Host "Open http://127.0.0.1:8080/index%20(1).html"
while ($listener.IsListening) {
  $ctx = $listener.GetContext()
  $rel = [Uri]::UnescapeDataString($ctx.Request.Url.AbsolutePath.TrimStart('/'))
  if ([string]::IsNullOrWhiteSpace($rel)) { $rel = 'index (1).html' }
  $full = Join-Path $root ($rel -replace '/','\')
  if (Test-Path -LiteralPath $full -PathType Leaf) {
    $bytes = [System.IO.File]::ReadAllBytes($full)
    $ctx.Response.ContentType = 'text/html; charset=utf-8'
    if ($full.EndsWith('.pdf')) { $ctx.Response.ContentType = 'application/pdf' }
    $ctx.Response.ContentLength64 = $bytes.Length
    $ctx.Response.OutputStream.Write($bytes, 0, $bytes.Length)
  } else { $ctx.Response.StatusCode = 404 }
  $ctx.Response.Close()
}
```

Then open [http://127.0.0.1:8080/](http://127.0.0.1:8080/). Stop it by closing that terminal.

## Notes

- The page is self-contained. You do not need the `fonts/` folder to view `index (1).html`.
- A few Cloudflare scripts at the bottom of the saved page only run on the original host. They do not affect the layout when you open the file locally.
- Theme (light / dark) is stored in the browser under the key `skd-theme`.
