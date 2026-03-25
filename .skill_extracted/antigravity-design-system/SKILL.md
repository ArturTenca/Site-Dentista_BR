---
name: antigravity-design-system
description: >
  Use this skill whenever the user wants to rebrand, redesign, or customize their Antigravity website — including changing the site name, company name, logo (image URL or SVG), brand colors, typography, or the overall visual identity across all pages (index.html, services.html, contact.html). Trigger this skill when the user says things like: "mude o nome do site", "troque a logo", "mude as cores", "quero rebrandear", "alterar o design padrão", "mudar identidade visual", "trocar fonte", "novo nome da empresa", "atualizar a logo no nav", or any variation. Also trigger when the user uploads new HTML files and asks to apply a new visual identity, swap branding, or update design tokens. This skill handles ALL pages at once and produces ready-to-download updated .html files.
---

# Antigravity Design System Generator

A skill for rebranding and redesigning the Antigravity website. Handles name, logo, colors, typography, and layout changes across all pages simultaneously, producing updated HTML files ready to use.

---

## Site Architecture (reference)

The site is built with **HTML + Tailwind CSS** (CDN) + Lucide icons. Three main pages:

| File | Purpose | Key branding locations |
|------|---------|----------------------|
| `index.html` | Home page | `<title>`, `#logo-img` src+alt, `#logo-icon-container`, footer brand name, `© year` text, `selection:bg-*` classes on `<body>` |
| `services.html` | Services listing | `<title>`, nav logo `<img>` src+alt, nav brand `<span>` text, footer brand name |
| `contact.html` | Booking / Contact | `<title>`, nav logo `<img>` src+alt, nav brand `<span>` text, footer brand name |

**Current defaults (Aqua Dental Loft):**
- Brand name: `Aqua Dental Loft`
- Logo URL: `https://i.ibb.co/7NRgqhWK/aqua-loft-logo.png`
- Primary accent color: `cyan` (Tailwind: `cyan-600`, `cyan-400`, `cyan-50`)
- Selection highlight: `selection:bg-cyan-100 selection:text-cyan-900`
- Font: Tailwind default (serif used for headings via `font-serif`)

---

## Workflow

### Step 1 — Gather requirements

