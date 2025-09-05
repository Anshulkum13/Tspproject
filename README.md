# Tspproject
# 🗺️ Travelling Salesman City-Tour Optimizer

Optimize your city tour using geodesic math and classic TSP heuristics. This Python project reads a list of landmarks from a CSV file, calculates distances using the Haversine formula, and generates the shortest possible route using Greedy and 2-opt algorithms. The final route is exported as a GeoJSON file for easy visualization in Google Maps.

---

## 🚀 Features

- Parse CSV files with location data
- Compute geodesic distances using the Haversine formula
- Solve the Travelling Salesman Problem using:
  - Greedy nearest-neighbor heuristic
  - 2-opt optimization
- Export optimized route as GeoJSON
- Command-line interface for flexible usage
- Optional Matplotlib visualization
- Bonus: Simulated Annealing (experimental)

---

## 📁 Input Format

### `places.csv`

```csv
Name,Lat,Lon
Eiffel Tower,48.8584,2.2945
Louvre Museum,48.8606,2.3376
Notre-Dame,48.8529,2.3500
Arc de Triomphe,48.8738,2.2950
