# Gatling Cloud Documentation

## Prerequisites:

You need [Go](https://golang.org/doc/install) and [Hugo](https://gohugo.io/getting-started/installing/) installed.

## Setup:

```
hugo mod get -u 
hugo mod npm pack
npm install
```

## Development server:

```
hugo server -D --debug --baseURL="http://localhost:1313/docs/cloud/"
```

## Troubleshooting

### Check theme version

Theme version, defined in `go.mod`, should match a commit on Gatling theme [git repository](https://github.com/gatling/gatling.io-doc-theme/commits/main/) (`main` branch) :
```
require github.com/gatling/gatling.io-doc-theme v0.0.0-AAAMMJJHHmmSS-<first 12 chars of commit id>
```

### Update Hugo

If you continue to encouter errors, check your installed Hugo version and update it if possible.

Check [official documentation](https://gohugo.io/installation/linux/) for details.

### Extract shortcode Error

In case of issue such as `failed to extract shortcode: template for shortcode "img" not found`, run `hugo mod clean`.
