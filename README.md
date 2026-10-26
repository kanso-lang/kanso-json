# kanso-json

[![ci](https://github.com/ClayShentrup/kanso-json/actions/workflows/ci.yml/badge.svg)](https://github.com/ClayShentrup/kanso-json/actions/workflows/ci.yml)

A complete JSON decoder and encoder written in [kanso](https://kanso-lang.dev) — the first real library in the language.

```
decode "{\"a\": [1, 2.5, null]}"     // ["a": [1 2.5 json_null]]
encode ["a": true "b": [1 2]]        // {"a":true,"b":[1,2]}
```

- **decode** — full JSON grammar: objects (string-keyed maps), arrays (lists), strings with every escape including `\uXXXX`, ints and floats, `true`/`false`/`null`. Text that is not JSON answers `none`.
- **decode!** — the same parse for text you know is JSON. It answers a box holding the value, or a failure carrying `parse_failure position reason` with 1-based byte positions, which rides to the endpoint or to a rescue in another package.
- **encode** — canonical output: sorted object keys, compact separators, escaped strings. Encoding is dispatch: one `encode` overload per JSON-visible type.

The parser has no error-handling code. Kanso's auto-propagation carries every failure past the continuation functions it reaches; the happy path is the only path written. Every design decision this library forced on the language is documented at https://kanso-lang.dev/decisions.html.

## running the tests

```
git clone https://github.com/ClayShentrup/kanso && cd kanso && cargo build --release && cd ..
git clone https://github.com/ClayShentrup/kanso-json
./kanso/target/release/kanso test kanso-json/json.kso
```

16 tests: decoding every value class, escapes, unicode, whitespace, error positions, trailing-garbage detection, canonical encoding, and roundtripping.

## status

Tracks the kanso reference interpreter on main; a copy at `lib/json/json.kso` in the kanso repo runs in that project's CI as its integration suite. This repo becomes the canonical package when the kanso package manager ships.

MIT licensed.
