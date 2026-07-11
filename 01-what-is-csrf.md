# What CSRF is

Cross-site request forgery (CSRF) is an attack where a malicious page causes a victim's browser to send an authenticated request to a site the victim is already logged into, without the victim intending it. The attacker never sees the response or steals the session; they just get an action performed under the victim's identity.

## The mechanism

The attack works because browsers attach a site's cookies to every request to that site automatically, no matter what page triggered the request. Suppose you are logged into bank.example and your session lives in a cookie. If you visit evil.example while that session is active, a hidden form on evil.example can POST to bank.example/transfer, and your browser dutifully attaches your bank cookie. The bank sees a valid session and processes the transfer. Your credentials were never stolen; they were simply along for the ride.

## A concrete example

A classic CSRF payload is a form that submits itself the moment the page loads:

```html
<form action="https://bank.example/transfer" method="POST" id="f">
  <input type="hidden" name="to" value="attacker">
  <input type="hidden" name="amount" value="5000">
</form>
<script>document.getElementById('f').submit();</script>
```

The victim only had to open the attacker's page while logged into the bank. Even a plain image tag pointing at a GET-based state-changing URL can do it, which is one reason GET requests should never change state.

## Why it happens

CSRF exists because cookie-based authentication is ambient: the browser proves who you are on every request without asking whether you meant to make it. The server cannot tell, from the cookie alone, whether the request came from your real interaction with its own pages or from a forged request on someone else's page. The defenses in the next file all boil down to adding proof of intent that a cross-site attacker cannot forge.
