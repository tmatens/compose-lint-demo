# compose-lint demo

A deliberately insecure `docker-compose.yml` that triggers every rule in
[compose-lint](https://github.com/tmatens/compose-lint), with a workflow that
sends the findings to GitHub Code Scanning.

Do not run the Compose file. Several of its services would give a container
control of the host.

## What it shows

- `docker-compose.yml` has six small services, each grouping related
  misconfigurations: host control, host namespaces and devices, added
  capabilities, disabled security profiles, host mounts, and a web service
  with credentials in its environment.
- `.github/workflows/compose-lint.yml` runs the compose-lint GitHub Action on
  every push to `main` and uploads a SARIF file.
- The findings appear under **Security > Code scanning**. With compose-lint
  1.5.0 the file produces 54 findings across all 28 rules.

Code scanning alerts are visible only to people with write access to a
repository. To see them yourself, fork this repository, enable Actions on the
fork, and run the workflow.

## Run it locally

```bash
pip install compose-lint
compose-lint docker-compose.yml
```

## Workflow notes

The lint step uses `continue-on-error: true` because the file is meant to
fail. A real project would leave that out so that a finding at or above
`fail-on` fails the job.
