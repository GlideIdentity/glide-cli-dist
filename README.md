# Glide CLI releases

Signed release artifacts for the `glide` CLI: archives, `checksums.txt`, cosign signatures and SBOMs. This repository holds releases only, no source code.

Primary downloads: https://dl.glideidentity.app/glide-cli/

## Verify a release

Works as written with GNU, busybox (Alpine) and macOS. Pick your archive: `darwin_all`, `linux_amd64`, `linux_arm64` or `windows_amd64.zip`.

```sh
V=$(curl -fsSLo /dev/null -w '%{url_effective}' https://github.com/GlideIdentity/glide-cli-dist/releases/latest); V=${V##*/v}
F=glide_${V}_linux_amd64.tar.gz
B=https://dl.glideidentity.app/glide-cli/v$V
curl -fsSL --fail-early -O $B/checksums.txt -O $B/checksums.txt.sig -O $B/checksums.txt.pem -O $B/$F &&
cosign verify-blob \
  --certificate checksums.txt.pem --signature checksums.txt.sig \
  --certificate-identity https://github.com/GlideIdentity/magic-auth-cli/.github/workflows/publish.yml@refs/heads/master \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  checksums.txt &&
L=$(grep "  $F\$" checksums.txt) && echo "$L" | $(command -v sha256sum || echo shasum -a 256) -c -
```
