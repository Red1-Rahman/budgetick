# tech stack

Development starts on Windows and must work on Linux from any commit.
## core

| layer | choice | reason |
|---|---|---|
| language | python 3.11 or 3.12 | fastest to iterate, and numba support is reliable on these versions |
| kernels | numba (`njit`, `prange`) | compiled code, so parallel results are not distorted by interpreter overhead |
| thread backend | numba parallel threads, with static and dynamic scheduling variants | needed for the load balancing experiment |
| process backend | `multiprocessing` with `shared_memory` | needed for the threads versus processes comparison |
| arrays and data | numpy, pandas | standard tools |
| statistics | scipy | percentiles, bootstrap, significance tests |
| config | pyyaml | experiment configs |
| timing | `time.perf_counter_ns` | high resolution monotonic clock |
| system metrics | psutil | per core utilization and memory use |

## profiling

| tool | use |
|---|---|
| `cProfile` and `py-spy` | python level hot spots (py-spy runs on windows) |
| windows performance recorder and analyzer | system level cpu and thread view on windows |
| intel vtune (optional) | deeper cpu analysis, memory bound versus compute bound |
| wsl2 with `perf` (optional) | linux style profiling |
| nsight systems (gpu only) | gpu timeline and transfer cost |
| a simple stream style test | rough memory bandwidth, to explain why the stencil stops scaling |

Hardware summary (cpu model, physical and logical cores, cache sizes, memory): record with a cpu-z style tool and save it with the results. `hardware_info.py` records the same data in the metadata json.

## optional gpu

| layer | choice | reason |
|---|---|---|
| gpu kernels | numba cuda | stays in python, and the stencil is short |
| gpu metrics | nvidia-ml-py (imported as pynvml) | utilization and memory |

Only if an nvidia gpu and working driver are available. A cpu only project is complete.

## quality and automation

| tool | use |
|---|---|
| pytest | tests |
| ruff | lint and format (pep8-naming rules off, see `conventions.md`) |
| import-linter | enforce the layer rules in `architecture.md` |
| github actions | run setup, lint and tests on ubuntu and windows for every push (no benchmarks in ci, timings there are meaningless) |
| mypy (optional) | type checks if time allows |

## packaging and environment

- `pyproject.toml` for the package, with `hatchling` as the build backend and a `src` layout
- one virtual environment in `.venv`, created by `python scripts/dev.py setup`, with extras `dev` and `gpu`
- `python scripts/dev.py freeze` writes `requirements-lock-<os>.txt`, one file per operating system, so exact versions can be recreated
- `scripts/dev.py` (standard library only) runs every task. `scripts/dev.sh` and `scripts/dev.ps1` are thin launchers
- record numba, numpy and python versions in every result metadata file

## reporting

| layer | choice | reason |
|---|---|---|
| figures | matplotlib, saved as pdf or svg | vector figures required |
| methodology flowchart | svg (draw.io or mermaid export) | vector, and editable |
| report | [Tessera](https://github.com/Red1-Rahman/Tessera), with one reference style (ACM or APA7) | industry + academia standard |
| release | github repository, then a zenodo release for a doi | FAIR principles |

Out of Scope: streamlit, plotly or any dashboard.

## operating system notes

differences between windows and linux, and how the code handles them, are listed in `portability.md`. the points below are the ones that most affect measurements.

### windows and numba notes

- python on windows starts processes with spawn, not fork. entry points need `if __name__ == "__main__":`, worker functions must be importable at module level, and large arrays should go through `shared_memory` instead of being pickled
- numba compiles on first call. warmup runs and the first call's compile time are measured separately and kept out of steady state timings
- the numba threading layer (workqueue, omp or tbb) can change results, so record which one is active and set the thread count explicitly with `numba.set_num_threads`
- set the power plan to high performance, stay on AC power and close other programs before benchmark runs. record the power plan in the metadata
- windows laptops throttle under sustained load. keep short cool down gaps between configurations and watch for drift across repeats

## data

| item | choice |
|---|---|
| real maps and scenarios | Moving AI Lab grid benchmarks: dragon age: origins (`dao`), starcraft (`sc1`), warcraft iii (`wc3`) |
| synthetic maps | generated with fixed seeds, in the same `.map` format |
| usage terms and credits | `data/readme.md` |

## open decisions

- thread backend variants: static chunks versus dynamic scheduling, final design after the first scaling runs
- whether the gpu kernel or delta stepping gets built first (currently gpu not availabe)