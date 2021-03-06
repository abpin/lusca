# lusca

[![Build Status](https://travis-ci.org/paypal/lusca.png?branch=master)](https://travis-ci.org/paypal/lusca)
[![NPM version](https://badge.fury.io/js/lusca.png)](http://badge.fury.io/js/lusca)

Application security for express.

# methods

```js
var express = require('express'),
	appsec = require('lusca'),
	server = express();

server.use(appsec.csrf());
server.use(appsec.csp({ /* ... */}));
server.use(appsec.xframe('SAMEORIGIN'));
server.use(appsec.p3p('ABCDEF'));
server.use(appsec.hsts({maxAge: 31536000});
```

Or you can opt in to all purely by config:

```js
server.use(appsec({
    csrf: true,
    csp: { /* ... */},
    xframe: 'SAMEORIGIN',
    p3p: 'ABCDEF',
    hsts: {maxAge: 31536000, includeSubDomains: true}
}));
```

# appsec.csrf()

Enables [Cross Site Request Forgery](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)) (CSRF) headers.

If enabled, the CSRF token must be in the payload when modifying data or you will receive a *403 Forbidden*. To send the token you'll need to echo back the `_csrf` value you received from the previous request.


# appsec.csp(options)

* `options.policy` Object - Object definition of policy.
* `options.reportOnly` boolean - Enable report only mode.
* `options.reportUri` String - URI where to send the report data

Enables [Content Security Policy](https://www.owasp.org/index.php/Content_Security_Policy) (CSP) headers.



# appsec.xframe(value)

* `value` String - The value for the header, e.g. one of DENY, SAMEORIGIN or ALLOW-FROM uri.

Enables X-FRAME-OPTIONS headers to help prevent [Clickjacking](https://www.owasp.org/index.php/Clickjacking).



# appsec.p3p(value)

* `value` String - The compact privacy policy.

Enables [Platform for Privacy Preferences Project](http://support.microsoft.com/kb/290333) (P3P) headers.

# appsec.hsts(options)

* `options.maxAge` Number - Required, number of seconds HSTS is in effect. A value of zero cancels HSTS for the domain.
* `options.includeSubDomains` boolean - If provided and true, apply HSTS to all subdomains of this host

Enables [HTTP Strict Transport Security](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security) for the host domain.
