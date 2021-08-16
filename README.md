# tsql-lint-action

An Action that runs [tsqllint](https://github.com/tsqllint/tsqllint) on SQL files.


## Inputs
| Parameter | Is Required | Default | Description           |
| --------- | ----------- | ------- | --------------------- |
| `tsqllint-version`    | true     | 1.11.0 | The version of tsqllint to use. |
| `path-to-sql-files`   | true     | N/A    | The path to the folder that contains the SQL files to lint. |
| `path-to-lint-config` | true     | The file located at the root of this action | The path to the tsqllint config file. |
| `file-name-filter`    | false    | N/A    | The filter to apply when choosing which files to lint. |

## Outputs
| Output        | Description                                           |
| ------------- | ----------------------------------------------------- |
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
```
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

## Example

```yml
# TODO: Fill in the correct usage
jobs:
  lint-files:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2

      - id: sql-file-folder
        shell: pwsh
        run: echo "::set-output name=folder::$($(Get-Date).Year).$($(Get-Date).Month.ToString("00"))"

      
      - name: SQL Lint
        uses: im-open/tsql-lint-action@v1.0.1
        with:
          tsqllint-version: 1.11.0
          path-to-sql-files: "Database/src/Migrations/${{ steps.sql-file-folder.outputs.folder }}"
          file-name-filter: "*.sql"
          path-to-lint-config: ./Database/src/.tsqllintrc
```


## Code of Conduct

This project has adopted the [im-open's Code of Conduct](https://github.com/im-open/.github/blob/master/CODE_OF_CONDUCT.md).

## License

Copyright &copy; 2021, Extend Health, LLC. Code released under the [MIT license](LICENSE).
