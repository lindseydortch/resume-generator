# Resume Editor

A self-contained, single-file resume editor that converts markdown to a styled PDF. Built as a free, owned alternative to subscription resume tools like Teal — keep your resume content as plain markdown files, customize the look with your brand colors and fonts, and export to PDF in one click.

## Features

- **Live preview** — edit markdown on the left, see your resume render in real time on the right
- **Brand color customization** — set primary and secondary accent colors via color pickers
- **Font selection** — pick from 8 common resume fonts
- **One-click PDF export** — download a US Letter PDF named after the resume's title
- **Multiple resume support** — keep separate `.md` files for different role types and paste in whichever you need
- **GFM line breaks** — single newlines render as line breaks (great for skill sections)
- **No subscription, no account, no internet required** — runs entirely in your browser

## Quick Start

1. Download `resume-editor.html`
2. Double-click the file to open it in your browser (Chrome, Firefox, Safari, or Edge)
3. The editor loads with a sample resume — replace it with your own markdown
4. Adjust accent colors and font in the header controls
5. Click **Download PDF** to export

No installation, no build step, no server. The file is fully portable — keep it on your desktop, in Dropbox, or in a project folder.

## How to Use It

### Editing your resume

The left pane is a markdown editor. Type or paste your resume content and the right pane updates instantly. Your changes live only in the browser tab — close the tab and they're gone, so save your markdown back to a file when you're done.

### Customizing the look

Three controls in the header:

| Control | What it changes |
|---------|----------------|
| **Primary** color | Your name, section headers, section underlines, and links |
| **Secondary** color | Job/school entry titles and bold skill category labels |
| **Font** dropdown | The font used throughout the resume |

Changes apply instantly to both the preview and the exported PDF.

### Exporting to PDF

Click **Download PDF**. The file is named automatically based on the `#` heading at the top of your resume (e.g., "Lindsey Dortch" → `lindsey-dortch.pdf`).

### Working with multiple resume versions

The intended workflow:

1. Keep your master resume markdown in a file like `resume-master.md`
2. For different role types, save variants: `resume-frontend.md`, `resume-fullstack.md`, `resume-product.md`, etc.
3. For each application, duplicate the closest variant, delete bullets you don't want, and tweak the summary
4. Open the editor, paste the markdown, export PDF, done

This replaces the "toggle bullets on/off" workflow from subscription tools with plain-text version control.

## Markdown Structure

The editor expects a specific heading hierarchy. Each level is styled differently:

```markdown
# Your Name
## Your Title or Tagline
Email: you@example.com | Phone: 555-555-5555 | [LinkedIn](https://linkedin.com/in/you)

### Section Header
Content for this section.

#### Company | Role | Date Range
- Bullet point one
- Bullet point two
```

| Markdown | Renders as | Styled with |
|----------|-----------|-------------|
| `# Name` | Large name at top | Primary color |
| `## Subtitle` | Job title / location under name | Muted gray |
| `### Section` | Section header (Experience, Skills, etc.) | Primary color + underline |
| `#### Entry` | Individual job or school | Secondary color |
| `- bullet` | Bullet point | Default text |
| `**bold**` | Skill category labels | Secondary color |
| `[text](url)` | Link | Primary color |

### Line breaks

Single line breaks in markdown render as actual line breaks in the preview. This is especially useful for the Technical Skills section:

```markdown
### Technical Skills
**Languages**: HTML | CSS | JavaScript
**Frameworks**: React | Next.js | Node.js
**Databases**: PostgreSQL | MongoDB
```

Each line above becomes its own line in the PDF — no `<br>` tags needed.

## ATS Compatibility

⚠️ **Important caveat**: the PDF this tool generates is **image-based** (the preview is rendered to a canvas, then embedded as an image in the PDF). This means:

- Looks great visually with full color/font fidelity
- Some applicant tracking systems (ATS) may struggle to parse text from it
- Modern ATSs that use OCR (Workday, Greenhouse, Lever) typically handle it fine
- Older or stricter parsers may miss content entirely

### Tips to improve ATS parsing

1. **Show URLs as plain text** in your contact line rather than hiding them behind markdown link syntax:
   - ✅ `LinkedIn: linkedin.com/in/lindseydortch`
   - ⚠️ `[LinkedIn](https://linkedin.com/in/lindseydortch)` — the link works but the URL isn't visible as text
2. **Always paste your full resume text** into a second field on the application if the parser misses sections
3. **Test your PDF** by uploading it to a free ATS-checker (Jobscan, Resume Worded) before applying to important roles

If you need a text-based PDF (rather than image-based) for stricter ATS compatibility, that's a separate build — the current approach trades parser-friendliness for visual fidelity.

## Tech Stack

- Pure HTML/CSS/JS in a single file — no build step, no dependencies installed locally
- [marked.js](https://marked.js.org/) (v12.0.0) for markdown parsing, loaded from CDN
- [html2pdf.js](https://github.com/eKoopmans/html2pdf) (v0.10.1) for PDF generation, loaded from CDN
- No browser storage used — content lives only in the tab's memory until you export

## Browser Support

Works in any modern browser:

- Chrome / Edge / Brave (Chromium)
- Firefox
- Safari

Requires JavaScript and an internet connection on first load (to fetch the marked.js and html2pdf.js libraries from a CDN). After the libraries are cached, subsequent uses work offline.

## File Structure

```
resume-editor.html       # The entire app — open this in a browser
resume-master.md         # (Your file) Your full resume content
resume-frontend.md       # (Your file) Variant for frontend roles
resume-fullstack.md      # (Your file) Variant for fullstack roles
...
```

No build directory, no `node_modules`, no config files. Just the HTML and your markdown.

## Customization

To change defaults without using the UI controls, edit these CSS variables at the top of the `<style>` block in `resume-editor.html`:

```css
:root {
  --resume-accent: #1a1a1a;       /* Primary color */
  --resume-accent-2: #444444;     /* Secondary color */
  --resume-font: 'Georgia', serif; /* Default font */
}
```

To change page sizing or margins, edit the `#preview` rule:

```css
#preview {
  width: 8.5in;              /* US Letter width */
  min-height: 11in;          /* US Letter height */
  padding: 0.55in 0.65in;    /* Page margins */
  ...
}
```

For A4 instead of US Letter, change to `width: 210mm; min-height: 297mm;` and update the `jsPDF` format in the download handler from `'letter'` to `'a4'`.

## Troubleshooting

**The preview doesn't update.** Make sure JavaScript is enabled in your browser. Check the browser console (F12) for errors.

**The PDF is blank or cut off.** Try refreshing the page and re-pasting your content. If a single job entry is very long, page breaks may render awkwardly — split bullets across multiple shorter entries if needed.

**The font doesn't apply in the PDF.** The font must be installed on your system. The dropdown lists common system fonts that ship with macOS and Windows by default. Custom fonts (e.g., your brand font from a paid foundry) would require loading the font file separately.

**Special characters render as `?` or boxes.** Make sure your markdown is saved as UTF-8. Most modern editors do this by default.

**The ATS didn't pick up my LinkedIn.** See the ATS Compatibility section above — show the URL as plain text rather than using markdown link syntax.

## Roadmap (Possible Future Additions)

- Text-based PDF export for better ATS parsing
- Google Fonts integration with searchable picker
- Local font detection (Chrome/Edge only)
- Save/load resume files directly from the UI
- Multiple page support with smart page-break controls
- A4 / US Letter toggle in the UI

## License

Free to use, modify, and distribute. No attribution required.