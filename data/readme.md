# data

maps and benchmark problems from the moving ai lab grid-based pathfinding
benchmarks, downloaded from https://movingai.com/benchmarks/grids.html
(the collection is also described at https://digitalcommons.du.edu/gridmaps2D)

downloaded: 2026-11-03
families used: dao, sc1, wc3
  dao = dragon age: origins
  sc1 = starcraft
  wc3 = warcraft iii (scaled to 512x512)

credits, as listed by the moving ai lab:
  dao: Nathan R. Sturtevant and BioWare Corp. maps extracted with permission
       from BioWare Corp for use and distribution as benchmark problems.
  sc1: Nathan R. Sturtevant and Blizzard Corp. converted to standard format
       by Dave Churchill.
  wc3: Nathan R. Sturtevant and Blizzard Corp.

citation:
  Sturtevant, N. R. (2012). Benchmarks for grid-based pathfinding.
  IEEE Transactions on Computational Intelligence and AI in Games, 4(2), 144-148.
  DOI: 10.1109/TCIAIG.2012.2197681

usage terms and redistribution:
  the map and scenario files are NOT included in this repository.
  <no licence statement found on the source pages as of 2026-11-03>
  <permission to redistribute requested from the lab on YYYY-MM-DD, status: pending or granted or declined>
  if the lab grants permission, quote its wording here.

layout (after rebuilding):
  maps/<family>/*.map           lowercased copies of the original map files
  scenarios/<family>/*.scen     lowercased copies of the original scenario files
  manifest.csv                  original name, local name, family and map size
                                (this file is tracked in git, the folders above are not)

rebuild:
  1. download the map zips and the benchmark problem (scenario) zips for dao,
     sc1 and wc3 from the source above and put them in temp/
  2. run python scripts/setup_data.py from the project root
  the script lowercases all file names and recreates manifest.csv

conventions used by the scenario files (from the lab's format notes):
  - (0, 0) is the upper left corner of the map
  - optimal lengths assume sqrt(2) diagonal cost and no corner cutting through walls
  - water (W) is traversable but not passable from regular terrain; budgetick
    treats it as blocked (a simplification, stated in the report)