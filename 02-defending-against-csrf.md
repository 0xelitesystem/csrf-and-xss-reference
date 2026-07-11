# Defending against CSRF

You stop CSRF by requiring proof that a state-changing request was intended, proof that an attacker on another origin cannot supply. The two primary defenses are anti-CSRF tokens and SameSite cookies, and using both is the safe default.

## Anti-CSRF tokens

The server generates a random, unpredictable token tied to the user's session and embeds it in every form (or sends it for the client to include in a header). On submission, the server checks that the token matches. A cross-site attacker cannot read the token (the same-origin policy blocks that), so they cannot forge a valid request.

```html
<input type="hidden" name="csrf_token" value="a1b2c3d4e5f6...">
```

This is the synchronizer token pattern. A stateless variant is the double-submit cookie: send the token both as a cookie and as a form field or header, and require the two to match.

## SameSite cookies

Setting the `SameSite` attribute on your session cookie tells the browser not to send it on cross-site requests. An example session cookie:

```
Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Lax
```

`SameSite=Lax` (a common default now) blocks the cookie on cross-site POSTs and background requests while still allowing top-level navigations. `SameSite=Strict` is tighter but can log users out when they follow inbound links. SameSite is strong but should back up tokens, not replace them, since older browsers and some edge cases weaken it.

## Supporting practices

Never let GET requests change state; reserve GET for reads so an image or link cannot trigger an action. For sensitive operations, re-check with a header the browser sets and script cannot spoof (`Origin` or `Referer`), and consider re-authentication. Custom request headers on JSON APIs also help, because a simple cross-site form cannot set arbitrary headers.
