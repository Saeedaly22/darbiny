---
name: darbiny
description: Use when working on the darbiny website (darbaini.com) - a static Arabic RTL landing page for a Saudi female driving school. Covers file structure, performance optimization history, SVG icons, and deployment to Vercel.
---

# مشروع دربيني

موقع تعريفي لتطبيق "دربيني" - منصة سعودية لتعليم القيادة للنساء.

## التقنيات
- HTML + CSS + JavaScript ثابت (Static) — بدون إطارات أو build pipeline
- Arabic RTL, `dir="rtl"` + `lang="ar"`
- Google Fonts: Cairo
- Vercel deployment from `darbiny/` subdirectory

## هيكل الملفات
```
.git/
.gitignore
vercel.json
.opencode/
  skills/
    darbiny/SKILL.md
darbiny/
  index.html      ← الصفحة الرئيسية
  privacy.html    ← صفحة سياسة الخصوصية
  style.css       ← 35KB, render-blocking
  script.js       ← 8KB, deferred في <head>
  robots.txt
  sitemap.xml
  media/
    Logo.gif              ← 39KB (LCP element)
    driving-preview.mp4   ← 1.8MB (H.264)
    videoCover.webp       ← poster
    img{1,2,3}.{webp,jpg} ← معرض الصور (WebP + JPEG fallback)
    app-store-qr.{webp,jpeg}
```

## الأيقونات
- **SVG sprite مضمّن** (ليس Font Awesome CDN) بعد `<body>` مباشرة
- كل أيقونة عبارة عن `<symbol>` في `<svg style="display:none">`
- تستخدم مع `<svg class="icon fa-NAME"><use href="#fa-NAME"/></svg>`
- 30 symbol (53 استخداماً)

## قرارات الأداء
- **CSS** متزامن (render-blocking) — عدم استخدام `media="print"` أو `preload` لأنه يخفض الدرقة
- **Font Preload**: `<link rel="preload" as="font">` للـ woff2 قبل Google Fonts stylesheet
- **LCP Preload**: `<link rel="preload" as="image" fetchpriority="high">` لـ Logo.gif
- **Script defer**: `<script defer src="script.js">` في `<head>` (ليس نهاية body)
- **rAF**: `handleNavbar()` يستخدم `requestAnimationFrame` لتجنب forced reflow
- **Favicon**: `media/Logo.gif` (وليس inline SVG)
- **التقييم**: كان 91 قبل GA/GTM. الآن بدون GA/GTM متوقع أعلى

## SEO
- OG / Twitter Card meta tags
- JSON-LD: Organization, WebSite, Course ItemList
- `robots.txt` + `sitemap.xml`

## Vercel
- Domain: darbaini.com
- Root directory في Vercel: `darbiny/` (أو عبر `vercel.json` بقاعدة المشروع)
- الـ `vercel.json` عشان يخدم من `darbiny/`

## الصور
- WebP + JPEG fallback في `<picture>` tags
- الـ GIF (Logo.gif) يستخدم مباشرة
