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

## Reading a log
Every log starts with a header: the **tester that found it**, the project URL, the SHA, and the list of flaky tests that tool found in that log (with module, which tools found each one, and whether it is in IDoFT). The raw tool output follows.
- `<project_name>` is `owner_repo` (e.g. `abel533_Mapper`).
- NonDex / iDFlakies logs cover one project + SHA. When it has several modules or runs, the raw logs are joined, each starting with a `########## [i/n]` line.
- NonDex: search `nondexExecid=` (start of each execution); the execution whose id starts with `clean_` is the unshuffled run. A flaky test passes there and fails (`<<< FAILURE!`) in a shuffled one.
- iDFlakies: search `Located N tests` and the test name; an OD test passes in one order and fails in the other.
- FlakeSync: one log per test. The `concurrentfind` section runs it with no delay (passes); `delaylocs` / `deltadebug` inject delays and it fails (`<<< FAILURE!`).
- Test names too long for Windows filenames are shortened with a `~hash` suffix. The `log` column always links to the right file.

## Notes
FlakeSync: a test is counted only when its own log shows it **passing in the no-delay run** (`concurrentfind`) and **failing once delays are injected** (`delaylocs` / `deltadebug`): 45 tests. Of the 2096 tests FlakeSync's output files marked "reproduced and minimized", the log was read for each one:

| What the FlakeSync log shows | Tests | Counted? |
|---|---:|---|
| Passes with no delay, fails with delays injected | 45 | Yes |
| Passes and fails, but also fails with no delay | 369 | No - [needs verification](flakesync/flakesync_needs_verification.csv) ([logs](flakesync/logs/needs_verification)) |
| Never fails in any run | 906 | No - not flaky |
| Fails every run (mostly NullPointerException / missing services) | 561 | No - broken in this environment |
| Test never ran ("No tests to run", "Tests run: 0") | 215 | No - build/run problem |

No build failures are counted: every NonDex test passes the unshuffled run before failing a shuffled one, and every iDFlakies test passes in one order and fails in the other, so the test compiled and ran.

A test is counted only if the most recent run that checked it flagged it again and that run's raw output confirms it: NonDex = passes in the unshuffled run and fails in a shuffled run; iDFlakies = OD with one order passing and the other failing (NOD only if IDoFT already lists it as NOD). Other reported tests that could not be confirmed are **not** included.

Short SHAs from the dataset (apache/incubator-dubbo, nlighten/tomcat_exporter, tootallnate/java-websocket) are expanded to full 40-character SHAs.

Environment: OpenJDK 11.0.32.1 (Ubuntu on WSL2), Maven 3.9.12, iDFlakies 2.0.0, NonDex 2.2.1, FlakeSync 1.0-SNAPSHOT.