Ask the user for the following (collect all at once, don't ask one by one):

1. **New site/company name** (e.g., "Smile Studio Pro")
2. **New logo** — one of:
   - A URL to an image (`.png`, `.svg`, `.webp`)
   - An SVG code snippet they paste
   - "Keep the current logo" (only swap the name)
   - "Generate a text logo" (use styled text instead of image)
3. **Primary accent color** — accept any format: hex (`#2563EB`), Tailwind color name (`blue`, `emerald`, `rose`), or natural language (`azul royal`, `verde esmeralda`, `roxo`)
4. **Any other changes** — optional: tagline, footer address/phone/email, hours, page titles

If the user uploads the HTML files directly in the conversation, read them and use those as the source. Otherwise, tell the user to upload the files they want modified.

### Step 2 — Map color tokens

Convert the requested accent color to a Tailwind color scale. The site uses these accent color slots:

```
text-{color}-600       → primary text accent (nav active, badges, labels)
text-{color}-400       → secondary accent (footer icons, dark backgrounds)
bg-{color}-50          → light tint backgrounds (badges, hover states)
bg-{color}-100         → selection highlight bg
text-{color}-900       → selection highlight text
border-{color}-*       → any border accents
hover:text-{color}-600 → hover states in dropdowns and nav
```

**Color mapping table** (natural language → Tailwind):

| User says | Tailwind token |
|-----------|---------------|
| azul, blue | `blue` |
| azul royal | `blue` |
| verde, green | `emerald` |
| verde esmeralda | `emerald` |
| roxo, purple | `violet` |
| rosa, pink | `pink` |
| vermelho, red | `red` |
| laranja, orange | `orange` |
| amarelo, yellow | `amber` |
| cinza, gray | `slate` |
| ciano, teal, aqua | `cyan` (keep current) |
| dourado, gold | `amber` |

### Step 3 — Apply changes to all HTML files

For each uploaded HTML file, perform ALL of these replacements:

#### 3a. Brand name replacements
Search and replace ALL instances of the old company/site name:
- In `<title>` tags
- In `alt` attributes of logo images
- In visible text nodes (nav `<span>`, footer `<a>`, `© year` text, `<p>` copyright)
- In `aria-label` attributes if present

#### 3b. Logo replacements
- Replace `src="..."` of every `<img>` that is the logo (identified by having the old brand name in `alt=`, or being inside `#logo-icon-container` or a nav `<a>` that links to `#` or `index.html`)
- Update the `alt` attribute to match the new brand name
- If user wants a **text logo** instead: remove the `<img>` tag and replace the containing `<div>` with: `<span class="font-serif text-xl font-semibold tracking-tight text-neutral-950">{NewBrandName}</span>`

#### 3c. Accent color replacements
Use a global string replace across the full HTML:

```
cyan-600  →  {newColor}-600
cyan-400  →  {newColor}-400
cyan-50   →  {newColor}-50
cyan-100  →  {newColor}-100
cyan-900  →  {newColor}-900
```

Also replace in `<body>` class:
```
selection:bg-cyan-100 selection:text-cyan-900
→
selection:bg-{newColor}-100 selection:text-{newColor}-900
```

#### 3d. Page `<title>` format
Follow the pattern already in use:
- `index.html`: `{New Brand Name} - {Tagline or specialty}`
- `services.html`: `Our Services | {New Brand Name}`
- `contact.html`: `Book an Appointment | {New Brand Name}` (or whatever the contact page title pattern is)

#### 3e. Footer contact details (if user provides them)
Replace address, phone, email, hours in all footer sections.

### Step 4 — Output

- Write each modified file to `/mnt/user-data/outputs/` with the **same original filename** (`index.html`, `services.html`, `contact.html`)
- Use `present_files` to deliver all three to the user
- Give a concise summary of exactly what was changed in each file

---

## Edge cases

- **Logo is an SVG inline** (not an `<img>`): find the SVG block by proximity to the nav brand text or `id="logo-*"`, replace the entire SVG block with the new one provided
- **User provides a hex color** (e.g. `#6366f1`): do NOT convert to Tailwind tokens. Instead, replace all `text-cyan-*` / `bg-cyan-*` class occurrences with inline `style="color:#6366f1"` or `style="background-color:#6366f1"` as appropriate — but warn the user that Tailwind utility classes are preferred for maintainability
- **User only wants one page changed**: only modify the requested file(s); leave the others untouched
- **User wants to keep the logo but change name only**: skip step 3b entirely, only apply 3a, 3c, 3d
- **Missing files**: if the user hasn't uploaded all three pages, ask which pages they want modified before proceeding

---

## Quality checklist (verify before outputting)

- [ ] ALL instances of old brand name replaced (including `<title>`, alt text, footer text, copyright)
- [ ] Logo `src` and `alt` updated on every page
- [ ] Accent color tokens replaced consistently (no leftover `cyan-` occurrences if color was changed)
- [ ] `selection:bg-*` on `<body>` updated
- [ ] No broken references (href/src paths unchanged except logo URL)
- [ ] Files saved with original filenames

---

## Example interaction

**User:** "Quero mudar o nome do site para 'Branco Sorriso', trocar a logo para essa URL: https://example.com/logo.png, e mudar a cor de destaque para roxo"

**Claude should:**
1. Map "roxo" → Tailwind `violet`
2. Replace all `Aqua Dental Loft` → `Branco Sorriso`
3. Replace logo `src` on all pages → `https://example.com/logo.png`, `alt` → `Branco Sorriso Logo`
4. Replace all `cyan-600` → `violet-600`, `cyan-400` → `violet-400`, etc.
5. Update `<title>` on each page
6. Output all 3 modified files

---

## Reference: branding locations per file

Read `/mnt/skills/user/antigravity-design-system/references/branding-map.md` for a detailed line-by-line map of every branding location in each file (useful when doing precise targeted replacements).
