| | Flaky tests | New (not in IDoFT) |
|---|---:|---:|
| iDFlakies | [413](table_cells/idflakies_all.csv) | [140](table_cells/idflakies_new.csv) |
| NonDex | [1699](table_cells/nondex_all.csv) | [10](table_cells/nondex_new.csv) |
| FlakeSync | [1179](table_cells/flakesync_all.csv) | [1](table_cells/flakesync_new.csv) |
| Both | [226](table_cells/both_all.csv) | [0](table_cells/both_new.csv) |
| Unique total | [3065](table_cells/unique_total_all.csv) | [151](table_cells/unique_total_new.csv) |

Each number links to a CSV listing those tests (`table_cells/`): project_name, sha, module, flaky_tests, found_by, in_idoft, java_version. "Both" = found by two or more tools.

FlakeSync: 1245 reproduced by delay injection, 114 no concurrent methods, 16 hit the time limit; counted only when the failure was reproduced by delay injection and minimized (`flakesync_outcomes.csv`).

A test is counted only if the most recent run that checked it flagged it again and that run's raw output confirms it: NonDex = passes in the unshuffled run and fails in a shuffled run; iDFlakies = OD with one order passing and the other failing (NOD only if IDoFT already lists it as NOD).
1736 other reported tests need manual verification and are in `sus_flaky_tests.csv` with the reason (including tests a re-test did not reproduce).

Environment: OpenJDK 11.0.32.1 (Ubuntu on WSL2), Maven 3.9.12, iDFlakies 2.0.0, NonDex 2.2.1, FlakeSync 1.0-SNAPSHOT.
