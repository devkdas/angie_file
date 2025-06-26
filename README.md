# Auto-Updating Angie Tarball Repository

## Description

This repository automatically fetches and stores the latest stable version of the Angie web server's tarball (`.tar.gz`) from the official Angie download site (`https://download.angie.software/files/`). The primary goal is to always have the most recent version readily available in this repository as `angie-latest.tar.gz`.

## How it Works

The automation is handled by a GitHub Actions workflow defined in `.github/workflows/update-angie.yml`. The workflow performs the following steps:

1.  **Fetch Latest Version Information:** It scrapes the Angie downloads page to identify the filename of the latest available tarball (e.g., `angie-1.9.1.tar.gz`).
2.  **Compare with Current Version:**
    *   It checks for a file named `.current_angie_version` in the repository. This file stores the filename of the tarball that `angie-latest.tar.gz` currently represents.
    *   It also checks if `angie-latest.tar.gz` itself exists.
3.  **Decision to Update:**
    *   If `angie-latest.tar.gz` is missing, or if the version name found on the website is different from the one stored in `.current_angie_version`, an update is triggered.
    *   If the versions match and `angie-latest.tar.gz` exists, the workflow concludes that the repository is already up-to-date, and no further action is taken.
4.  **Download and Replace:**
    *   If an update is needed, any existing `angie-latest.tar.gz` is removed.
    *   The latest tarball is downloaded from the Angie website.
    *   The downloaded file (e.g., `angie-1.9.1.tar.gz`) is renamed to `angie-latest.tar.gz`.
    *   The actual version filename (e.g., `angie-1.9.1.tar.gz`) is written into the `.current_angie_version` file.
5.  **Commit Changes:** If any files were changed (i.e., `angie-latest.tar.gz` or `.current_angie_version`), the workflow commits these changes back to the repository with a message indicating the new version.

## Files

*   **`.github/workflows/update-angie.yml`**: The GitHub Actions workflow file that automates the update process.
*   **`angie-latest.tar.gz`**: The latest available Angie tarball, downloaded from the official source. This file is a direct copy of the latest versioned tarball but is always named `angie-latest.tar.gz` for consistent access.
*   **`.current_angie_version`**: A small text file containing the actual filename of the Angie tarball that `angie-latest.tar.gz` currently points to (e.g., `angie-1.9.1.tar.gz`). This file is used by the workflow to determine if an update is necessary.

## Workflow Triggers

The workflow is configured to run:

*   **On a schedule:** Daily at midnight UTC (`0 0 * * *`).
*   **Manually:** Can be triggered at any time via the GitHub Actions tab (`workflow_dispatch`).

## Permissions

The GitHub Actions workflow requires `write` permission for repository `contents` to be able to commit the updated tarball and version file back to the repository. This is configured within the workflow file:

```yaml
permissions:
  contents: write
```

## Usage/Viewing

*   The `angie-latest.tar.gz` file in the root of this repository will always be the latest version fetched by the workflow.
*   You can check the commit history to see when updates have occurred.
*   The status of workflow runs (scheduled or manual) can be viewed under the "Actions" tab of this repository.
