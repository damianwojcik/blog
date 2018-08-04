---
title: Progressive Web Apps
date: 2018-07-01 18:40:34
tags: #pwa #optimization
---
This post covers everything I have recently learned about PWA. It is also a kind of checklist and list of procedures, how to properly transform a page into PWA.
By steps performed in this Post, my webpage got 100% in Lighthouse PWA Audit.

![Lighthouse PWA Result](lighthouse.jpg)

## FEATURES
- Invented by Google
- Uses HTTPS protocol
- Might (but not MUST) be served OFFLINE
- Not fully supported by iOS (yet)
- Fully responsive and cross-browser
- Fully optimized, loading fast even on 3G
- Reactive - fast UX, fast view changing
- Allows push notifications

## CHECKLIST
### SSL
### METADATA
- Use [RealFaviconGenerator](https://realfavicongenerator.net/) with at least 512x512 image
- Inside <code>&lt;head&gt;</code>:
``` html
	<meta name="theme-color" content="#ffffff">
	<meta name="description" content="Description here">
```

### 'MANIFEST.JSON'
<code>manifest.json</code> is a file inside <code>'./'</code> which describe app and allows for Add to Home Screen (android).
- Inside <code>&lt;head&gt;</code>:
``` html
	<link rel="manifest" href="manifest.json">
```
- My <code>manifest.json</code>:
``` javascript
{
	"short_name": "DW",
	"name": "DamianWojcik.IT",
	"icons": [
		{
			"src": "/android-chrome-192x192.png",
			"sizes": "192x192",
			"type": "image/png"
		},
		{
			"src": "/android-chrome-512x512.png",
			"sizes": "512x512",
			"type": "image/png"
		}
	],
	"start_url": "/?utm_source=homescreen",
	"background_color": "#000000",
	"theme_color": "#FFFFFF",
	"display": "standalone"
}
```
- [Google walkthrough](https://codelabs.developers.google.com/codelabs/add-to-home-screen/#0)
- [Manifest validator tool](https://manifest-validator.appspot.com/)

### SERVICE-WORKER
The location of the service worker is important! For security reasons, a service worker can only control the pages that are in its same directory or its subdirectories. This means that if you place the service worker file in a scripts directory it will only be able to interact with pages in the scripts directory or below.

A service worker is a background script that intercepts network requests made by your web app. It can use the Cache Storage API to respond to those requests. Is so called **third-layer** between server and browser.

#### STRATEGIES
- cache-first
- network-first

#### TOOLS
- sw-precache
- sw-toolbox
- workbox

[Example](https://gist.github.com/khamian/413d350fc5d87931fd73cd928728c60a#file-service-worker-js)

### EACH PAGE HAS URL

## SSL IMPLEMENTATION
### - CloudFlare
### - Letsencrypt ([zeroSSL](https://zerossl.com/))
1. Walkthrough, download and backup all generated files,
2. Hosting *Cpanel -> SSL -> ADD CRT, ADD KEY, INSTALL*,
3. Activation.
	- For Wordpress: 
		* [Really Simple SSL](https://pl.wordpress.org/plugins/really-simple-ssl/) plugin,
		* Refresh direct links
	- For static page, edit *.htaccess*
	``` php
		# BEGIN Force SSL
		# This should be the first rule before other rules
		<IfModule mod_rewrite.c>
		RewriteEngine On
		RewriteCond %{HTTPS} !=on
		RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
		</IfModule>
		# END Force SSL
	```

4. Google Search Console and Google Analytics.
- Add all four webiste links to [Google Search Console](https://www.google.com/webmasters/tools/)
- (Optionally) create a set in GSC,
- Set cannonical URL,
- In Google Analytics change urls to https://
- Connect [GA](https://www.google.com/analytics/) to GSC

5. Remember to refresh CRT every 3 months

## AUDITS AND PERFORMANCE
### AUDITS
- Lighthouse
	* npm:
		lighthouse https://damianwojcik.it/ --view
	* chrome dev tools (F12)
- [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)
- [GTmetrix](https://gtmetrix.com/)
- [Vavry](https://varvy.com/pagespeed/)

### OPTIMIZATION
- Concat -> Minify all CSS/JS (Gulp/Webpack/Online tools)
- Async load CSS/JS
``` html
	<link rel="stylesheet" href="/styles/main.css" media="none" onload="if(media!='all')media='all'">
	<script async src="assets/js/scripts.min.js"></script>
```
- Use conditional loading for CSS media queries (mobile first FTW)
- Minify HTML
- Optimized and responsive images (srcset)
- Update libraries and frameworks (eg. jQuery)
- For open new window links use target="_blank" rel="noopener"
- Use lazy load ([bLazy](http://dinbror.dk/blog/blazy/)) for images,
- Text contrast compilant with WCAG 2.1 - [Lea Verou tool](http://contrast-ratio.com/)
- Remove unused CSS - [npm css-purge](https://www.npmjs.com/package/css-purge)
- Avoid [render blocking CSS](https://varvy.com/pagespeed/render-blocking-css.html)
- Use critical CSS for above-the-fold (before scrolling) - [npm critical](https://www.npmjs.com/package/critical)
- Async conditional load below-the-fold media queries
- Nextgen .WebP images with fallback
	* [Google Developers](https://developers.google.com/speed/webp/)
	* [Css-tricks](https://css-tricks.com/using-webp-images/)
- Cache files (Wordpress plugin or .htaccess rules)
- [Barba.js](http://barbajs.org/) for pre-fetching
- Lazy load (async) Google Fonts, GA scripts, Google Maps:
``` html
	<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Fira+Sans:400,500,300&subset=latin,latin-ext" media="none" onload="if(media!='all')media='all'">
```
- Push notifications - [OneSignal](https://onesignal.com/)
- [AMP](https://www.ampproject.org/)

<br>

---

**SOURCES:**

<https://en.wikipedia.org/wiki/Progressive_Web_Apps>
<https://developers.google.com/web/progressive-web-apps/>
<https://piecioshka.pl/blog/2017/05/07/jak-przerobic-strone-na-pwa.html>
<https://developers.google.com/web/progressive-web-apps/checklist>
<https://developers.google.com/web/fundamentals/primers/service-workers/>
<https://www.biggerpicture.agency/insights/guide-to-website-performance-optimisation>
<https://www.biggerpicture.agency/insights/feedgist-a-progressive-web-app-case-study>
<https://syntax.fm/show/050/progressive-web-apps>
<https://pwa.rocks/>