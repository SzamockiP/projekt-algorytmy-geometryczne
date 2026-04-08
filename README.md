# Projekt: Algorytmy Geometryczne na Siatkach Trójkątnych
*(Geometric Algorithms on Triangular Meshes)*

## Project Description

University project implementing and benchmarking two triangular mesh representations — plain lists and the Half-Edge Data Structure — for three geometric operations: vertex neighbourhood search, triangle neighbourhood search, and point location.

## Repository Structure

```
main.ipynb              ← All code (85 cells)
siatka.txt              ← Small 7-vertex test mesh
big_siatka.txt          ← Larger grid mesh
pasek.txt               ← Strip mesh
gwiazdy.txt             ← Star mesh
testy_czasowe.csv       ← Timing benchmark results
testy_pamieciowe.csv    ← Memory benchmark results
visualizations/         ← Static algorithm screenshots (JPG)
gify/                   ← Animated algorithm GIFs
```

## Requirements

- Python ≥ 3.9
- Jupyter Notebook or JupyterLab

```bash
pip install numpy pandas matplotlib scipy bitalg
```

## Running the Notebook

### CLI

```bash
jupyter notebook main.ipynb
# or
jupyter lab main.ipynb
```

### JetBrains IDE (PyCharm / IntelliJ)

Open `main.ipynb` directly; the IDE has built-in Jupyter support. Cells can be run individually with **Shift+Enter** or via **Run All**.

### Headless (script export)

```bash
jupyter nbconvert --to script main.ipynb
python main.py
```

## Implemented Algorithms

### Data Structures

| Name | Description | Cell(s) |
|---|---|---|
| Plain lists | `list[tuple]` for vertices + triangles | — |
| `HalfEdgeDataStructure` | HEDS with `HalfEdge`, `Vertex`, `Face` | 8–11 |

### Operations

| # | Name | List function | HEDS function | Cell(s) |
|---|---|---|---|---|
| 1 | Vertex neighbourhood (k layers) | `find_surrounding_points_list` | `find_surrounding_points_structure` | 24, 48 |
| 2 | Triangle neighbourhood (k layers) | `find_surrounding_triangles_list` | `find_surrounding_triangles_structure` | 30, 54 |
| 3 | Point location (walk + BFS) | `find_point_in_mesh_list` | `find_point_in_mesh_structure` | 37, 61 |

### Geometric Primitive

`orientation(a, b, c, eps=1e-12)` — signed cross-product, returns `1`/`-1`/`0` (cell 35)

## Mesh File Format

```
<n_vertices>
<n_triangles>
x0 y0
...
a0 b0 c0   ← zero-based vertex indices, CCW winding preferred
...
```

## Generating Random Meshes

Uses `scipy.spatial.Delaunay` (cell 19):

```python
generate_uniform_points(n=5000, low=0, high=1000, file_path='random_0.txt')
```

## Benchmarks Summary

Time results (`testy_czasowe.csv`): HEDS is **20–95% faster** than list-based traversal across all datasets and operations, with the largest gains on triangle neighbourhood search.

Memory results (`testy_pamieciowe.csv`): HEDS uses **85–96% less peak working memory** per operation because traversal is O(vertex degree) vs. scanning all triangles.

## Visualisation

Each operation has a `visualize_*` variant using the `bitalg` `Visualizer`. To save an animation:

```python
vis = visualize_point_in_mesh_list(points, triangles, 0, (2.7, 2.9), show_gif=True)
vis.save_gif("my_search.gif")
```

Pre-rendered outputs are in `visualizations/` (JPG) and `gify/` (GIF).

## Notes

- All comments and markdown headings are in Polish.
- `visualizer.main` refers to the `bitalg` PyPI package (see commented alternative import in cell 0).
- Random meshes `random_0.txt` / `random_1.txt` used in benchmarks are not committed; regenerate them with `generate_uniform_points()` before running benchmark cells.
