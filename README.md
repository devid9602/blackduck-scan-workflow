# Black Duck Scan Workflow

A GitHub Actions workflow prototype for iterating over repositories listed in `repos.txt` and preparing Maven project files for a planned Black Duck scan.

## Current behavior

The workflow in `.github/workflows/blackduck-scan.yml` can be started manually or on its daily UTC schedule. It reads repository names from `repos.txt`, clones each repository with `GH_TOKEN`, copies a root-level `pom.xml` into `pom_files/` when one exists, and sets up Java 17.

**The current workflow does not run Black Duck Detect or another scanner.** Its final step only prints start and completion messages, and it does not publish a scan result or artifact. A successful Actions run is therefore not evidence that a software-composition scan took place.

## Configuration

- Add one `owner/repository` per line to `repos.txt`.
- Configure a repository secret named `GH_TOKEN` with only the access needed to clone the listed repositories.
- Review the scheduled workflow and token permissions before enabling it for private or sensitive repositories.

The current list contains `devid9602/K8S-Ansible`. The script only copies a `pom.xml` at the repository root; it does not discover nested Maven projects.

## Running it

Open **Actions → Black Duck Scan for Multiple Repositories → Run workflow** to trigger it manually. It also runs daily at midnight UTC.

## To make this a real scan

Add and configure the official Black Duck scanning action or Detect CLI, pass credentials through GitHub Secrets, fail the job on scanner errors, and retain or publish the resulting report. Pin third-party actions to reviewed versions or commit SHAs. Until those steps are implemented and verified, use this repository as a workflow scaffold only—not as a security or compliance control.
