# Source maintenance

Keep the original v3 API, source notices and explicit backport provenance.
`upstream` is read-only; publish focused changes only to the owned `origin` main
branch. Never rewrite published history or retag upstream releases.

Build, test and scan jobs require private ephemeral JIT guests with minimal
read permissions and restricted egress. Preserve native platform requirements;
do not add hosted routes or fallbacks. CodeQL remains disabled.

Qualify changes with the original tests and upstream security regressions, then
validate the new workflow revision on private JIT before releasing it. Keep
credentials, private machine identities and local paths out of commits,
public documentation and release text.
