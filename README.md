# snyk-iac-code-security-checks

"Snyk IaC Security Checks" is a reusable workflow for scanning Infrastructure as Code (IaC) files using Snyk.

- It identifies security vulnerabilities, provides a detailed report, and can update pull requests with scan results.
- Integrating into your GitHub Actions workflows as part of the CI/CD pipeline is easy.
- It allows determining how to acct on venerability detection to continue or failure pipeline.
- It allows to define the level of severity to fail the pipeline.

## Usage

To use this workflow in your GitHub Actions, add the following step to your workflow file:

```yaml
- name: Snyk Infrastructure as Code scan
  uses: Gershon-A/snyk-iac-code-security-checks@v1.0.1
  with:
    # your inputs here
```

| Input                     | Description                                                       | Required | Default     |
| ------------------------- | ----------------------------------------------------------------- | -------- | ----------- |
| `SNYK_TOKEN`     | Please see  [SNYK_TOKEN](https://docs.snyk.io/getting-started/how-to-obtain-and-authenticate-with-your-snyk-api-token)| Yes       |        |
| `SNYK_ORG_ID`     | Please see  [SNYK_ORG_ID](https://docs.snyk.io/snyk-cli/scan-and-maintain-projects-using-the-cli/using-snyk-code-from-the-cli/set-the-snyk-organization-for-the-cli-tests) | Yes       |        |
| `SEVERITY_THRESHOLD`     | The severity threshold for Snyk. Only issues of this severity or higher will be reported. Allowed values `low`, `medium`, `high`, `critical` | No       | `low`       |
| `FILE`                   | The IaC file or directory to scan. Can use wildcard like `templates/*`, `example-*`  ect..                           | No       | `*` (any file) |
| `JSON_FILE_OUTPUT`       | The output file for the Snyk scan results in JSON format.         | No       | `snyk.json` |
| `update_pr_with_scan_results` | Whether to update the pull request with the scan results.      | No       | `false`      |
|       | |        |      |

Example usage:

```yaml
      - name: Snyk Infrastructure as Code scan
        id: snyk-iac-scan
        uses: Gershon-A/snyk-iac-code-security-checks@v1.0.1
        continue-on-error: true
        with:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
          SNYK_ORG_ID: ${{ secrets.SNYK_ORG_ID }}
          SEVERITY_THRESHOLD: "critical"
          FILE: "example-*"
          update_pr_with_scan_results: true

```

If `update_pr_with_scan_results` set to `true`, the action will add a comment to the PR with the scan results.  
Add permissions to the workflow to allow the action to add comments to the PR:

```yaml
permissions:
  issues: write
  pull-requests: write
```
Comment example:

![Snyk scan result comment](https://imgur.com/YTOHD9l.png)

## To Do

- [ ] Improve the PR comment with scan results. Currently, the action adds a comment to the PR with the scan results when `update_pr_with_scan_results` is set to `true`. You could enhance this feature by formatting the comment to make it easier to read, or by including additional information in the comment.
- [ ] Add support for other Snyk features. Snyk offers many features beyond IaC security scanning. You could extend your action to support other Snyk features, like container scanning or open source dependency scanning.
- [ ] Add issues exclusion.

## Future Features

- **Slack Integration**: Integrate with Slack to send scan results to a specified Slack channel.
- **Email Notifications**: Add an option to send scan results via email.
- **Customizable PR Comments**: Allow users to customize the format and content of the PR comment with scan results.
