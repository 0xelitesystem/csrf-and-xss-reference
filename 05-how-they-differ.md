# How CSRF and XSS differ

CSRF and XSS are often named together but are different attacks with different fixes. CSRF makes the victim's browser send a request the victim did not intend; XSS makes the victim's browser run script the attacker wrote. One abuses trust in the user, the other abuses trust in the page.

## The trust each abuses

CSRF abuses the trust a server places in the user's browser: cookies are sent automatically, so a forged request looks authentic. The attacker causes an action but never reads the response. XSS abuses the trust a browser places in your site's content: whatever markup is in the page runs with your origin's privileges, so the attacker can read and change almost anything the user can.

## Different defenses

CSRF defenses prove intent: anti-CSRF tokens and `SameSite` cookies. XSS defenses prevent data from becoming code: output encoding, a Content Security Policy, and avoiding sinks like `innerHTML`. Notice the fixes do not overlap, which is why you need both classes of defense; a site can be immune to one and wide open to the other.

## A quick side-by-side

| Aspect | CSRF | XSS |
|---|---|---|
| What runs | A forged authenticated request | Attacker's script in your page |
| Trust abused | Server trusts the browser's cookies | Browser trusts the page's markup |
| Attacker sees response | No | Yes |
| Primary fix | Tokens and SameSite cookies | Output encoding and CSP |

## Why XSS can defeat CSRF defenses

The relationship matters: XSS is the more powerful bug, and it can undermine CSRF protection. If an attacker can run script in your origin, they can simply read the anti-CSRF token out of the page and submit a valid request. So fixing XSS is a prerequisite; strong CSRF defenses assume no script injection. Treat XSS as the higher priority, and defend against both.
