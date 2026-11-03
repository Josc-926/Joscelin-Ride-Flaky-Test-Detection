# Joscelin_Lake - Flaky Test Detection Results

| | Flaky tests | New (not in IDoFT) | Logs (all) | Logs (new) |
|---|---:|---:|---|---|
| iDFlakies | [417](IDFlakies/IDFlakies_flaky_tests.csv) | [144](IDFlakies/IDFlakies_flaky_tests_new.csv) | [74 logs](IDFlakies/logs/all) | [40 logs](IDFlakies/logs/new) |
| NonDex | [2270](nondex/nondex_flaky_tests.csv) | [11](nondex/nondex_flaky_tests_new.csv) | [201 logs](nondex/logs/all) | [3 logs](nondex/logs/new) |
| FlakeSync | [45](flakesync/flakesync_flaky_tests.csv) | [1](flakesync/flakesync_flaky_tests_new.csv) | [45 logs](flakesync/logs/all) | [1 log](flakesync/logs/new) |
| Both | [2](summary/both_all.csv) | [0](summary/both_new.csv) | | |
| Unique total | [2730](summary/unique_total_all.csv) | [156](summary/unique_total_new.csv) | | |

Each number links to a CSV listing those tests: `project_url`, `sha`, `test_name`, `found_by`, `module`, `in_idoft`, and (tool CSVs) `log` = a link to that test's log. "Both" = found by two or more tools (`found_by` says which, e.g. `iDFlakies+NonDex`).

## Folder layout

```
Joscelin_Lake/
  README.md
  nondex/      nondex_flaky_tests.csv, nondex_flaky_tests_new.csv
               logs/all/<project_name>_<SHA>_nondex.log            (every confirmed flaky test)
               logs/new/<project_name>_<SHA>_nondex.log            (only projects with tests not in IDoFT)
  IDFlakies/   IDFlakies_flaky_tests.csv, IDFlakies_flaky_tests_new.csv
               logs/all/<project_name>_<SHA>_IDFlakies.log
               logs/new/<project_name>_<SHA>_IDFlakies.log
  flakesync/   flakesync_flaky_tests.csv, flakesync_flaky_tests_new.csv, flakesync_needs_verification.csv
               logs/all/<project_name>_<SHA>_<test_name>_flakesync.log
               logs/new/<project_name>_<SHA>_<test_name>_flakesync.log
               logs/needs_verification/<project_name>_<SHA>_<test_name>_flakesync.log   (not counted)
  summary/     both_*.csv, unique_total_*.csv
```


Environment: OpenJDK 11.0.32.1 (Ubuntu on WSL2), Maven 3.9.12, iDFlakies 2.0.0, NonDex 2.2.1, FlakeSync 1.0-SNAPSHOT.
