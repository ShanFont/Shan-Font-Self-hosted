# Self-hosting Shan Font

![Shan Font Self-Hosting](https://shanfont.com/wp-content/uploads/2025/07/shan-font-on-self-hosting.jpg)

Self-hosting **Shan** webfonts includes optimized WOFF2 fonts, CSS with Unicode ranges, and server configurations for optimal caching and security.

**Download:** [shan-font-hosting.1.0.0.zip](https://github.com/ShanFont/Shan-Font-Self-hosted/releases/download/Shan/shan-font-hosting.1.0.0.zip)

## Features

- **8 Complete Font Variants**: Thin, Regular, Bold, Black with italic versions
- **Performance Optimized**: WOFF2 format with font-display: swap
- **Unicode Range Targeting**: Optimized for Myanmar/Shan scripts
- **Server Configurations**: Apache and Nginx configs included
- **Security Ready**: Proper MIME types and CORS settings
- **Production Ready**: Long-term caching and performance optimization

## File Structure

```
shan-font/
├── css/
│   └── shan-font.css
├── fonts/
│   ├── Shan-Thin.woff2
│   ├── Shan-ThinItalic.woff2
│   ├── Shan-Regular.woff2
│   ├── Shan-Italic.woff2
│   ├── Shan-Bold.woff2
│   ├── Shan-BoldItalic.woff2
│   ├── Shan-Black.woff2
│   └── Shan-BlackItalic.woff2
├── server-config/
│   ├── .htaccess
│   └── nginx-fonts.conf
├── .gitattributes
├── .gitignore
├── LICENSE
└── README.md
```

## Quick Start

### Download and Setup

1. **Download the package**: [shan-font-hosting.1.0.0.zip](https://github.com/ShanFont/Shan-Font-Self-hosted/releases/download/Shan/shan-font-hosting.1.0.0.zip)
2. **Extract the files** to your project directory
3. **Upload to your server** (e.g., `/assets/shan-font/`)
4. **Configure your server** using the included config files
5. **Implement in your website** following the examples below

### Basic Implementation

1. **Upload the package** to your server (e.g., `/assets/shan-font/`)
2. **Preload critical fonts** (only the weights you use above-the-fold)
3. **Include CSS** in your document head
4. **Apply fonts** to your content

```html
<!DOCTYPE html>
<html>
<head>
  <!-- Preload only the fonts you need immediately -->
  <link rel="preload" as="font" type="font/woff2" 
        href="/assets/shan-font/fonts/Shan-Regular.woff2" crossorigin>
  <link rel="preload" as="font" type="font/woff2" 
        href="/assets/shan-font/fonts/Shan-Bold.woff2" crossorigin>
  
  <!-- Include the CSS -->
  <link rel="stylesheet" href="/assets/shan-font/css/shan-font.css">
  
  <!-- Apply fonts -->
  <style>
    html { 
      font-family: "Shan", system-ui, sans-serif; 
    }
  </style>
</head>
<body>
  <h1 class="font-shan">သၢင်ႇတႆး</h1>
  <p class="font-shan">Shan text sample with beautiful typography</p>
</body>
</html>
```

### CSS Usage

The package includes a utility class for easy application:

```css
/* Apply Shan font to any element */
.font-shan {
  font-family: "Shan", system-ui, sans-serif;
}

/* Or use directly in your CSS */
.my-shan-text {
  font-family: "Shan", system-ui, sans-serif;
  font-weight: 400; /* Regular */
}

.my-bold-shan {
  font-family: "Shan", system-ui, sans-serif;
  font-weight: 700; /* Bold */
}
```

### Font Weights Available

| Weight | CSS Value | Font File |
|--------|-----------|-----------|
| Thin | 100 | Shan-Thin.woff2 |
| Thin Italic | 100 italic | Shan-ThinItalic.woff2 |
| Regular | 400 | Shan-Regular.woff2 |
| Italic | 400 italic | Shan-Italic.woff2 |
| Bold | 700 | Shan-Bold.woff2 |
| Bold Italic | 700 italic | Shan-BoldItalic.woff2 |
| Black | 900 | Shan-Black.woff2 |
| Black Italic | 900 italic | Shan-BlackItalic.woff2 |

## Server Configuration

### Apache (.htaccess)

The included `.htaccess` file provides:

```apache
# Font MIME types
AddType font/woff2 .woff2

# Long-term caching for fonts
<FilesMatch "\.(woff2)$">
  ExpiresActive on
  ExpiresDefault "access plus 1 year"
  Header set Cache-Control "public, immutable"
</FilesMatch>

# Security headers
Header set X-Content-Type-Options nosniff

# Disable directory browsing
Options -Indexes
```

### Nginx Configuration

Include the provided `nginx-fonts.conf` in your server block:

```nginx
# Font handling
location ~* \.(woff2)$ {
  add_header Cache-Control "public, immutable";
  add_header X-Content-Type-Options nosniff;
  expires 1y;
  access_log off;
}

# Correct MIME type for WOFF2
location ~* \.woff2$ {
  add_header Content-Type font/woff2;
}
```

## Performance Optimization

### Preloading Strategy

**Best Practice**: Only preload fonts used above-the-fold

```html
<!-- Good: Preload only critical fonts -->
<link rel="preload" as="font" type="font/woff2" 
      href="/assets/shan-font/fonts/Shan-Regular.woff2" crossorigin>

<!-- Avoid: Preloading all fonts (slows initial load) -->
```

### Font Loading Performance

- **WOFF2 Format**: 30% smaller than WOFF, supported by 95%+ browsers
- **font-display: swap**: Prevents invisible text during font load
- **Unicode Ranges**: Only loads for Myanmar/Shan script content
- **Long-term Caching**: Fonts cached for 1 year with immutable flag

### Loading Optimization Tips

1. **Prioritize by usage**: Preload Regular and Bold first
2. **Lazy load decorative weights**: Load Thin/Black when needed
3. **Use font-display: swap**: Already included in CSS
4. **Enable compression**: Gzip/Brotli on your server
5. **Monitor performance**: Check Core Web Vitals impact

## Security Considerations

### CORS Configuration

Fonts are served without CORS by default. Enable only if loading from external domains:

```apache
# Apache: Enable CORS for specific domains
<FilesMatch "\.(woff2)$">
  Header set Access-Control-Allow-Origin "https://yourdomain.com"
</FilesMatch>
```

```nginx
# Nginx: Enable CORS for specific domains
location ~* \.(woff2)$ {
  add_header Access-Control-Allow-Origin "https://yourdomain.com";
}
```

### Content Security Policy

Add fonts to your CSP if using strict policies:

```html
<meta http-equiv="Content-Security-Policy" 
      content="font-src 'self' https://yourdomain.com;">
```

## Browser Support

- **Primary Support**: Modern browsers with WOFF2 (Chrome 36+, Firefox 39+, Safari 12+, Edge 79+)
- **Fallback Strategy**: System fonts automatically used if WOFF2 fails
- **Coverage**: 95%+ of global browser usage

### Adding WOFF Fallback (Optional)

For broader browser support, add WOFF files and update CSS:

```css
@font-face {
  font-family: 'Shan';
  src: url('fonts/Shan-Regular.woff2') format('woff2'),
       url('fonts/Shan-Regular.woff') format('woff');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}
```

## Advanced Usage

### Subset Loading for Multi-language Sites

For sites with mixed content, consider creating subsets:

```css
/* Myanmar/Shan subset */
@font-face {
  font-family: 'Shan';
  src: url('fonts/Shan-Regular-Myanmar.woff2') format('woff2');
  unicode-range: U+1000-109F, U+AA60-AA7F, U+A9E0-A9FF;
}

/* Latin subset */
@font-face {
  font-family: 'Shan';
  src: url('fonts/Shan-Regular-Latin.woff2') format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153;
}
```

### CDN Alternative

If you prefer CDN hosting, consider these alternatives:
- **jsDelivr**: `https://cdn.jsdelivr.net/gh/ShanFont/ShanFont@main/shan.css`
- **GitHub Releases**: Download from [GitHub Releases](https://github.com/ShanFont/Shan-Font-Self-hosted/releases)
- **Custom CDN**: Upload the downloaded package to your preferred CDN service

## Troubleshooting

### Common Issues

**Fonts not loading:**
1. Check file paths in CSS match your server structure
2. Verify MIME types are configured correctly
3. Ensure CORS settings if loading cross-domain
4. Check browser developer tools for network errors

**Performance issues:**
1. Don't preload all font weights - only critical ones
2. Ensure server compression (Gzip/Brotli) is enabled
3. Verify long-term caching headers are working
4. Monitor loading performance with PageSpeed Insights

**Display issues:**
1. Ensure proper fallback fonts are specified
2. Check font-display: swap is working
3. Verify Unicode ranges match your content
4. Test across different browsers and devices

## Changelog

### Version 1.0.0
- Initial release with 8 WOFF2 font variants
- Complete CSS with Unicode ranges and font-display: swap
- Apache and Nginx server configurations
- Production-ready performance optimizations
- Security headers and CORS configuration options

## License

This project uses the [SIL Open Font License](LICENSE) for maximum compatibility and redistribution freedom.

## Support

- **Issues**: Report problems via the project repository
- **Documentation**: Additional guides available at project website
- **Community**: Join discussions in project forums

**Developed by TaiDev** | **© 2025 All Rights Reserved**
