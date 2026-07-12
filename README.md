# kanso-json

A complete JSON decoder and encoder written in [kanso](https://kanso-lang.dev) — the first real library in the language.

```
decode "{\"a\": [1, 2.5, null]}"     // ["a": [1 2.5 (json_null true)]]
encode ["a": true "b": [1 2]]        // {"a":true,"b":[1,2]}
```

- **decode** — full JSON grammar: objects (string-keyed maps), arrays (lists), strings with every escape including `\uXXXX`, ints and floats, `true`/`false`/`null`. Failures are values: `err (parse_failure position reason)` with 1-based character positions, propagating automatically until an overload handles them.
- **encode** — canonical output: sorted object keys, compact separators, escaped strings. Encoding is dispatch: one `encode` overload per JSON-visible type.
- **must** — converts an allowed failure into a `defect` for the "this failing is a bug" case: `must (decode config)`.

The parser has no error-handling code. Kanso's auto-propagation carries every failure past the continuation functions whose patterns don't match it; the happy path is the only path written. Every design decision this library forced on the language is documented at https://kanso-lang.dev/decisions.html.

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
