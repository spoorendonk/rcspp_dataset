
# Resource Constrained Shortest Path Problem Instances with Time Windows, Capacity, and Neighbourhoods

This dataset consists of instances of the Resource Constrained Shortest Path Problem (RCSPP) with time windows, vehicle capacity, and vertex neighbourhoods. 

## Instance Generation

Instances are generated as pricing problems of the root nodes when solving the Solomon instances for the Vehicle Routing Problem with Time Windows (VRPTW) using column generation.

- **Origin**: The 56 original Solomon VRPTW instances are divided into six classes:
  - **R1, R2**: Random customer distribution (short/long time windows)
  - **C1, C2**: Clustered customer distribution (short/long time windows)
  - **RC1, RC2**: Mixed random and clustered distribution (short/long time windows)

For each Solomon instance three RCSPP instances are generated based on the "neighborhood size". The name convention N$X$, where $X \in \{8,\,16,\,24\}$, denotes the maximum size of a customer's neighborhood.

### Neighborhood Construction

- neighbourhoods are dynamically grown up to the specified maximum size during the column generation process.
- At each iteration, neighbourhoods of vertices are adjusted to remove cycles, maintaining the maximum allowed size.
- When an optimal (zero-cost) path is found in the subproblem, the column generation has reached a valid lower bound and neighbourhoods get trimmed to just those vertices on the currently optimal path.
- This grow-and-trim process repeats until either:
   - The maximum neighborhood size is reached, or
   - No negative-cost path can be found (the current neighbourhoods are considered high quality).

All published instances are captured after the trimming process, within a time limit of 3600 seconds for solving the root node.

## Citing the Dataset

These instances are available as described in:

```tex
@misc{spoorendonk2025rcspp,
  author       = {Spoorendonk, Simon},
  title        = {Resource Constrained Shortest Path Problem Instances with Time Windows, Capacity, and Neighbourhoods},
  year         = {2025},
  howpublished = {\url{https://github.com/spoorendonk/rcspp_dataset}}
}
```

## File/Format Description

Each `.graph` file is in a DIMACS like format and contains the following sections:

1. **Header (p)**: Problem instance name, number of vertices, number of edges, and neighborhood size (N8, N16, or N24).
2. **Vertices (v)**: Each vertex is defined with attributes `(id, a, b, d, Q)` where:
   - `id`: Vertex ID
   - `a`: Time window start
   - `b`: Time window end
   - `d`: Demand
   - `Q`: Capacity
3. **Edges (e)**: Each edge specifies `(id, source, target, cost, time)` where:
   - `id`: Edge ID
   - `source`: Source vertex ID
   - `target`: Target vertex ID
   - `cost`: Edge cost
   - `time`: Travel time
4. **Neighbors (n)** (optional): For each vertex, a list of allowed subsequent vertices in the neighborhood. The maximum neighborhood size matches the instance type (8, 16, or 24).
5. **Comments (c)**: Comments are ignored.

