+++
title = "Secure"
[menu.side]
  name = "Secure"
  parent = "middleware"
  weight = 5
+++

## Secure Middleware

Secure middleware provides protection against cross-site scripting (XSS) attack,
content type sniffing, clickjacking, insecure connection and other code injection
attacks.

### Configuration

```go
SecureConfig struct {
  // XSSProtection provides protection against cross-site scripting attack (XSS)
  // by setting the `X-XSS-Protection` header.
  // Optional. Default value "1; mode=block".
  XSSProtection string

  // ContentTypeNosniff provides protection against overriding Content-Type
  // header by setting the `X-Content-Type-Options` header.
  // Optional. Default value "nosniff".
  ContentTypeNosniff string

  // XFrameOptions can be used to indicate whether or not a browser should
  // be allowed to render a page in a <frame>, <iframe> or <object> .
  // Sites can use this to avoid clickjacking attacks, by ensuring that their
  // content is not embedded into other sites.provides protection against
  // clickjacking.
  // Optional. Default value "SAMEORIGIN".
  // Possible values:
  // `SAMEORIGIN` - The page can only be displayed in a frame on the same origin as the page itself.
  // `DENY` - The page cannot be displayed in a frame, regardless of the site attempting to do so.
  // `ALLOW-FROM uri` - The page can only be displayed in a frame on the specified origin.
  XFrameOptions string

  // HSTSMaxAge sets the `Strict-Transport-Security` header to indicate how
  // long (in seconds) browsers should remember that this site is only to
  // be accessed using HTTPS. This reduces your exposure to some SSL-stripping
  // man-in-the-middle (MITM) attacks.
  // Optional. Default value 0.
  HSTSMaxAge int

  // HSTSExcludeSubdomains won't include subdomains tag in the `Strict Transport Security`
  // header, excluding all subdomains from security policy. It has no effect
  // unless HSTSMaxAge is set to a non-zero value.
  // Optional. Default value false.
  HSTSExcludeSubdomains bool

  // ContentSecurityPolicy sets the `Content-Security-Policy` header providing
  // security against cross-site scripting (XSS), clickjacking and other code
  // injection attacks resulting from execution of malicious content in the
  // trusted web page context.
  // Optional. Default value "".
  ContentSecurityPolicy string
}
```

### Default Configuration

```go
DefaultSecureConfig = SecureConfig{
  XSSProtection:      "1; mode=block",
  ContentTypeNosniff: "nosniff",
  XFrameOptions:      "SAMEORIGIN",
}
```

*Usage*

`e.Use(middleware.Secure())`

### Custom Configuration

*Usage*

```go
e := echo.New()
e.Use(middleware.SecureWithConfig(middleware.SecureConfig{
	XSSProtection:         "",
	ContentTypeNosniff:    "",
	XFrameOptions:         "",
	HSTSMaxAge:            3600,
	ContentSecurityPolicy: "default-src 'self'",
}))
```

Passing empty `XSSProtection`, `ContentTypeNosniff`, `XFrameOptions` or `ContentSecurityPolicy`
disables that protection.