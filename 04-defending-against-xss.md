# Defending against XSS

You stop XSS by making sure untrusted data can never become executable markup, and by limiting what could run even if some slips through. The core defense is context-aware output encoding; a Content Security Policy and avoiding dangerous sinks are the strong backups.

## Output encoding (the main defense)

Encode data for the exact context where it lands, at the moment it is written into the page. In HTML body context, turn `<` into `&lt;`, `>` into `&gt;`, `&` into `&amp;`, and `"` into `&quot;`, so the browser shows the characters instead of parsing them as markup. So `<script>` becomes the literal text `&lt;script&gt;` and never runs. Different contexts need different encoding: HTML attributes, JavaScript, URLs, and CSS each have their own rules, which is why you rely on a framework's context-aware escaping rather than hand-rolling it.

## Use the framework and safe APIs

Modern templating engines (React, Angular, and server-side templates with auto-escaping) encode by default. Prefer text-setting APIs over markup-setting ones: use `textContent` instead of `innerHTML`, and `setAttribute` over building attribute strings. When you genuinely must render user HTML (a rich-text comment), run it through a vetted sanitizer such as DOMPurify rather than trusting it.

## Content Security Policy

A CSP is an HTTP header that tells the browser which sources of script are allowed, so injected inline script is refused even if it reaches the page. An example header:

```
Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none'
```

This blocks inline `<script>` and event handlers and only permits scripts from your own origin. Add HttpOnly to session cookies so that even successful XSS cannot read them via `document.cookie`. CSP is defense in depth: it reduces the damage of a bug, it does not excuse skipping encoding.
