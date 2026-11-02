# budgetick
Frame-Budget Profiler for Parallel Game AI Algorithms

## Project summary
This project benchmarks sequential and parallel versions of three game AI workloads on real game maps. It measures how many agents, or how large a map, each parallel backend can handle inside a real-time frame budget, and it explains what limits scaling in each case.
This is a performance analysis study. No playable game, game engine, graphics or game design is built. The workloads run headless on array data. A "frame" is one call to a kernel, and the "frame budget" is a number that measured timings are compared against.
## Core research question
For each game AI workload, how many agents or how large a map can each parallel backend handle inside a real-time frame budget, and what limits its scaling: serial fraction, memory bandwidth, load imbalance, or overhead?
