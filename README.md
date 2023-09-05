# DHIS2 Workflows for GitHub Actions

This repository provides a centralized location for GitHub Actions workflows that can be utilized across various DHIS2 repositories.

## Directory structure:
**ci**: Solutions for Continuous Integration.

**automation**: Solutions for automating workflows.
## Consuming the Workflows:
Workflows in this repository can be utilized in other repositories by either copying them or by calling them directly if they support the `workflow_call` event.

### Consuming the e2e-prod Workflow:
The `e2e-prod` workflow provides an end-to-end testing solution, running Cypress E2E tests against various production versions of DHIS2.

#### Requirements:
To utilize this workflow:

In the DHIS2 repository where you wish to implement this workflow, create a new workflow file (e.g., `.github/workflows/use-e2e-prod.yml`).
Use the following template to call the central `e2e-prod` workflow:

```yaml
name: Use e2e-prod

on: [push, pull_request]  # or any other GitHub event

jobs:
    e2e-tests:
        uses: dhis2/workflows/.github/workflows/analytics-e2e-tests.yml@main
        secrets:
            baseurl: ${{ secrets.BASEURL }}   # e.g. BASEURL = https://test.e2e.dhis2.org/analytics-dev
            username: ${{ secrets.USERNAME }}
            password: ${{ secrets.PASSWORD }}
            recordkey: ${{ secrets.RECORDKEY }}
            reportportal_api_key: ${{ secrets.REPORTPORTAL_API_KEY }}
            reportportal_endpoint: ${{ secrets.REPORTPORTAL_ENDPOINT }}
            reportportal_project: ${{ secrets.REPORTPORTAL_PROJECT }}
            # Add any other required secrets
```


Ensure the required secrets (`baseurl`, `username`, `password`, `recordkey`, etc.) are set in the repository settings that is consuming the workflow.
Description:
The `e2e-prod` workflow facilitates Cypress E2E tests across different versions of the DHIS2 backend. Here's how it operates:


1. **Computing Production Versions**: The workflow uses the [dhis2/action-supported-legacy-versions ](https://github.com/dhis2/action-supported-legacy-versions "dhis2/action-supported-legacy-versions ")GitHub Action to determine which DHIS2 backend versions the app will be tested against. This ensures tests are executed against all relevant DHIS2 instances.
2. Runs E2E tests on various container setups for each backend version, ensuring the application's compatibility across all of them.
3. Provides a summary of the test results.
4. Optionally, if provided with the `reportportal_api_key`, `reportportal_endpoint`, and `reportportal_project` secrets, it can send test results to the [DHIS2 Report Portal Dashboard](https://test.reports.dhis2.org/ "DHIS2 Report Portal Dashboard").