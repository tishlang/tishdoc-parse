# @tishlang/tishdoc-parse

Tish-native Markdown / TishDoc parser. Frontmatter (YAML / JSON), `meta.imports`, `:::directive`, includes, and a CommonMark/GFM-ish body grammar (headings, paragraphs, fenced code, lists ordered + unordered, blockquotes, thematic breaks, tables, links, images, italics, bold, strikethrough, code spans, autolinks, hard line breaks) → AST + diagnostics.

## Install

```bash
npm install @tishlang/tishdoc-parse
```

## Usage (Tish)

```tish
import { parseDocument, stringifyAst } from "@tishlang/tishdoc-parse"

let result = parseDocument(source, null)
let json = stringifyAst(result.ast)
```

## Exports

- `parseDocument(source, options) -> { ast, diagnostics, bodyStartLine1 }`
- `stringifyDiagnostics(diagnostics) -> string`
- `stringifyAst(ast) -> string`

`options` is optional and may include:

| Key | Type | Notes |
|---|---|---|
| `readPartial` | `(path) => string` | Resolver for `:::include{path=…}` blocks. |
| `maxIncludeDepth` | `number` | Default 16. |
| `strictMeta` | `boolean` | Promote meta validation warnings to errors. |

## AST node types (subset for README rendering)

`Document`, `Heading`, `Paragraph`, `Text`, `Emphasis` (`<strong>`), `Italic` (`<em>`), `Strikethrough`, `CodeSpan`, `CodeBlock`, `Link`, `Image`, `LineBreak`, `BlockQuote`, `ThematicBreak`, `List` (unordered), `OrderedList`, `ListItem`, `Table`, `TableCell`, `DirectiveBlock`, `DirectiveLeaf`, `Include`.

See `src/ast.tish` for builder signatures.

## Companions

- [`@tishlang/tishdoc-render-html`](https://github.com/tishlang/tishdoc-render-html) — AST → safe semantic HTML (default consumer for READMEs and docs).
- [`@tishlang/tish-syntax-highlight`](https://github.com/tishlang/tish-syntax-highlight) — fenced-code syntax highlighter used by the HTML renderer.
- [`@tishlang/tishdoc-render-typst`](https://github.com/tishlang/tishdoc-render-typst) — AST → Typst source (for PDF / print pipelines).

## Build

```bash
tish build --target js --no-optimize src/main.tish -o dist/tishdoc-parse.js
TISH_EXPORT_OUT=dist/tishdoc-parse.js \
  TISH_EXPORT_NAMES=parseDocument,stringifyDiagnostics,stringifyAst \
  node scripts/append-exports.mjs
```

The script replaces the older `tish-creator/scripts/js_append_exports.tish` so this package is now standalone.
