# What XSS is

Cross-site scripting (XSS) is an attack where untrusted data is inserted into a web page in a way that lets it run as script in other users' browsers. Once attacker script runs in your page's origin, it can read cookies (unless they are HttpOnly), read the DOM, make requests as the user, and rewrite what the page shows.

## How injection becomes execution

XSS happens when user-supplied input is placed into the page without being treated as plain data. If a comment field value of `<script>alert(1)</script>` is written straight into the HTML, the browser parses it as a real script tag and runs it. The browser cannot tell attacker markup from legitimate markup; it just executes what is in the document. The whole class of bugs is data crossing the line into code.

## The three kinds

Stored XSS: the malicious input is saved on the server (a comment, a profile name) and served to everyone who views that content later. This is the most damaging because it hits many victims automatically.

Reflected XSS: the input is echoed back immediately in the response, for example a search term shown on the results page. The attacker delivers it through a crafted link the victim clicks.

DOM-based XSS: the vulnerability is entirely in client-side JavaScript that takes attacker-controlled values (like `location.hash`) and writes them into the page. The dangerous data may never touch the server.

## An example sink

A common DOM XSS sink is assigning untrusted text to `innerHTML`:

```js
element.innerHTML = location.hash.slice(1); // attacker controls the hash
```

A URL ending in `#<img src=x onerror=alert(document.cookie)>` then runs script. The same danger applies to `document.write`, `eval`, and inline event handlers built from input. The fix, covered next, is to treat all such data as text and to constrain what can execute at all.
