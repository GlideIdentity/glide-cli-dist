# Glide CLI releases

Signed release artifacts for the `glide` CLI: archives, `checksums.txt`, cosign signatures and SBOMs. This repository holds releases only, no source code.

Primary downloads: https://dl.glideidentity.app/glide-cli/

## Verify a release

```sh
V=0.1.0
B=https://dl.glideidentity.app/glide-cli/v$V
curl -fsSLO $B/checksums.txt -O $B/checksums.txt.sig -O $B/checksums.txt.pem
cosign verify-blob \
  --certificate checksums.txt.pem --signature checksums.txt.sig \
  --certificate-identity https://github.com/GlideIdentity/magic-auth-cli/.github/workflows/publish.yml@refs/heads/master \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  checksums.txt
shasum -a 256 -c checksums.txt --ignore-missing
```
