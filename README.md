# Public Github Action workflows

:warning: Do not commit any sensitive information like API-keys or similar :warning:

## Release rules

<table>
<tr>
<td> Commit message </td> <td> Release type </td>
</tr>
<tr>
<td>

```
fix(pencil): stop graphite breaking when too much pressure applied
```

⚠️If no pattern in found it will default to patch release ⚠️

</td>
<td>Patch Release</td>
</tr>
<tr>
<td>

```

feat(pencil): add 'graphiteWidth' option
```

</td>
<td>Minor Release</td>
</tr>
<tr>
<td>

```
perf(pencil): remove graphiteWidth option
BREAKING CHANGE: The graphiteWidth option has been removed.
The default graphite width of 10mm is always used for performance reasons.
```

</td>
<td>Major Release</td>
</tr>
</table>

## Kambi publish (`kambi-publish.yml`)

Builds and publishes the packages matched by `filter` to Kambi Artifactory, then verifies their `version` and `engines` there.

| Input / secret | Default | Description |
| --- | --- | --- |
| `ref` | _required_ | Release commit SHA that carries the new versions (not a branch name) |
| `filter` | _required_ | pnpm `--filter` selector, e.g. `./packages/engine/**` |
| `node-version` | `24.22.1` | Node.js version |
| `build-command` | `pnpm -r --filter "$FILTER" run build` | Command run before publishing |
| secret `gh_packages_token` | _required_ | GitHub Packages token (`NPM_TOKEN`), used only by `pnpm install` |
| secret `kambi_artifactory_api_key` | _required_ | Kambi Artifactory token, used only for publish and verify |

If the Kambi publish fails, re-run the failed job; do not wait for the next release.

```yaml
jobs:
  publish-kambi:
    needs: release
    uses: ShapeGames/workflows/.github/workflows/kambi-publish.yml@<commit-sha>
    with:
      ref: ${{ needs.release.outputs.sha }}
      filter: './packages/engine/**'
    secrets:
      gh_packages_token: ${{ secrets.GH_PACKAGES_TOKEN }}
      kambi_artifactory_api_key: ${{ secrets.KAMBI_ARTIFACTORY_API_KEY }}
```
