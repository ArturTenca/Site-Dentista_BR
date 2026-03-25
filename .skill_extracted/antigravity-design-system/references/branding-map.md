# Branding Location Map — Antigravity Site

Detailed map of every location where brand identity appears across the 3 HTML files.
Use this for precise, targeted replacements.

---

## index.html

| Location | Element | Attribute/Content | Current value |
|----------|---------|-------------------|---------------|
| `<head>` | `<title>` | text | `Aqua Dental Loft - Cosmetic, General & Emergency Dentistry` |
| `<body>` | body tag | class | `selection:bg-cyan-100 selection:text-cyan-900` |
| Nav logo | `<img id="logo-img">` | `src` | `https://i.ibb.co/7NRgqhWK/aqua-loft-logo.png` |
| Nav logo | `<img id="logo-img">` | `alt` | `Aqua Dental Loft Logo` |
| Dropdown hovers | `<a>` elements in nav dropdowns | class | `hover:text-cyan-600` |
| Nav active state | `<a>` or `<button>` | class | `text-cyan-600`, `bg-cyan-50` |
| Section badges | Various `<span>` and `<div>` | class | `text-cyan-600`, `bg-cyan-50` |
| Footer brand | `<a class="font-serif text-2xl...">` | text | `Aqua Dental Loft` |
| Footer icons | `<svg>` wrappers in contact section | class | `text-cyan-400` |
| Copyright | `<p>` in footer bottom | text | `© 2026 Aqua Dental Loft. All rights reserved.` |
| Selection highlight | `<body>` | class | `selection:bg-cyan-100 selection:text-cyan-900` |

### Accent color occurrences in index.html (search for these):
- `text-cyan-600` — nav hover, badges, labels, section accents
- `text-cyan-400` — footer map pin, phone, mail icons
- `bg-cyan-50` — badge backgrounds, nav active bg
- `bg-cyan-100` — selection highlight
- `text-cyan-900` — selection text
- `hover:text-cyan-600` — dropdown link hovers
- `focus:ring-cyan-*` — if any form inputs

---

## services.html

| Location | Element | Attribute/Content | Current value |
|----------|---------|-------------------|---------------|
| `<head>` | `<title>` | text | `Our Services \| Aqua Dental Loft` |
| Nav logo | `<img>` inside nav `<a href="index.html">` | `src` | `https://i.ibb.co/7NRgqhWK/aqua-loft-logo.png` |
| Nav logo | `<img>` | `alt` | `Aqua Dental Loft Logo` |
| Nav brand text | `<span class="font-serif text-lg...">` | text | `Aqua Dental Loft` |
| Nav active link | `<a href="services.html">` | class | `text-cyan-600 bg-cyan-50` |
| Mobile menu | `<a href="services.html" class="...text-cyan-400...">` | class | `text-cyan-400` |
| Service badges | `<div class="text-[10px]...text-cyan-600 bg-cyan-50...">` | class | `text-cyan-600`, `bg-cyan-50` |
| Footer brand | `<a>` or `<span>` with brand text | text | `Aqua Dental Loft` |
| Copyright | `<p>` in footer | text | `© 2026 Aqua Dental Loft. All rights reserved.` |

### Accent color occurrences in services.html:
- `text-cyan-600` — nav active, service category badges
- `text-cyan-400` — mobile menu active item, footer icons
- `bg-cyan-50` — badge tints, nav active background
- `hover:text-cyan-600` — nav and CTA hover states

---

## contact.html

| Location | Element | Attribute/Content | Current value |
|----------|---------|-------------------|---------------|
| `<head>` | `<title>` | text | `Book an Appointment \| Aqua Dental Loft` (or similar) |
| Nav logo | `<img>` in nav | `src` | `https://i.ibb.co/7NRgqhWK/aqua-loft-logo.png` |
| Nav logo | `<img>` | `alt` | `Aqua Dental Loft Logo` |
| Nav brand text | `<span>` | text | `Aqua Dental Loft` |
| Form accents | inputs focus rings, labels | class | `focus:ring-cyan-*`, `text-cyan-600` |
| Footer brand | brand name text | text | `Aqua Dental Loft` |
| Copyright | footer `<p>` | text | `© 2026 Aqua Dental Loft. All rights reserved.` |

---

## Global replacement strategy (Python pseudocode)

```python
def rebrand(html_content, old_name, new_name, old_color, new_color, old_logo_url, new_logo_url):
    # 1. Brand name (case-sensitive, covers all instances)
    result = html_content.replace(old_name, new_name)
    
    # 2. Logo URL
    if new_logo_url:
        result = result.replace(old_logo_url, new_logo_url)
    
    # 3. Logo alt text (after name replacement, alt already updated)
    # Already handled by brand name replace if alt="Aqua Dental Loft Logo"
    # But also handle partial match: alt="Aqua Dental Loft Logo" → alt="{new_name} Logo"
    
    # 4. Accent color tokens
    for shade in ['50', '100', '200', '300', '400', '500', '600', '700', '800', '900']:
        result = result.replace(f'{old_color}-{shade}', f'{new_color}-{shade}')
    
    return result
```

---

## Notes for Claude

- The nav on `index.html` only shows the logo image (no text), while `services.html` and `contact.html` show both the logo image AND the brand name as text next to it. Make sure both are updated.
- The footer on `index.html` uses a `<a class="font-serif text-2xl text-white">` for the brand name — this is text-only, no image.
- When replacing colors, process ALL shades (`-50` through `-900`) to avoid missing variants.
- `selection:bg-cyan-100` and `selection:text-cyan-900` are on the `<body>` tag class — easy to miss because they're not in the visible content.
