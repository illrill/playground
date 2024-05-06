# Set commit status

Set Github commit status for a commit: https://docs.github.com/en/rest/commits/statuses. The action is developed with a focus on reflecting the workflow job's current [`job.status`](https://docs.github.com/en/actions/learn-github-actions/contexts#job-context) as a commit status. Typically, we want to set the commit status as `pending` as the first step of a workflow job, and then set it again as the last step of workflow job, using the `always()` condition so that it reports the commit status even if the job failed.

## Inputs

| Name          | Default             | Description |
| ------------- | ------------------- | ----------- |
| `token`       | `${{ github.token }}` | Github token used to set the commit status. Defaults to [GITHUB_TOKEN](https://docs.github.com/en/actions/security-guides/automatic-token-authentication). The token must have `statuses: write` permission. |
| `status`      | `pending` | Commit status to set. Must be one of `error`, `failure`, `cancelled`, `pending`, `success`. Defaults to `pending`. Status `cancelled` is translated as `failure`. |
| `repo`        | `${{ github.repository }}` | The repository to set the status for (formatted as `owner/repo`). Defaults to the current repository. |
| `sha`         | `${{ github.sha }}` | The commit SHA to set the status for. Defaults to `github.sha`. |
| `target-url`  | `${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}` | The URL to associate with this status. Defaults to the URL of the workflow run. |
| `description` | `This check has started...` | A short description of the status. The first character is automatically uppercased. |
| `context`     | `${{ github.workflow }} / ${{ github.job }}` | A string label to differentiate this status from the status of other systems. Defaults to the workflow and job name. |

## Example usage

```yaml
      - uses: actions/checkout@v4

      - name: Set initial commit status
        uses: ./.github/workflows/actions/set-commit-status

      - name: Do something else
        run: echo "I'm working"

      - name: Set final commit status based on the job status
        uses: ./.github/workflows/actions/set-commit-status
        if: always()
```
