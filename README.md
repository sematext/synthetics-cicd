# Sematext Synthetics CI/CD

This GitHub Action allows you to trigger [Sematext Synthetics](https://sematext.com/docs/synthetics/) tests during your CI/CD workflows, enabling you to validate your deployments and ensure your applications are functioning correctly before pushing to production.

Refer to our docs for a [detailed overview](https://sematext.com/docs/synthetics/ci-cd/overview/) of the features, [installation instructions](https://sematext.com/docs/synthetics/ci-cd/ci-cd-installation/) and [setup examples](https://sematext.com/docs/synthetics/ci-cd/ci-cd-installation/#examples) for different use-cases.



## Prerequisites

Before you begin, make sure you have the following:
- A Sematext Cloud account
  - A two-week free trial with no CC info required is available for new users, and it lets you try out all of our features
  - Click [here](https://apps.sematext.com/ui/registration) to register an account on our US region, or [here](https://apps.eu.sematext.com/ui/registration) for the EU region
- Some CI/CD Synthetics Monitors configured on your Sematext Cloud account
  - Setting up [Synthetics Monitors](https://sematext.com/docs/synthetics/getting-started/) is easy and takes less than 5 minutes per monitor
- A Synthetics [CI/CD Group](https://sematext.com/docs/synthetics/ci-cd/overview/#cicd-groups) set up in your Sematext Cloud account
  - A guide on setting this up can be found in the [installation instructions](https://sematext.com/docs/synthetics/ci-cd/ci-cd-installation/)
- Your Sematext Cloud account's API key (found in Settings → API), which you'll need to add as a repository secret with the name `SEMATEXT_API_KEY`
  - You can find it [here](https://apps.sematext.com/ui/account/api) if your account is registered in the US region, or [here](https://apps.eu.sematext.com/ui/account/api) for the EU region



## Usage and Examples

Add the following to your GitHub workflow file:

```yaml
steps:
  - name: Run Sematext Synthetics Tests
    uses: sematext/synthetics-cicd@v1.0.0
    with:
      MONITOR_GROUP_ID: 42                                          # Replace with your actual Monitor Group ID
      REGION: 'US'                                                  # Replace with your Sematext Cloud account's region ('EU' or 'US')
      SEMATEXT_API_KEY: ${{ secrets.SEMATEXT_API_KEY }}             # Make sure to add your Sematext API key as a repository secret first
      TARGET_URL: 'https://your-deployment-url.com'                 # Pass dynamically from your setup, used as the replacement for <DYNAMIC_URL>
      GIT_COMMIT_HASH: '5a24a0f8cd48be7f315787dcc23ad418ecdb36f2'   # Pass dynamically from your setup as needed
      USE_HEAD_SHA: false                                           # Set to true if the invoking event is linked to the commit you're testing
```

Since these are Synthetic Monitoring tests, they'll need to run on a live environment. Depending on your deployment setup, it'll most likely mean triggering these tests on a successful `deployment_status` event or a `repository_dispatch` event. For *GitHub Workflow* examples showcasing how to run tests for both of these scenarios, check out our [docs](https://sematext.com/docs/synthetics/ci-cd/ci-cd-installation/#examples).



## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `MONITOR_GROUP_ID` | The ID of the *CI/CD Group* which contains the tests (e.g., `42`) | Yes |
| `REGION` | The region where your Sematext Cloud account is registered (`EU` or `US`) | Yes |
| `SEMATEXT_API_KEY` | Your Sematext Cloud account's API key | Yes |
| `TARGET_URL` | The URL to run the Synthetics tests against - mandatory for monitors with [Dynamic URLs](https://sematext.com/docs/synthetics/ci-cd/overview/#dynamic-urls) | No |
| `GIT_COMMIT_HASH` | The commit hash that the Synthetics tests will be triggered for | No |
| `USE_HEAD_SHA` | Whether to use the HEAD SHA for the Synthetics tests | No |

> **Note**: Either `GIT_COMMIT_HASH` must be provided or `USE_HEAD_SHA` must be set to `true`.



## Outputs

| Output | Description |
|--------|-------------|
| `result` | The full result output from the Synthetics *Group Run* |
| `status` | The status of the *Group Run* (`passed` or `failed`) |
| `error` | The error message if the *Group Run* failed |
| `group_run_url` | The URL of the *Group Run* in the Sematext Cloud UI |



## Useful Resources

Here are some resources that can help you learn more about the Action, and about Sematext Synthetics in general:
- [Sematext Synthetics Overview](https://sematext.com/synthetic-monitoring/)
- [Sematext Synthetics Docs](https://sematext.com/docs/synthetics/)
- [Sematext Synthetics FAQ](https://sematext.com/docs/synthetics/faq/)
- [Sematext Synthetics CI/CD Integration Docs](https://sematext.com/docs/synthetics/ci-cd/overview/)

If you encounter issues with Sematext Synthetics or setting up the Action, just email support@sematext.com and we'll help you out ASAP.

If you're already using various workflows that create *check suites* in your repository along with a `repository_dispatch` event to trigger the Action, then please read [this page](https://sematext.com/docs/synthetics/ci-cd/ci-cd-check-run-fix/) to learn how to circumvent the limitations tied to GitHub's *check-runs* API.
