# Publishing Mori

## Prerequisites

```sh
cargo install --locked --git https://github.com/l0ng-ai/openidea ide-host --bin openidea-plugin
```

## Sign and pack

```sh
openidea-plugin keygen fgbg.key          # once; keep the file secret
openidea-plugin check .
openidea-plugin pack . --key fgbg.key --publisher fgbg
shasum -a 256 fgbg.mori-0.1.0.oiplugin
```

## Release

```sh
gh release create v0.1.0 fgbg.mori-0.1.0.oiplugin \
  --repo xuhuafeifei/openidea-mori --title "Mori 0.1.0" --notes "First release."
```

## Registry

Pull request to `l0ng-ai/openidea-plugins` with two files:

`publishers/fgbg.toml`

```toml
name = "fgbg"
owners = ["xuhuafeifei"]
keys = ["<public key printed by keygen>"]
revoked-keys = []
```

`plugins/fgbg.mori.toml`

```toml
repository = "https://github.com/xuhuafeifei/openidea-mori"

[[versions]]
version = "0.1.0"
package = "https://github.com/xuhuafeifei/openidea-mori/releases/download/v0.1.0/fgbg.mori-0.1.0.oiplugin"
sha256 = "<sha-256 from shasum>"
```

CI downloads the package, checks the digest, runs `openidea-plugin check` against the latest release and
verifies the signature against the key in `publishers/fgbg.toml`.
