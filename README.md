# markdown-it-incremental-dom

[![Build Status](https://travis-ci.org/yhatt/markdown-it-incremental-dom.svg?branch=master)](https://travis-ci.org/yhatt/markdown-it-incremental-dom)

A [markdown-it](https://github.com/markdown-it/markdown-it) renderer plugin by using [Incremental DOM](https://github.com/google/incremental-dom).

Let's see key features: **[https://yhatt.github.io/markdown-it-incremental-dom/](https://yhatt.github.io/markdown-it-incremental-dom/)** or [`docs.md`](docs/docs.md)

[![](./docs/images/repainting-incremental-dom.gif)](https://yhatt.github.io/markdown-it-incremental-dom/)

## Requirement

- [markdown-it](https://github.com/markdown-it/markdown-it) >= 4.0.0 (Recommend latest version >= 8.2.2, that this plugin use it)
- [Incremental DOM](https://github.com/google/incremental-dom) >= 0.5.x

## Examples

### Node.js

```javascript
const IncrementalDOM = require('incremental-dom')

const md = require('markdown-it')()
             .use(require('markdown-it-incremental-dom'), IncrementalDOM)

IncrementalDOM.patch(
  document.getElementById('target'),
  md.renderToIncrementalDOM('# Hello, Incremental DOM!')
)
```

### Browser

Define as `window.markdownitIncrementalDOM`.

```html
<!DOCTYPE HTML>
<html>
<head>
  <script src="https://ajax.googleapis.com/ajax/libs/incrementaldom/0.5.1/incremental-dom.js"></script>
  <script src="https://cdn.jsdelivr.net/markdown-it/8.2.2/markdown-it.min.js"></script>
  <script src="./node_modules/markdown-it-incremental-dom/dist/markdown-it-incremental-dom.js"></script>
</head>
<body>
  <div id="target"></div>

  <script>
    var md = new markdownit().use(markdownitIncrementalDOM)

    IncrementalDOM.patch(
      document.getElementById('target'),
      md.renderToIncrementalDOM('# Hello, Incremental DOM!')
    )
  </script>
</body>
</html>
```

## Installation

We recommend using [yarn](https://yarnpkg.com/) to install.

```bash
$ yarn add markdown-it-incremental-dom
```

If you wanna use npm, try this:

```bash
$ npm install markdown-it-incremental-dom --save
```

## Usage

When injecting this plugin by `.use()`, *you should pass Incremental DOM class as second argument.* (`window.IncrementalDOM` by default)

```javascript
const IncrementalDOM = require('incremental-dom')
const md = require('markdown-it')()
             .use(require('markdown-it-incremental-dom'), IncrementalDOM)
```

If it is succeed, 2 new rendering methods would be injected to instance.

### `MarkdownIt.renderToIncrementalDOM(src[, env]) => Function`

Similar to [`MarkdownIt.render(src[, env])`](https://markdown-it.github.io/markdown-it/#MarkdownIt.render) but *it returns a function for Incremental DOM*. It means doesn't render Markdown immediately.

You must render to DOM by using [`IncrementalDOM.patch(node, description)`](http://google.github.io/incremental-dom/#api/patch). Please pass the returned function to the description argument. For example:

```javascript
const node = document.getElementById('#target')
const func = md.renderToIncrementalDOM('# Hello, Incremental DOM!')

// It would render "<h1>Hello, Incremental DOM!</h1>" to <div id="target">
IncrementalDOM.patch(node, func)
```

### `MarkdownIt.renderInlineToIncrementalDOM(src[, env]) => Function`

Similar to `MarkdownIt.renderToIncrementalDOM` but it wraps [`MarkdownIt.renderInline(src[, env])`](https://markdown-it.github.io/markdown-it/#MarkdownIt.renderInline).

## Development

```bash
$ git clone https://github.com/yhatt/markdown-it-incremental-dom

$ yarn install
$ yarn build
```

### Publish to npm

```bash
$ npm publish
```

:warning: ***CAUTION:*** You should not use `yarn publish` when you would publish to npm. It would cause publishing wrong / empty build because the building process on `prepublish` will run after packaging.

## Author

Yuki Hattori ([@yhatt](https://github.com/yhatt/))

## License

This plugin releases under the [MIT License](https://github.com/yhatt/markdown-it-incremental-dom/blob/master/LICENSE).
