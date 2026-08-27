# kaeru-vendor

Vendored Rust dependencies for [kaeru](https://github.com/LamantinAI/kaeru),
one tag per release, for building with no network access.

**This is not a fork.** There is no kaeru source here — only its dependencies.
The source lives in the main repository; this repo exists so that an offline
build does not need a 600 MB directory committed alongside it forever.

## Using it

From a kaeru checkout, on a machine that still has network:

```sh
./contrib/offline/fetch-vendor.sh
```

Then carry the whole directory to the machine that does not, and build there.
See [`docs/offline-build.md`](https://github.com/LamantinAI/kaeru/blob/main/docs/offline-build.md)
in the main repository for the full procedure, including the C++ toolchain
kaeru needs for RocksDB whether or not you are offline.

## Tags

Each tag is the complete vendor tree for the release of the same name, and
matches that release's `Cargo.lock` byte for byte. Fetch one release without
paying for the rest:

```sh
git clone --depth 1 --branch v0.7.0 \
  https://github.com/LamantinAI/kaeru-vendor.git
```

`main` carries the most recent one; the tags are what you should pin to.

## Contents of a tag

| | |
|---|---|
| `vendor/` | every crate source, including the one git dependency |
| `config.toml` | the source-replacement config, captured from `cargo vendor`'s own output |
| `Cargo.lock` | the lock this tree corresponds to |
| `.gitattributes` | `* -text` — without it Windows rewrites line endings and every checksum fails |

## Updating

Do not commit here by hand. The tree is produced by
`contrib/offline/publish-vendor.sh` in the main repository, from a clean
checkout of the tag being released. A published tag is immutable: if a vendor
tree is wrong, the fix is a new version, because someone has already built
against the old one.
