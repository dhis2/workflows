# DHIS2 Workflows for GitHub Actions

This repository provides a centralized location for GitHub Actions workflows that can be utilized across various DHIS2 repositories.

## Directory Structure:

-   **ci**: Solutions for Continuous Integration.
-   **automation**: Solutions for automating workflows.

---

## Consuming the Workflows:

Workflows in this repository can be utilized in other repositories by either copying them or by calling them directly if they support the `workflow_call` event.

---

### Consuming the `e2e-prod` Workflow:

The `e2e-prod` workflow provides an end-to-end testing solution, running Cypress E2E tests against various production versions of DHIS2.

#### Requirements:

To utilize this workflow:

1. In the DHIS2 repository where you wish to implement this workflow, create a new workflow file (e.g., `.github/workflows/use-e2e-prod.yml`).

2. Use the following template to call the central `e2e-prod` workflow:

    ```yaml
    name: Use e2e-prod

    on: [push, pull_request] # or any other GitHub event

    jobs:
        e2e-tests:
            uses: dhis2/workflows/.github/workflows/analytics-e2e-tests.yml@main
            secrets:
                baseurl: ${{ secrets.BASEURL }} # e.g., BASEURL = https://test.e2e.dhis2.org/analytics-dev
                username: ${{ secrets.USERNAME }}
                password: ${{ secrets.PASSWORD }}
                recordkey: ${{ secrets.RECORDKEY }}
                # Add any other required secrets
    ```

---

### Description:

The `e2e-prod` workflow facilitates Cypress E2E tests across different versions of the DHIS2 backend. Here's how it operates:

1. **Computing Production Versions**:  
   The workflow uses the [dhis2/action-supported-legacy-versions](https://github.com/dhis2/action-supported-legacy-versions 'dhis2/action-supported-legacy-versions') GitHub Action to determine which DHIS2 backend versions the app will be tested against. This ensures tests are executed against all relevant DHIS2 instances.

2. **Testing Across Versions**:  
   Runs E2E tests on various container setups for each backend version, ensuring the application's compatibility across all of them.

3. **Result Summaries**:  
   Provides a summary of the test results after execution.
