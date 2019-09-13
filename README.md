<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/835857/14581711/ba623018-0436-11e6-8fce-d2ccd4d379c9.gif">
</p>

# JavaScript Cookie [![Build Status](https://travis-ci.org/js-cookie/js-cookie.svg?branch=master)](https://travis-ci.org/js-cookie/js-cookie) [![BrowserStack Status](https://www.browserstack.com/automate/badge.svg?badge_key=b3VDaHAxVDg0NDdCRmtUOWg0SlQzK2NsRVhWTjlDQS9qdGJoak1GMzJiVT0tLVhwZHNvdGRoY284YVRrRnI3eU1JTnc9PQ==--5e88ffb3ca116001d7ef2cfb97a4128ac31174c2)](https://www.browserstack.com/automate/public-build/b3VDaHAxVDg0NDdCRmtUOWg0SlQzK2NsRVhWTjlDQS9qdGJoak1GMzJiVT0tLVhwZHNvdGRoY284YVRrRnI3eU1JTnc9PQ==--5e88ffb3ca116001d7ef2cfb97a4128ac31174c2) [![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com) [![Code Climate](https://codeclimate.com/github/js-cookie/js-cookie.svg)](https://codeclimate.com/github/js-cookie/js-cookie) [![jsDelivr Hits](https://data.jsdelivr.com/v1/package/npm/js-cookie/badge?style=rounded)](https://www.jsdelivr.com/package/npm/js-cookie)

A simple, lightweight JavaScript API for handling cookies

