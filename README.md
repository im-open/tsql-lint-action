# tsql-lint-action

An action that runs [tsqllint](https://github.com/tsqllint/tsqllint) on SQL files.

## Index <!-- omit in toc -->

- [tsql-lint-action](#tsql-lint-action)
  - [Inputs](#inputs)
  - [Outputs](#outputs)
    - [Example Output](#example-output)
  - [Usage Examples](#usage-examples)
  - [Contributing](#contributing)
    - [Incrementing the Version](#incrementing-the-version)
    - [Source Code Changes](#source-code-changes)
    - [Updating the README.md](#updating-the-readmemd)
  - [Code of Conduct](#code-of-conduct)
  - [License](#license)

## Inputs

| Parameter             | Is Required | Default                                     | Description                                                 |
|-----------------------|-------------|---------------------------------------------|-------------------------------------------------------------|
| `tsqllint-version`    | true        | 1.11.0                                      | The version of tsqllint to use.                             |
| `path-to-sql-files`   | true        | N/A                                         | The path to the folder that contains the SQL files to lint. |
| `path-to-lint-config` | true        | The file located at the root of this action | The path to the tsqllint config file.                       |
| `file-name-filter`    | false       | N/A                                         | The filter to apply when choosing which files to lint.      |

## Outputs

| Output        | Description                                            |
|---------------|--------------------------------------------------------|
| `lint-result` | The result returned from running the tsqllint command. |

### Example Output

**No Errors**

```
Running TSqlLint on folder "C:\Database\Scripts\*.sql"
Displaying TSqlLint Results
TSqlLint Found 0 Errors

Linted 1 files in 0.2623817 seconds

0 Errors.
0 Warnings
```

**1 Or More Errors**

```txt
Running TSqlLint on folder "C:\Database\Scripts\*.sql"
Displaying TSqlLint Results
TSqlLint Found 1 or more Errors

Write-Error:
C:\Database\Scripts\CreateTable.sql(1,40): error keyword-capitalization : Expected TSQL Keyword to be capitalized.
C:\Database\Scripts\CreateTable.sql(15,15): error semicolon-termination : Statement not terminated with semicolon.
C:\Database\Scripts\CreateSproc.sql(41,114): error keyword-capitalization : Expected TSQL Keyword to be capitalized.

Linted 2 files in 0.9998865 seconds
3 Errors.
0 Warnings
```

## Usage Examples

```yml
jobs:
  lint-files:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v3

      - id: sql-file-folder
        shell: pwsh
        run: |
          folderName=$($(Get-Date).Year).$($(Get-Date).Month.ToString("00"))
          echo "folder=$folderName" >> $GITHUB_OUTPUT

      - name: SQL Lint
        id: sql-lint
        # You may also reference the major or major.minor version
        uses: im-open/tsql-lint-action@v1.1.1
        with:
          tsqllint-version: 1.11.0
          path-to-sql-files: "Database/src/Migrations/${{ steps.sql-file-folder.outputs.folder }}"
          file-name-filter: "*.sql"
          path-to-lint-config: ./Database/src/.tsqllintrc
      
      - name: Print Lint Result
        run: echo '${{ steps.sql-lint.outputs.lint-result }}'
```

## Contributing

When creating PRs, please review the following guidelines:

- [ ] The action code does not contain sensitive information.
- [ ] At least one of the commit messages contains the appropriate `+semver:` keywords listed under [Incrementing the Version] for major and minor increments.
- [ ] The README.md has been updated with the latest version of the action.  See [Updating the README.md] for details.

### Incrementing the Version

This repo uses [git-version-lite] in its workflows to examine commit messages to determine whether to perform a major, minor or patch increment on merge if [source code] changes have been made.  The following table provides the fragment that should be included in a commit message to active different increment strategies.

| Increment Type | Commit Message Fragment                     |
|----------------|---------------------------------------------|
| major          | +semver:breaking                            |
| major          | +semver:major                               |
| minor          | +semver:feature                             |
| minor          | +semver:minor                               |
| patch          | *default increment type, no comment needed* |

### Source Code Changes

The files and directories that are considered source code are listed in the `files-with-code` and `dirs-with-code` arguments in both the [build-and-review-pr] and [increment-version-on-merge] workflows.  

If a PR contains source code changes, the README.md should be updated with the latest action version.  The [build-and-review-pr] workflow will ensure these steps are performed when they are required.  The workflow will provide instructions for completing these steps if the PR Author does not initially complete them.

If a PR consists solely of non-source code changes like changes to the `README.md` or workflows under `./.github/workflows`, version updates do not need to be performed.

### Updating the README.md

If changes are made to the action's [source code], the [usage examples] section of this file should be updated with the next version of the action.  Each instance of this action should be updated.  This helps users know what the latest tag is without having to navigate to the Tags page of the repository.  See [Incrementing the Version] for details on how to determine what the next version will be or consult the first workflow run for the PR which will also calculate the next version.

## Code of Conduct

This project has adopted the [im-open's Code of Conduct](https://github.com/im-open/.github/blob/main/CODE_OF_CONDUCT.md).

## License

Copyright &copy; 2023, Extend Health, LLC. Code released under the [MIT license](LICENSE).

<!-- Links -->
[Incrementing the Version]: #incrementing-the-version
[Updating the README.md]: #updating-the-readmemd
[source code]: #source-code-changes
[usage examples]: #usage-examples
[build-and-review-pr]: ./.github/workflows/build-and-review-pr.yml
[increment-version-on-merge]: ./.github/workflows/increment-version-on-merge.yml
[git-version-lite]: https://github.com/im-open/git-version-lite
