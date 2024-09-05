# Boost Security Scanner Gitlab pipe

Executes the Boost Security Scanner cli tool to scan repositories for
vulnerabilities and uploads results to the Boost Security API.

## Example

Add the following to your `.gitlab-ci.yml`:

```yml
include:
  - remote: 'https://raw.githubusercontent.com/boostsecurityio/boostsec-scanner-gitlab/main/scanner.yml'

boost-native-scanner:
  stage: build
  extends:
    - .boost_scan
  variables:
    BOOST_SCANNER_REGISTRY_MODULE: "boostsecurityio/native-scanner"
```

## Configuration

The scanner job may be configured through the use of the `variables` outlined below:

### `BOOST_CLI_ARGUMENTS` (Optional, str)

Additional CLI args to pass to the `boost` cli.

### `BOOST_API_ENABLED` (Optional, boolean string, default true)

Enable or disable boost uploading results to the boost api

### `BOOST_API_ENDPOINT` (Optional, string)

Overrides the API endpoint url

### `BOOST_API_TOKEN` (Required, string)

The Boost Security API token secret.

**NOTE**: We recommend you not put the API token directly in your pipeline
file. Instead, it should be exposed via a **secret**.

### `BOOST_CLI_VERSION` (Optional, string)

Overrides the cli version to download when performing scans. If undefined,
this will default to pulling "1".

### `BOOST_DOCKER_PROXY` (Optional, string)

Enable or disable the gitlab dependency proxy for docker images.
May be set to one of the following values:
- group: enable the group proxy
- direct: enable the direct proxy

### `BOOST_IGNORE_FAILURE` (Optional, boolean string, default false)

Ignore any non-zero exit status and always return a success.

### `BOOST_LOG_LEVEL` (Optional, string, default INFO)

Change the CLI logging level.

### `BOOST_MAIN_BRANCH` (Optional, string)

The name of the main branch that PRs would merge into. This is automatically
detected by querying the git server.

### `BOOST_PRE_SCAN_CMD` (Optional, string)

Optional command to execute prior to scanning. This may be used to generate
additional files that are not tracked in git.

### `BOOST_REGISTRY_MODULE` (Required, string)

The relative path towards a module within the [Scanner Registry](https://github.com/boostsecurityio/scanner-registry).
To streamline the configuration, both the _scanners_ prefix and _module.yaml_ suffix may be omitted.

### `BOOST_SCANNER_ID` (Optional, string)

Optional identifier to uniquely identify the scanner

### `BOOST_SCAN_LABEL` (Optional, string)

Optional identifier to identify a a monorepo component

### `BOOST_DIFF_SCAN_TIMEOUT` (Optional, number)

The optional scan timeout value, in seconds, after which the GitLab check will be marked as failed. This defaults to 120 seconds (2 minutes). Min value is 30 seconds, max value is 1800 seconds (30 minutes). For more information, please see the [source](https://github.com/boostsecurityio/boostsec-scanner-cli/blob/main/boostsec/scanner/cli/parameters/cli.py).

