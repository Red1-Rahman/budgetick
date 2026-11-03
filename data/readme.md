# data

maps and benchmark problems from the moving ai lab grid-based pathfinding
benchmarks: https://www.movingai.com/benchmarks/

downloaded: 2026-11-03
families used: dao, sc1, wc3
  dao = dragon age: origins, sc1 = starcraft, wc3 = warcraft iii (scaled to 512x512)

layout:
  maps/<family>/*.map           lowercased copies of the original map files
  scenarios/<family>/*.scen     lowercased copies of the original scenario files
  manifest.csv                  original name, local name, family and map size

rebuild: download the zips again from the site above, put them in temp/ and
run the one-time setup script (or unpack them by hand and lowercase the names).

citation: Sturtevant, N. (2012). Benchmarks for Grid-Based Pathfinding.
IEEE Transactions on Computational Intelligence and AI in Games.
check the site for its usage terms and for the game publisher credits.
