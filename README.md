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

Reusable workflow that publishes only the workspace packages matched by a pnpm `--filter` selector to Kambi Artifactory, then verifies their `version` and `engines` on Kambi. Versions that already exist on Kambi are skipped, private packages are ignored, and the job fails before publishing if a matched package sets a `publishConfig.registry` other than Kambi. If nothing matches, it logs that and succeeds.

| Input | Default | Description |
| --- | --- | --- |
| `filter` | _required_ | pnpm `--filter` selector, e.g. `./packages/engine/**` |
| `ref` | `github.sha` | Commit SHA or tag to publish from |
| `node-version` | `24.22.1` | Node.js version |
| `build-command` | `pnpm run build` | Command run before publishing |

Secrets: `gh_token`, `gh_packages_token`, `kambi_artifactory_api_key`.

```yaml
jobs:
  publish-kambi:
    uses: ShapeGames/workflows/.github/workflows/kambi-publish.yml@main
    with:
      filter: './packages/engine/**'
    secrets:
      gh_token: ${{ secrets.GITHUB_TOKEN }}
      gh_packages_token: ${{ secrets.GH_PACKAGES_TOKEN }}
      kambi_artifactory_api_key: ${{ secrets.KAMBI_ARTIFACTORY_API_KEY }}
```
