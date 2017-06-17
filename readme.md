# remark-retext [![Build Status][travis-badge]][travis] [![Coverage Status][codecov-badge]][codecov]

Bridge / mutate from [**remark**][remark] to [**retext**][retext].

## Installation

[npm][]:

```bash
npm install remark-retext
```

## Usage

Say we have the following file, `example.md`:

```markdown
## Hello guys!
```

And our script, `example.js`, looks as follows:

```javascript
var vfile = require('to-vfile');
var report = require('vfile-reporter');
var unified = require('unified');
var parse = require('remark-parse');
var stringify = require('remark-stringify');
var remark2retext = require('remark-retext');
var english = require('retext-english');
var equality = require('retext-equality');

unified()
  .use(parse)
  .use(remark2retext, unified().use(english).use(equality))
  .use(stringify)
  .process(vfile.readSync('example.md'), function (err, file) {
    console.error(report(err || file));
  });
```

Now, running `node example` yields:

```text
example.md
  1:10-1:14  warning  `guys` may be insensitive, use `people`, `persons`, `folks` instead  gals-men  retext-equality

⚠ 1 warning
```

## API

### `origin.use(remark2retext, destination)`

Either bridge or mutate from [**remark**][remark] ([MDAST][]) to
[**retext**][retext] ([NLCST][]).

###### `destination`

`destination` is either a parser or a processor.

If a [`Unified`][processor] processor is given, runs the destination
processor with the new NLCST tree, then, after running discards that
tree and continues on running the origin processor with the original
tree ([bridge-mode][bridge]).

If a parser (such as [**parse-latin**][latin], [**parse-english**][english],
or [**parse-dutch**][dutch]) is given, passes the tree to further
plug-ins (mutate-mode).

## Related

*   [`rehype-retext`](https://github.com/wooorm/rehype-retext)
    — Transform HTML to [NLCST][]
*   [`remark-rehype`](https://github.com/wooorm/remark-rehype)
    — Transform markdown to HTML
*   [`rehype-remark`](https://github.com/wooorm/rehype-remark)
    — Transform HTML to markdown

## License

[MIT][license] © [Titus Wormer][author]

<!-- Definitions -->

[travis-badge]: https://img.shields.io/travis/wooorm/remark-retext.svg

[travis]: https://travis-ci.org/wooorm/remark-retext

[codecov-badge]: https://img.shields.io/codecov/c/github/wooorm/remark-retext.svg

[codecov]: https://codecov.io/github/wooorm/remark-retext

[npm]: https://docs.npmjs.com/cli/install

[license]: LICENSE

[author]: http://wooorm.com

[mdast]: https://github.com/wooorm/mdast

[remark]: https://github.com/wooorm/remark

[retext]: https://github.com/wooorm/retext

[processor]: https://github.com/wooorm/unified#processor

[bridge]: https://github.com/wooorm/unified#bridge

[nlcst]: https://github.com/wooorm/nlcst

[latin]: https://github.com/wooorm/parse-latin

[english]: https://github.com/wooorm/parse-english

[dutch]: https://github.com/wooorm/parse-dutch
