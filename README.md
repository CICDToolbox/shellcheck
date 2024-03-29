<p align="center">
    <a href="https://github.com/CICDToolbox">
        <img src="https://cdn.wolfsoftware.com/assets/images/github/organisations/cicdtoolbox/black-and-white-circle-256.png" alt="CICDToolbox Logo" />
    </a>
    <br />
    <a href="https://github.com/CICDToolbox/shellcheck/actions/workflows/pipeline.yml">
        <img src="https://img.shields.io/github/workflow/status/CICDToolbox/shellcheck/pipeline/master?style=for-the-badge" alt="Github Build Status">
    </a>
    <a href="https://github.com/CICDToolbox/shellcheck/releases/latest">
        <img src="https://img.shields.io/github/v/release/CICDToolbox/shellcheck?color=blue&label=Latest%20Release&style=for-the-badge" alt="Release">
    </a>
    <a href="https://github.com/CICDToolbox/shellcheck/releases/latest">
        <img src="https://img.shields.io/github/commits-since/CICDToolbox/shellcheck/latest.svg?color=blue&style=for-the-badge" alt="Commits since release">
    </a>
    <br />
    <a href=".github/CODE_OF_CONDUCT.md">
        <img src="https://img.shields.io/badge/Code%20of%20Conduct-blue?style=for-the-badge" />
    </a>
    <a href=".github/CONTRIBUTING.md">
        <img src="https://img.shields.io/badge/Contributing-blue?style=for-the-badge" />
    </a>
    <a href=".github/SECURITY.md">
        <img src="https://img.shields.io/badge/Report%20Security%20Concern-blue?style=for-the-badge" />
    </a>
    <a href="https://github.com/CICDToolbox/shellcheck/issues">
        <img src="https://img.shields.io/badge/Get%20Support-blue?style=for-the-badge" />
    </a>
    <br />
    <a href="https://wolfsoftware.com">
        <img src="https://img.shields.io/badge/Created%20by%20Wolf%20Software-blue?style=for-the-badge" />
    </a>
</p>

## Overview

A tool to lint your shell scripts with [ShellCheck](https://github.com/koalaman/shellcheck) in CI/CD pipelines.

This tool has been written and tested using GitHub Actions but it should work out of the box with a lot of other CI/CD tools.

## Usage

```yml
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    - name: Run Shellcheck
      run: bash <(curl -s https://raw.githubusercontent.com/CICDToolbox/shellcheck/master/pipeline.sh)
```

### Other Options

The following environment variables can be set in order to customise the script.

| Name          | Purpose | Default Value |
| ------------- | ------- | ------------- |
| EXCLUDE_FILES | A comma separated list of files to exclude from being scanned. You can also use `regex` to do pattern matching. | Unset |
| REPORT_ONLY   | Generate the report but do not fail the build even if an error occurred. | False | 
| SHOW_ERRORS   | Show the actual errors instead of just which files had errors. | True | 
| SHOW_SKIPPED  | Show which files are being skipped. | False | 

You can use any combination of the above settings.

```yml
on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    - name: Run Shellcheck
      env:
        REPORT_ONLY: true
        SHOW_ERRORS: true
      run: bash <(curl -s https://raw.githubusercontent.com/CICDToolbox/shellcheck/master/pipeline.sh)
```

### Example Output

This is an example of the output report generated by this tool, this is the actual output from the tool running against itself.

```
-------------------------------------------------------------------------- Stage 1 - Parameters --
 No parameters given
--------------------------------------------------------------- Stage 2 - Install Prerequisites --
 [  OK  ] shellcheck is alredy installed
------------------------------------------------------------- Stage 3 - Run shellcheck (v0.7.0) --
 [  OK  ] pipeline.sh
 [  OK  ] tests/advanced-tests
 [  OK  ] tests/bash.sh
 [  OK  ] tests/dash.sh
 [  OK  ] tests/ksh.sh
 [  OK  ] tests/no-extension
 [  OK  ] tests/sh.sh
------------------------------------------------------------------------------ Stage 4 - Report --
 Total: 7, OK: 7, Failed: 0, Skipped: 0
---------------------------------------------------------------------------- Stage 5 - Complete --
```

### File Identification

Shell scripts are identified using the following code:

```shell
file -b "${filename}" | grep -qE '(shell|dash) script'

AND

[[ ${filename} =~ \.(sh|bash|dash|ksh)$ ]]

```
