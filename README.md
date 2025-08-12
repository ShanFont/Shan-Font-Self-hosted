# Shan Font (Self-Hosted Package)

Production-ready package for self-hosting **Shan** webfonts.  
Includes optimized `.woff2` fonts, CSS with Unicode ranges, and server configs for caching & security.

## 📂 File Structure
```
shan-font/
├─ css/
│  └─ shan-font.css
├─ fonts/ (8 WOFF2 font files)
├─ server-config/
│  ├─ .htaccess
│  └─ nginx-fonts.conf
├─ .gitattributes
├─ .gitignore
├─ LICENSE
└─ README.md
```

## 🚀 Quick Start
1. Upload the `shan-font` folder to your server (e.g., `/assets/`).
2. Preload only the font weights you actually use.
3. Include the CSS in your `<head>`.

```html
<link rel="preload" as="font" type="font/woff2" href="/assets/shan-font/fonts/Shan-Regular.woff2" crossorigin>
<link rel="stylesheet" href="/assets/shan-font/css/shan-font.css">
<style>
  html { font-family: "Shan", system-ui, sans-serif; }
</style>
```

Use the utility class:
```html
<p class="font-shan">သၢင်ႇတႆး / Shan text sample</p>
```

## ⚡ Performance
- **WOFF2 only** (smallest, fastest)
- `font-display: swap` prevents invisible text
- Unicode ranges target Myanmar/Shan scripts
- Long-term caching (Apache & Nginx configs included)
- Preload selectively for above-the-fold text

## 🔒 Security
- Correct MIME type (`font/woff2`)
- CORS disabled by default (enable only if required)
- Directory listing disabled in `.htaccess`
- **Optional**: Use SRI if loading from CDN

## 📜 License
This project uses the [SIL Open Font License](LICENSE).

## 🛠 Maintainer Notes
- To support older browsers, add `.woff` fallbacks.
- For multi-language sites, split into subsets to reduce file size.
- Keep `server-config/` files updated with your domain for CORS if needed.