- Works in [all](https://www.browserstack.com/automate/public-build/b3VDaHAxVDg0NDdCRmtUOWg0SlQzK2NsRVhWTjlDQS9qdGJoak1GMzJiVT0tLVhwZHNvdGRoY284YVRrRnI3eU1JTnc9PQ==--5e88ffb3ca116001d7ef2cfb97a4128ac31174c2) browsers
- Accepts [any](#encoding) character
- [Heavily](test) tested
- No dependency
- [Unobtrusive](#json) JSON support
- Supports ES modules
- Supports AMD/CommonJS
- [RFC 6265](https://tools.ietf.org/html/rfc6265) compliant
- Useful [Wiki](https://github.com/js-cookie/js-cookie/wiki)
- Enable [custom encoding/decoding](#converters)
- **~900 bytes** gzipped!

**If you're viewing this at https://github.com/js-cookie/js-cookie, you're reading the documentation for the master branch.
[View documentation for the latest release.](https://github.com/js-cookie/js-cookie/tree/latest#readme)**

## Installation

### NPM

JavaScript Cookie supports [npm](https://www.npmjs.com/package/js-cookie) under the name `js-cookie`.

```
$ npm install js-cookie --save
```

### Direct download

The source comes as an ES module. If you download it [here](https://github.com/js-cookie/js-cookie/blob/latest/src/js.cookie.mjs) directly, you must include it as such.

Example:

```html
<script type="module" src="./js.cookie.mjs"></script>
<script type="module">
  import Cookies from './js.cookie.mjs'

  Cookies.set('foo', 'bar')
</script>
```

_Not all browsers support ES modules natively yet_. For this reason the npm package/release
comes with both an ES module as well as an UMD module variant. Include the module along
with the fallback to account for this. You may want to load the nomodule script in a deferred
fashion (modules are deferred by default):

```html
<script type="module" src="/path/to/js.cookie.mjs"></script>
<script nomodule defer src="/path/to/js.cookie.js"></script>
```

Note the different extensions: `.mjs` denotes an ES module.

### CDN

Alternatively, include it via [jsDelivr CDN](https://www.jsdelivr.com/package/npm/js-cookie):

```html
<script src="https://cdn.jsdelivr.net/npm/js-cookie@2/src/js.cookie.min.js"></script>
```

**Never include the source directly from GitHub (http://raw.github.com/...).** The file
is being served as text/plain and as such may be blocked because of the wrong MIME type.
Bottom line: GitHub is not a CDN.

## ES Module

Example for how to import the ES module from another module:

```javascript
import Cookies from './node_modules/js-cookie/dist/js.cookie.mjs'

Cookies.set('foo', 'bar')
```

## Basic Usage

Create a cookie, valid across the entire site:

```javascript
Cookies.set('name', 'value')
```

Create a cookie that expires 7 days from now, valid across the entire site:

```javascript
Cookies.set('name', 'value', { expires: 7 })
```

Create an expiring cookie, valid to the path of the current page:

```javascript
Cookies.set('name', 'value', { expires: 7, path: '' })
```

Read cookie:

```javascript
Cookies.get('name') // => 'value'
Cookies.get('nothing') // => undefined
```

Read all visible cookies:

```javascript
Cookies.get() // => { name: 'value' }
```

_Note: It is not possible to read a particular cookie by passing one of the cookie attributes (which may or may not
have been used when writing the cookie in question):_

```javascript
Cookies.get('foo', { domain: 'sub.example.com' }) // `domain` won't have any effect...!
```

The cookie with the name `foo` will only be available on `.get()` if it's visible from where the
code is called; the domain and/or path attribute will not have an effect when reading.

Delete cookie:

```javascript
Cookies.remove('name')
```

Delete a cookie valid to the path of the current page:

```javascript
Cookies.set('name', 'value', { path: '' })
Cookies.remove('name') // fail!
Cookies.remove('name', { path: '' }) // removed!
```

_IMPORTANT! When deleting a cookie and you're not relying on the [default attributes](#cookie-attributes), you must pass the exact same path and domain attributes that were used to set the cookie:_

```javascript
Cookies.remove('name', { path: '', domain: '.yourdomain.com' })
```

_Note: Removing a nonexistent cookie does not raise any exception nor return any value._

## Namespace conflicts

If there is any danger of a conflict with the namespace `Cookies`, the `noConflict` method will allow you to define a new namespace and preserve the original one. This is especially useful when running the script on third party sites e.g. as part of a widget or SDK.

```javascript
// Assign the js-cookie api to a different variable and restore the original "window.Cookies"
var Cookies2 = Cookies.noConflict()
Cookies2.set('name', 'value')
```

_Note: The `.noConflict` method is not necessary when using AMD or CommonJS, thus it is not exposed in those environments._

## JSON

js-cookie provides unobtrusive JSON storage for cookies.

When creating a cookie you can pass an Array or Object Literal instead of a string in the value. If you do so, js-cookie will store the string representation of the object according to `JSON.stringify`:

```javascript
Cookies.set('name', { foo: 'bar' })
```

When reading a cookie with the default `Cookies.get` api, you receive the string representation stored in the cookie:

```javascript
Cookies.get('name') // => '{"foo":"bar"}'
```

```javascript
Cookies.get() // => { name: '{"foo":"bar"}' }
```

When reading a cookie with the `Cookies.getJSON` api, you receive the parsed representation of the string stored in the cookie according to `JSON.parse`:

```javascript
Cookies.getJSON('name') // => { foo: 'bar' }
```

```javascript
Cookies.getJSON() // => { name: { foo: 'bar' } }
```

## Encoding

This project is [RFC 6265](http://tools.ietf.org/html/rfc6265#section-4.1.1) compliant. All special characters that are not allowed in the cookie-name or cookie-value are encoded with each one's UTF-8 Hex equivalent using [percent-encoding](http://en.wikipedia.org/wiki/Percent-encoding).  
The only character in cookie-name or cookie-value that is allowed and still encoded is the percent `%` character, it is escaped in order to interpret percent input as literal.  
Please note that the default encoding/decoding strategy is meant to be interoperable [only between cookies that are read/written by js-cookie](https://github.com/js-cookie/js-cookie/pull/200#discussion_r63270778). To override the default encoding/decoding strategy you need to use a [converter](#converters).

_Note: According to [RFC 6265](https://tools.ietf.org/html/rfc6265#section-6.1), your cookies may get deleted if they are too big or there are too many cookies in the same domain, [more details here](https://github.com/js-cookie/js-cookie/wiki/Frequently-Asked-Questions#why-are-my-cookies-being-deleted)._

## Cookie Attributes

Cookie attributes defaults can be set globally by setting properties of the `Cookies.defaults` object or individually for each call to `Cookies.set(...)` by passing a plain object in the last argument. Per-call attributes override the default attributes.

### expires

Define when the cookie will be removed. Value can be a [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) which will be interpreted as days from time of creation or a [`Date`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) instance. If omitted, the cookie becomes a session cookie.

To create a cookie that expires in less than a day, you can check the [FAQ on the Wiki](https://github.com/js-cookie/js-cookie/wiki/Frequently-Asked-Questions#expire-cookies-in-less-than-a-day).

**Default:** Cookie is removed when the user closes the browser.

**Examples:**

```javascript
Cookies.set('name', 'value', { expires: 365 })
Cookies.get('name') // => 'value'
Cookies.remove('name')
```

### path

A [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) indicating the path where the cookie is visible.

**Default:** `/`

**Examples:**

```javascript
Cookies.set('name', 'value', { path: '' })
Cookies.get('name') // => 'value'
Cookies.remove('name', { path: '' })
```

**Note regarding Internet Explorer:**

> Due to an obscure bug in the underlying WinINET InternetGetCookie implementation, IE’s document.cookie will not return a cookie if it was set with a path attribute containing a filename.

(From [Internet Explorer Cookie Internals (FAQ)](http://blogs.msdn.com/b/ieinternals/archive/2009/08/20/wininet-ie-cookie-internals-faq.aspx))

This means one cannot set a path using `window.location.pathname` in case such pathname contains a filename like so: `/check.html` (or at least, such cookie cannot be read correctly).

In fact, you should never allow untrusted input to set the cookie attributes or you might be exposed to a [XSS attack](https://github.com/js-cookie/js-cookie/issues/396).

### domain

A [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) indicating a valid domain where the cookie should be visible. The cookie will also be visible to all subdomains.

**Default:** Cookie is visible only to the domain or subdomain of the page where the cookie was created, except for Internet Explorer (see below).

**Examples:**

Assuming a cookie that is being created on `site.com`:

```javascript
Cookies.set('name', 'value', { domain: 'subdomain.site.com' })
Cookies.get('name') // => undefined (need to read at 'subdomain.site.com')
```

**Note regarding Internet Explorer default behavior:**

> Q3: If I don’t specify a DOMAIN attribute (for) a cookie, IE sends it to all nested subdomains anyway?  
> A: Yes, a cookie set on example.com will be sent to sub2.sub1.example.com.  
> Internet Explorer differs from other browsers in this regard.

(From [Internet Explorer Cookie Internals (FAQ)](http://blogs.msdn.com/b/ieinternals/archive/2009/08/20/wininet-ie-cookie-internals-faq.aspx))

This means that if you omit the `domain` attribute, it will be visible for a subdomain in IE.

### secure

Either `true` or `false`, indicating if the cookie transmission requires a secure protocol (https).

**Default:** No secure protocol requirement.

**Examples:**

```javascript
Cookies.set('name', 'value', { secure: true })
Cookies.get('name') // => 'value'
Cookies.remove('name')
```

## Converters

### Read

Create a new instance of the api that overrides the default decoding implementation.  
All get methods that rely in a proper decoding to work, such as `Cookies.get()` and `Cookies.get('name')`, will run the converter first for each cookie.  
The returning String will be used as the cookie value.

Example from reading one of the cookies that can only be decoded using the `escape` function:

```javascript
document.cookie = 'escaped=%u5317'
document.cookie = 'default=%E5%8C%97'
var cookies = Cookies.withConverter(function (value, name) {
  if (name === 'escaped') {
    return unescape(value)
  }
})
cookies.get('escaped') // 北
cookies.get('default') // 北
cookies.get() // { escaped: '北', default: '北' }
```

### Write

Create a new instance of the api that overrides the default encoding implementation:

```javascript
Cookies.withConverter({
  read: function (value, name) {
    // Read converter
  },
  write: function (value, name) {
    // Write converter
  }
})
```

## Server-side integration

Check out the [Servers Docs](SERVER_SIDE.md)

## Contributing

Check out the [Contributing Guidelines](CONTRIBUTING.md)

## Security

For vulnerability reports, send an e-mail to `jscookieproject at gmail dot com`

## Releasing

We are using [release-it](https://www.npmjs.com/package/release-it) for automated releasing.

Start a dry run to see what would happen:

```
$ npm run release minor -- --dry-run
```

Do a real release (publishes both to npm as well as create a new release on GitHub):

```
$ npm run release minor
```

_GitHub releases are created as a draft and need to be published manually!
(This is so we are able to craft suitable release notes before publishing.)_

## Supporters

<p>
  <a href="https://www.browserstack.com/"><img src="https://raw.githubusercontent.com/wiki/js-cookie/js-cookie/Browserstack-logo%402x.png" width="150"></a>
</p>

Many thanks to [BrowserStack](https://www.browserstack.com/) for providing unlimited browser testing free of cost.

## Authors

- [Klaus Hartl](https://github.com/carhartl)
- [Fagner Brack](https://github.com/FagnerMartinsBrack)
- And awesome [contributors](https://github.com/js-cookie/js-cookie/graphs/contributors)
