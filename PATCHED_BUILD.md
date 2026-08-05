# Patched Frogbot build — XRAY-157176

This branch exists to demonstrate the SAST differential-scan fix end to end. Two things differ from `main`:

1. `go.mod` replaces `github.com/jfrog/jfrog-cli-security` with the fix branch
   ([jfrog/jfrog-cli-security#831](https://github.com/jfrog/jfrog-cli-security/pull/831)).
2. Root `action.yml` is a composite action that builds Frogbot from this branch instead of downloading a
   released binary, so `uses: Jordanh1996/frogbot@<ref>` actually runs the patched code.

Use it from a workflow like this — note that `JF_*` must be set at **job** level, since step-level `env`
does not propagate into a composite action's steps:

```yaml
jobs:
  scan:
    runs-on: ubuntu-latest
    env:
      JF_URL: ${{ secrets.JF_URL }}
      JF_ACCESS_TOKEN: ${{ secrets.JF_ACCESS_TOKEN }}
      JF_GIT_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      JFROG_CLI_LOG_LEVEL: DEBUG
    steps:
      - uses: actions/checkout@v4
      - uses: Jordanh1996/frogbot@xray-157176-sast-differential-fix
```

Do not merge this branch anywhere. It is a reproduction harness.
