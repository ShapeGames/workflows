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
