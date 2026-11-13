# CSRF and XSS

A plain-language reference on the two most common web application attacks: cross-site request forgery (CSRF) and cross-site scripting (XSS). It covers what each attack actually is, how it works step by step, the real defenses that stop it, and how the two attacks differ so you stop confusing them.

## The core idea

CSRF tricks a logged-in user's browser into sending an unwanted authenticated request, while XSS injects attacker-controlled script that runs inside your page. CSRF abuses the trust a site has in the user's browser (cookies get sent automatically), so the fix is proving the request was intended: CSRF tokens and SameSite cookies. XSS abuses the trust a browser has in your page, so the fix is never letting untrusted data become executable markup: output encoding, a Content Security Policy, and avoiding sinks like innerHTML.

## What is inside

- [01-what-is-csrf.md](01-what-is-csrf.md) what CSRF is and the exact mechanism that makes it work.
- [02-defending-against-csrf.md](02-defending-against-csrf.md) CSRF tokens, SameSite cookies, and other real defenses.
- [03-what-is-xss.md](03-what-is-xss.md) the three kinds of XSS and how injected script runs.
- [04-defending-against-xss.md](04-defending-against-xss.md) output encoding, CSP, and avoiding dangerous sinks.
- [05-how-they-differ.md](05-how-they-differ.md) a side-by-side that keeps the two straight.

## The stance

These are old, well-understood attacks with real, boring defenses. The failure is almost never a missing exotic tool; it is a skipped token, a cookie without SameSite, or user input dropped straight into HTML. Apply the standard defenses everywhere, not just where you remember to.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. Copyright RBL Industries LLC 2026.
