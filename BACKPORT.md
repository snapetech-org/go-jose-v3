# v3 security compatibility backport

This separately maintained fork retains the `github.com/go-jose/go-jose/v3`
module path and parser API. It is not an official upstream v3 release. New
applications should use maintained upstream v4 or newer.

The base is upstream v3.0.5, `be2f6548701d4ba561cd61c7d4a2f1289b66c273`.
The complete CVE-2026-116587 fix is adapted from upstream commit
[a0111f82742fa92bceec9ede40845e73319e6230](https://github.com/go-jose/go-jose/commit/a0111f82742fa92bceec9ede40845e73319e6230).
The original Apache-2.0 license and source notices are retained.

The backport bounds RSA moduli, checks prime consistency before expensive
validation, rejects private JWK/EPK material before parsing embedded public
headers, and fixes general JWS unprotected-header ordering. It preserves v3's
parser signatures; v4's algorithm-allowlist API is not introduced. RSA JWKs
larger than 4096 bits are rejected by default. Applications that intentionally
need larger keys must set `MaxRSAModulusBits` before parsing begins.

Use an explicit replacement so build metadata records this fork:

```sh
go mod edit -replace=github.com/go-jose/go-jose/v3=github.com/snapetech-org/go-jose-v3/v3@v3.0.6-snape.1
go mod tidy
```

The dependency refresh requires Go 1.26 or newer. Qualification includes the
original suite, adapted upstream regressions, and the race detector. The
oversized-modulus and general-header regressions fail against the unmodified
base. Govulncheck reports no reachable or imported-package findings; the
x/crypto module's unused OpenPGP warning remains visible. This library does not
import OpenPGP. Consumers must also scan their actual application binaries.

CI uses private ephemeral Linux x64 JIT guests. Missing capacity leaves jobs
queued. Public pull requests do not execute automatically; reviewed main
revisions use the explicit private workflow. CodeQL remains disabled.
