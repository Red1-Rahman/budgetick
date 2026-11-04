# data

Maps and benchmark problems from the [Moving AI Lab](https://www.movingai.com/) grid-based pathfinding benchmarks, used by budgetick to benchmark parallel game AI workloads.

## source

- downloaded on 2026-11-03 from https://movingai.com/benchmarks/grids.html
- the collection is also described at https://digitalcommons.du.edu/gridmaps2D
- the files in this folder are the lab's files, unchanged except that file names are converted to lowercase (see [layout](#layout))

## families used

| family | game | notes |
|---|---|---|
| `dao` | Dragon Age: Origins | |
| `sc1` | StarCraft | |
| `wc3` | Warcraft III | scaled to 512x512 |

## permission and attribution

No licence statement was found on the source pages as of 2026-11-03, so permission to redistribute the files was requested from the lab by email on 2026-11-03.

Nathan Sturtevant, who leads the Moving AI Lab (Professor, Computing Science, University of Alberta), replied on 2026-11-04:

> As long as you have clear attribution to the source of the benchmarks, it is fine to make them available with your code.

What this means for this repository:

- the map and scenario files are included here **unchanged**, with the attribution on this page
- any copy of these files must keep the credits and citation below
- the game content in the maps belongs to its original owners (BioWare Corp and Blizzard Corp)
- the reply did not mention archiving the files in a Zenodo release. this has not been confirmed with the lab

A copy of the email is kept by the repository owner.

## credits

As listed by the Moving AI Lab:

| family | credit |
|---|---|
| `dao` | Nathan R. Sturtevant and BioWare Corp. Maps extracted with permission from BioWare Corp for use and distribution as benchmark problems. |
| `sc1` | Nathan R. Sturtevant and Blizzard Corp. Converted to standard format by Dave Churchill. |
| `wc3` | Nathan R. Sturtevant and Blizzard Corp. |

## citation

Sturtevant, N. R. (2012). Benchmarks for grid-based pathfinding. *IEEE Transactions on Computational Intelligence and AI in Games*, 4(2), 144-148. https://doi.org/10.1109/TCIAIG.2012.2197681

```bibtex
@article{sturtevant2012benchmarks,
  author  = {Sturtevant, Nathan R.},
  title   = {Benchmarks for Grid-Based Pathfinding},
  journal = {IEEE Transactions on Computational Intelligence and AI in Games},
  volume  = {4},
  number  = {2},
  pages   = {144--148},
  year    = {2012},
  doi     = {10.1109/TCIAIG.2012.2197681}
}
```

## layout

```text
data/
  readme.md                  this file
  manifest.csv               one row per file
  maps/<family>/*.map        lowercased copies of the original map files
  scenarios/<family>/*.scen  lowercased copies of the original scenario files
```

`manifest.csv` columns:

| column | meaning |
|---|---|
| `family` | `dao`, `sc1` or `wc3` |
| `kind` | `map` or `scen` |
| `original_name` | file name as distributed by the lab |
| `local_name` | lowercased file name used here |
| `width`, `height` | map size in cells (maps only) |

## rebuild and verify

The files can be rebuilt from the original source, for example to check that nothing was altered.

1. download the map zips and the benchmark problem (scenario) zips for `dao`, `sc1` and `wc3` from the source above, and put them in `temp/` (the folder is not tracked in git)
2. run `python scripts/dev.py data` from the project root, or `python scripts/setup_data.py`
3. run `git status`. no changes in `data/` means the rebuilt files are identical to the committed ones

The script lowercases all file names and recreates `manifest.csv`.

## format notes

From the lab's format description, as used by budgetick.

Map files (`.map`) have a four line header (`type octile`, `height`, `width`, `map`) followed by one character per cell:

| character | meaning | treatment in budgetick |
|---|---|---|
| `.` and `G` | passable terrain | passable |
| `@` and `O` | out of bounds | blocked |
| `T` | trees | blocked |
| `S` | swamp | passable |
| `W` | water | blocked (the lab describes it as traversable, but not passable from regular terrain; treating it as blocked is a simplification, stated in the report) |

Scenario files (`.scen`) start with a version line, then one tab separated line per problem:

| column | meaning |
|---|---|
| 1 | bucket |
| 2 | map name (may contain a path such as `maps/dao/arena.map`, which budgetick ignores) |
| 3, 4 | map width and height |
| 5, 6 | start x and y |
| 7, 8 | goal x and y |
| 9 | optimal path length |

Conventions:

- `(0, 0)` is the upper left corner of the map, x is the column and y is the row
- optimal lengths assume a diagonal cost of sqrt(2) and no cutting corners through walls, so any A\* checked against them must use the same movement rules